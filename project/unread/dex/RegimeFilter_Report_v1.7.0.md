# Vesper Project 수정 내역서 (v1.7.0 Regime Filter 통합)

본 문서는 Iotzu Vesper 트레이딩 엔진 v1.7.0의 시장 레짐(Market Regime) 필터 도입 및 티커 관리 체계 개선에 대한 상세 수정 내역을 기록합니다.

---

### 1. 주요 변경 사항 요약
- **티커 분류 체계 도입**: `TargetTicker`를 `NASDAQ`, `NON_NASDAQ`, `REGIME` 카테고리로 세분화.
- **용어 및 DB 스키마 표준화**: 기존 'Symbol' 용어를 'Ticker'로 전면 교체하고, DB 테이블 및 코드 전반의 일관성을 확보.
- **시장 레짐 서비스(MarketRegimeService) 구축**: 시장 공포 및 추세를 판독하는 전용 서비스 레이어 추가.
- **전략 엔진 통합**: `VesperBreakout` 전략 진입 전 시장 상태를 검증하는 '레짐 검문소' 설치.
- **백테스트 로직 강화**: 백테스트 실행 시 레짐 분석용 데이터(QQQ, SQQQ 등)를 자동으로 로드하여 판독기에 공급.

---

### 2. 상세 수정 내역

#### A. `TargetTicker.java` (티커 엔티티 개선)
- **카테고리 Enums 추가**: `Category { NASDAQ, NON_NASDAQ, REGIME }` 도입.
- **레짐 티커 추가**: `QQQ`, `PSQ`, `SQQQ`를 `REGIME` 카테고리로 등록하여 관리.
- **메서드 확장**: `getCategory()`, `isRegimeTicker()` 메서드를 추가하여 코드 내에서 논리적 분기 처리 지원.

#### B. `MarketRegimeService.java` (신규 서비스)
- **역할**: 시장의 '공포 온도계' 역할 수행.
- **데이터 캐싱**: `ConcurrentHashMap`을 사용하여 시간대별 레짐 지표(QQQ, SQQQ 등)를 메모리에 캐싱.
- **검문 로직**: `isSafeToTrade(Instant time)` 메서드를 통해 전략 엔진이 매매 가능 여부를 판단할 수 있는 인터페이스 제공.
- **확장성**: 향후 SQQQ 급등이나 QQQ 데드크로스 판독 로직을 삽입할 수 있는 구조적 기반 마련.

#### C. `BacktestEngine.java` (백테스트 연동)
- **데이터 로딩 최적화**: 백테스트 대상 종목 실행 전, `REGIME` 티커 데이터를 DB에서 먼저 긁어와 `MarketRegimeService`에 공급(`loadMarketRegimeData`).
- **종목 필터링**: `REGIME` 티커 자체에 대한 개별 백테스트 실행은 건너뛰도록 예외 처리 추가.

#### D. `VesperBreakout.java` (전략 로직 업데이트)
- **레짐 필터 삽입**: `analyze()` 메서드 최상단에 `marketRegimeService.isSafeToTrade()` 호출문을 배치.
- **매수 차단**: 시장이 패닉 상태이거나 하락장 레짐일 경우, 기존의 모든 기술적 지표(RSI, VWAP 등) 분석을 무시하고 즉시 `Optional.empty()` 반환.

#### E. 시스템 환경 및 테스트 최적화
- **데이터 경로 표준화**: Anvil 시스템과 동일하게 `~/iotzu/data/processed`를 기본 데이터 경로로 설정 (`application.yaml` 및 `CandleRepository`).
- **테스트 메모리 확장**: 대용량 Parquet 파일 처리를 위해 Gradle 테스트 힙 메모리를 **4GB**로 상향 조정 (`build.gradle`).
- **테스트 실행 제어**: 테스트 중 불필요한 전체 백테스트 실행을 방지하기 위해 `no-run` 옵션을 도입하여 `BacktestRunner`를 제어.
- **로깅 체계 일원화**: 프로젝트 루트에 산재해 있던 로그 파일들을 `logs/` 디렉토리로 통합하고, `application.yaml` 설정을 통해 로그 롤링 정책(날짜별 분할, 30일 보관, 10MB 단위 아카이빙)을 수립했습니다.
- **문서 관리 체계 개선**: 프로젝트 루트의 마크다운 파일들을 `docs/` 하위 카테고리(`history`, `research`, `checkpoint`, `reports`)로 재배치하여 문서 접근성을 높였습니다.
- **보안 및 환경 설정 강화**: `.gitignore` 파일을 업데이트하여 로그, 시스템 파일, Python 캐시, 보안 민감 파일 등을 포함한 프로젝트 보호 설정을 강화했습니다.

---

### 3. 검증 결과
- **단위 테스트 (`MarketRegimeServiceTest`)**:
  - 티커 분류에 따른 데이터 캐싱 정상 작동 확인.
  - 비-레짐 티커 데이터 무시 로직 검증.
  - 캐시 초기화 및 조회 기능 확인.
- **통합 테스트 (`VesperApplicationTests`)**:
  - 신규 서비스 추가 후 스프링 컨텍스트 로딩 및 의존성 주입(DI) 정합성 확인.
  - **Parquet 데이터 무결성 검증**:
    - QQQ, SQQQ 등 최신 Parquet 데이터 로딩 확인 (2026-03-13 19:59분 데이터 포함).
    - NVDA, AMD 등 주요 종목의 Daily/Minute 데이터 정상 로드 확인.
- **메모리 안정성 확인**:
  - 힙 메모리 4GB 상향 후 `OutOfMemoryError` 없이 대용량 분봉 데이터(약 78만 건) 로드 성공.

---

### 4. 향후 로드맵 (Proposed)
1. **DB 관리 전환**: 현재 Enum 기반인 `TargetTicker`를 DB 테이블(`target_tickers`)로 이전하여 동적 관리 가능하도록 개선.
2. **레짐 판독 고도화**: SQQQ의 단기 변동성 폭발 감지 및 QQQ의 장기 이평선(200일선) 이탈 감지 로직 구현.
3. **Anvil ETL 연동**: Anvil에서 재생성된 QQQ, PSQ, SQQQ의 최신 Parquet 데이터를 Vesper 엔진에서 실시간(또는 주기적)으로 활용하는 파이프라인 완성.

---
**작성자**: Junie (AI Engineer)
**날짜**: 2026-03-14 (최종 업데이트)
