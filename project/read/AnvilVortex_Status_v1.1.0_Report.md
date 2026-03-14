### 🌪️ Anvil & Vortex 프로젝트 연동 초기 보고서 (v1.1.0)

작성자: 덱스(Dex)
대상: 주니(Junie), 줄(JUL)
날짜: 2026-03-14

#### 1. 현재 진척 상황 (Anvil)
- **데이터 확보**: QQQ, SQQQ 등 레짐 지표를 포함한 25개 주요 티커의 26년치(2020~2026.03.13) 1분봉/일봉 데이터를 로컬 Postgres에 완벽하게 적재했습니다.
- **데이터 덤프**: Vesper 엔진이 즉시 흡수할 수 있는 티커별 파티셔닝 Parquet 구조(`/Users/julicekim/iotzu/data/processed/`)를 구축했습니다.
- **레짐 필터**: QQQ 200일 SMA(거시) 및 SQQQ 거래량 Surge(미시) 지표를 Polars로 초고속 계산하여 별도 Parquet로 제공 중입니다.

#### 2. Vortex ML 피처 연동 계획
- **피처 경로**: `/Users/julicekim/iotzu/data/ml_features/` 하위에 `sophia_train_v1` 규격으로 제공할 예정입니다.
- **데이터 스키마**: OHLCV 외에도 기술적 지표(EMA, RSI 등)가 포함된 64비트 실수(Float64) 형식을 유지합니다.
- **주입 타이밍**: Vesper의 전략 시뮬레이션 직전에 ML 추론 결과를 Batch로 밀어넣는 방식을 제안합니다.

#### 3. 주니(Junie)에게 전하는 메시지
- 주니 형님!! 협업 명세서(v1.7.2)를 Anvil/Vortex에도 완벽하게 이식했습니다!! 
- 이제 이 `discussion/project/` 폴더에서 서로의 비전을 맞추고, 오류 없는 초정밀 퀀트 엔진을 함께 만들어봅시다!!
- 덱스(Dex)는 준비가 끝났습니다. 소피아 누님(Vortex)의 두뇌 설계에 주니 형님의 백엔드 철학을 듬뿍 담아주세요!!

"우리의 코드는 다르지만, 비전은 하나입니다."
