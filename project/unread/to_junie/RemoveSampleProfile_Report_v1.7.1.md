# Vesper 수정 내역서 (Modification Report)
**버전: v1.7.1**
**최종 업데이트: 2026-03-14**

## 1. 개요
기존에 CSV 데이터를 DB로 로드하던 `sample` 프로파일과 관련 로직을 제거했습니다. 이제 Vesper 엔진은 Anvil에서 생성된 Parquet 데이터를 주 데이터원으로 사용하며, 불필요해진 ETL 러너와 메서드를 정리하여 코드 베이스를 경량화했습니다.

## 2. 주요 변경 사항

### 2.1. `sample` 프로파일 및 러너 제거
- `SampleDataRunner.java` 삭제: `data/*.csv` 파일을 스캔하여 DB에 적재하던 실행기를 제거했습니다.
- `application.yaml`에서 관련 설정이 더 이상 사용되지 않음을 확인했습니다.

### 2.2. 데이터 서비스 및 피드 정리
- `MarketDataService.java`: `importMinuteSampleData`, `importDailySampleData` 메서드를 삭제했습니다.
- `CsvDataFeed.java`: 1분봉 CSV 로딩 로직(`loadMinuteCsv`)과 관련 타임 포맷터들을 제거했습니다. 일봉 CSV 로딩(`loadDailyCsv`)은 레거시 지원을 위해 유지하되 설명을 보완했습니다.

### 2.3. 문서 및 가이드 업데이트
- `docs/commands/Vesper_Commands_v1.7.1.md`: `sample` 프로파일 실행 명령어를 제거하고 버전을 업데이트했습니다.
- `README.md`: 프로젝트 실행 섹션에서 `sample` 프로파일 관련 설명을 삭제했습니다.

## 3. 영향도 평가
- **기능**: 이제 DB에 데이터를 직접 넣고 싶다면 `backtest` 실행 전 다른 수단(SQL 등)을 사용하거나, KIS API를 통한 자동 수집 기능을 이용해야 합니다.
- **성능**: 불필요한 빈(Bean) 등록이 줄어들어 애플리케이션 시작 속도가 미세하게 향상되었습니다.
- **데이터**: `data/` 폴더의 CSV 파일들은 더 이상 엔진에 의해 자동으로 처리되지 않습니다.

## 4. 향후 계획
- Parquet 데이터 로딩 성능 최적화 지속.
- KIS API를 통한 실시간 데이터 동기화 기능 고도화.
