# Vesper 수정 내역서 (Modification Report)
**버전: v1.7.3**
**최종 업데이트: 2026-03-14**

## 1. 개요
기존에 `docs/reports/` 및 `docs/research/`에 산재해 있던 팀원(덱스) 대상 문서들을 전용 폴더인 `docs/teams/`로 통합했습니다. 이는 백테스트 성과 리포트와 시스템 관리 문서를 명확히 분리하여 관리 효율성을 높이기 위함입니다.

## 2. 주요 변경 사항

### 2.1. 팀 전용 디렉토리 신설 및 문서 이동
- `docs/teams/` 디렉토리를 생성했습니다.
- `docs/research/System_Management_Standard_for_Dex.md` -> `docs/teams/System_Management_Standard_for_Dex.md` 이동.
- `docs/reports/Standard_Integration_Report_for_Dex_v1.7.2.md` -> `docs/teams/Standard_Integration_Report_for_Dex_v1.7.2.md` 이동.

### 2.2. 문서 간 연결성 업데이트
- `README.md`: `docs/teams/` 섹션을 추가하고 관련 링크를 현행화했습니다.
- `docs/Junies_Memory_Vault.md`: 시스템 관리 표준 문서의 경로를 최신화했습니다.
- `docs/research/JUL_Collaboration_Guide.md`: 팀 문서 관리 지침을 추가했습니다.

### 2.3. 인프라 설정 및 버전 업데이트
- `.gitignore`: `docs/teams/` 하위의 마크다운 파일이 Git에 포함되도록 예외 규칙을 추가했습니다.
- `build.gradle`: 프로젝트 버전을 `1.7.3`으로 격상했습니다.
- `docs/roadmap/Vesper_Roadmap_v1.7.3.md` 및 `docs/commands/Vesper_Commands_v1.7.3.md` 업데이트.

## 3. 영향도 평가
- **문서 관리**: 덱스(Dex)와 협업 시 필요한 모든 기술 명세서가 `docs/teams/`에 모여 있어 접근성이 향상되었습니다.
- **보고서 분리**: `docs/reports/`는 이제 순수하게 백테스트 결과 및 성과 분석용으로만 사용됩니다.

## 4. 향후 계획
- `docs/teams/` 내에 덱스와의 API 연동 규격서 추가 예정.
- `docs/reports/` 하위에 백테스트 결과 자동 아카이빙 구조 고도화.
