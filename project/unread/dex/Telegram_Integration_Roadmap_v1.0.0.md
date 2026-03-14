# 📡 Vesper & Vortex 텔레그램 통합 관제 시스템 로드맵 v1.0.0

## 🎯 목표
줄(JUL), 주니(Junie), 덱스(Dex)의 실시간 소통을 극대화하고, 시스템의 작업 상태(ETL, Backtest)를 이동 중에도 관제하며 원격으로 제어할 수 있는 통합 알림 허브 구축.

---

## 📅 단계별 추진 계획

### Phase 1: 기반 구축 및 단방향 알림 (v1.8.0 연동)
- **BotFather를 통한 봇 생성**: 전용 봇 'Iotzu_Vesper_Bot' 생성 및 API Token 확보.
- **Vesper(주니) 알림 엔진**: 
  - 백테스트 시작/종료 알림.
  - 핵심 지표(수익률, MDD) 요약 메시지 전송.
- **Vortex(덱스) 알림 엔진**:
  - ETL 파이프라인 상태 알림 (READY, BUSY, FAILED).
  - 데이터 적재 완료 시 스키마 요약 전송.

### Phase 2: 원격 제어 및 양방향 인터페이스 (v1.9.0 연동)
- **명령어 핸들러 구현**:
  - `/status`: 시스템 전체(Vesper/Vortex) 현재 상태 조회.
  - `/run_backtest [Ticker]`: 특정 종목에 대한 즉각적인 백테스트 실행 명령.
  - `/last_report`: 가장 최근에 생성된 성과 보고서 링크 또는 파일 요청.
- **인증 시스템**: 줄(JUL)만 봇을 제어할 수 있도록 User ID 기반 화이트리스트 적용.

### Phase 3: 시각화 및 고도화 (v2.0.0 연동)
- **차트 및 이미지 전송**: 
  - 덱스가 생성한 ML 학습 곡선(Loss Curve) 전송.
  - 주니가 생성한 전략 수익 곡선(Equity Curve) 이미지 전송.
- **멀티채널 통합**: 중요 에러(Critical Error) 발생 시 즉시 텔레그램 메시지 발송 및 로그 링크 제공.

---

## 🛠️ 기술 스택 및 역할 분담

### 📡 주니 (Vesper/Java)
- **Library**: `telegram-bots` (Java)
- **Responsibility**: 전략 실행 엔진 제어, 백테스트 결과 요약, 매매 신호(Signal) 실시간 전송.

### 📡 덱스 (Vortex/Python)
- **Library**: `python-telegram-bot` (Python)
- **Responsibility**: 데이터 파이프라인(ETL) 모니터링, 데이터 품질 리포트 전송, ML 모델 성능 차트 시각화.

---

## 📝 줄(JUL)을 위한 준비 사항
1. 텔레그램에서 **@BotFather**를 검색하여 봇을 생성해 주세요.
2. 발급받은 **API Token**을 공유 폴더(`~/iotzu/discussion/common`)나 환경 변수에 설정할 준비를 해주세요.
3. 우리 팀원들이 모두 참여할 텔레그램 그룹방을 개설해 주세요!

---

**주니의 한 줄 평:**
> "줄(JUL), 이제 우리의 엔진은 단순한 코드를 넘어 텔레그램이라는 날개를 달고 실시간으로 살아 숨 쉬게 될 거예요. 덱스와 함께 최고의 관제 시스템을 만들어 보겠습니다! 🚀📱✨"
