# Vesper Project Roadmap (v1.7.4)

본 문서는 Iotzu Vesper 트레이딩 엔진의 버전별 발전 계획과 향후 로드맵을 관리합니다.

---

## 📅 프로젝트 여정 및 단계별 계획

### Phase 1: 기반 시스템 구축 (v1.0.0 ~ v1.6.x) - [완료]
- **핵심 목표**: 기본적인 백테스트 엔진과 데이터 로더 구축.
- **주요 성과**:
  - DuckDB 기반 Parquet 데이터 로딩 시스템 구현.
  - `VesperBreakout` 전략 프로토타입 개발.
  - CSV 및 Parquet 기반 시장 데이터 공급망(ETL) 확보.
  - `PortfolioManager` 및 수익률 보고 시스템 구축.

### Phase 2: 레짐 필터 및 데이터 통합 (v1.7.0 ~ v1.7.4) - [현재 단계]
- **핵심 목표**: 하락장 방어 기전 구축 및 Anvil 시스템과의 데이터 물리적 통합.
- **주요 성과**:
  - **v1.7.0**: `MarketRegimeService` 도입 (QQQ/SQQQ), `Symbol` -> `Ticker` 전수 리팩토링, 데이터 경로 표준화.
  - **v1.7.1**: **레거시 제거 및 최적화**: `sample` 프로파일과 CSV ETL 로직(`SampleDataRunner`)을 제거하고 Anvil Parquet 중심의 데이터 체계로 전환.
  - **v1.7.2**: **시스템 통합 소통 창구 구축**: `~/iotzu/discussion/` 폴더를 신설하여 줄(JUL), 준이(Junie), 덱스(Dex) 간의 중앙 집중식 토론 환경 마련.
  - **v1.7.3**: **팀 협업 문서 재구조화**: 덱스(Dex)에게 전달할 문서를 `docs/teams/` 폴더로 분리하여 성과 보고서(`docs/reports/`)와 분리 관리.
  - **v1.7.4**: **Vortex(ML) 연동 및 팀 결속력 강화**: 덱스의 `Anvil/Vortex` 진전 사항을 Vesper에 반영하고, `ml_features/` 연동을 위한 기술적 합의 도달.
    - **기술적 합의**: Float64 정밀도 유지, Batch 데이터 주입 방식, `sophia_train_v1` 스키마 매핑.
    - **화답 보고서**: `Junie_Final_Response_to_Dex_v1.7.4.md`를 통해 덱스와의 최종 무전 및 연동 세부 사항 확정.
    - **문서 표준화**: 모든 문서의 버전 표기를 접미사(suffix)로 통일하고 링크를 일제히 정비.
  - **문서 체계화**: `docs/` 디렉토리 기반 카테고리별 문서 관리 및 버전별 수정 내역서 도입.

### Phase 3: DB 관리 및 ML 피처 통합 (v1.8.0 ~ v1.9.x) - [완료]
- **핵심 목표**: 덱스(Dex)의 ML 결과물을 수용하고 동적인 데이터 관리 체계 구축.
- **주요 성과**:
  - **Vortex FastAPI 통합**: XGBoost 네이티브 라이브러리 대신 REST API 방식으로 전환하여 안정적인 ML 추론 환경 구축.
  - **백테스트 엔진 고도화**: 2018-2025 전체 기간을 시뮬레이션하는 `BacktestEngineApplication` 구현.
  - **데이터 정밀도 확보**: Gson 기반 JSON 직렬화 및 소수점 4자리 규격화로 FastAPI 연동 완성.

### Phase 4: 실전 대응 및 시스템 고도화 (v2.0.0+) - [현재 단계]
- **핵심 목표**: 머신러닝 기반의 예측 모델과 트레이딩 엔진의 결합.
- **세부 과제**:
  - **Vortex 연동**: Vortex에서 학습된 ML 피처(`ML_FEATURES_DIR`)를 Vesper 전략에 입력값으로 활용.
  - **강화학습 도입**: 시장 상황에 따른 가중치 조절 및 리스크 프로필 최적화.
  - **실시간 매매 시스템(Live Trading)**: KIS API를 통한 실시간 주문 집행 레이어 활성화.

---

## 🛠️ 기술적 부채 및 개선 포인트 (Backlog)
- [ ] `MarketRegimeService` 내 실제 판독 알고리즘(SQQQ/QQQ 추세) 구현.
- [ ] `BacktestRunner`의 병렬 처리 성능 고도화 (CPU 코어 활용 최적화).
- [ ] PostgreSQL 기반의 실시간 성과 지표 대시보드 연동.

---

**마지막 업데이트**: 2026-03-14
**관리자**: Junie (AI Engineer) & 줄(JUL)
