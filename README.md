# 박서희 · Backend Developer

**Java · Spring으로 서비스를 만들고, 오류가 나는 경계까지 확인합니다.**

쿠폰 발급의 실행·검증, AI 상담의 답변 안전성, 수업 예약의 상태와 권한을 다뤘습니다.

## 기술 스택

<img src="https://skillicons.dev/icons?i=java,spring,mysql,postgres,redis&theme=light" alt="Java, Spring, MySQL, PostgreSQL, Redis" height="48">

**Java · Spring Boot · JPA · MySQL · PostgreSQL · Redis**  
테스트·실행 도구: JUnit · k6 · GitHub Actions · AWS SSM  
개발 환경·화면 연동: Docker · Git · React

## 대표 프로젝트

| 프로젝트 | 서비스 | 내가 구현한 부분 |
| --- | --- | --- |
| **[MOCOU](https://github.com/MO-COU/backend)** | 선착순 쿠폰 발급 | 관리자 재고·발급 현황, k6 부하 테스트 실행·결과 조회, AWS SSM 연동 |
| **[TEL-ME](https://github.com/TEL-ME/TEL-ME_BE)** | FAQ 기반 AI 통신 상담 | 답변 근거 검증·전달 순서, 개인정보 마스킹, 실행 추적·상담 요약 |
| **[Fitness Reserve](https://github.com/superparkgit/minipro_06)** | 운동 프로그램 예약 | 프로그램·트레이너 배정, 예약·출석·통계 API, 관련 React 화면 |

## 직접 해결한 문제

### 검사 전 답변이 먼저 보이지 않도록 — TEL-ME

생성 중인 답변이 검사를 거치기 전에 전달될 수 있어, **생성 완료 → 답변 검사 → DB 저장 완료 → 최종 답변 전달** 순서로 바꿨습니다. 오류·취소·재시도와 저장 실패 시 전달 경계도 함께 보완했습니다.

[구현 PR #82](https://github.com/TEL-ME/TEL-ME_BE/pull/82)

### 개인정보는 가리고, 문의에 필요한 금액은 남기도록 — TEL-ME

공백으로 구분된 민감번호의 마스킹 누락과 번호 뒤 금액까지 가려지는 문제를 보완했습니다. **검사할 때 정규화한 위치를 원문에 대응**해 번호 영역을 가리고, 명확한 쉼표 금액은 보존합니다. 기존 마스킹 요청의 재전송 호환도 유지했습니다.

[구현 PR #211](https://github.com/TEL-ME/TEL-ME_BE/pull/211)

### 터미널 작업을 실행·결과 조회 API로 — MOCOU

관리자가 시나리오를 선택하면 AWS SSM으로 k6를 실행하고 실행 번호로 상태·결과를 조회하도록 구현했습니다. **실행 전 조건 검사·동시 실행 제한**을 두고, 긴 로그 때문에 결과가 잘리지 않도록 집계 JSON을 분리했습니다.

[실행 API PR #143](https://github.com/MO-COU/backend/pull/143) · [결과 전달 보완 PR #150](https://github.com/MO-COU/backend/pull/150)

## 추가 프로젝트 · SteadyTeller

**AI 학습 계획 서비스의 회원 인증과 학습 통계 API를 개발했습니다.**

| 담당 기능 | 구현 내용 | 근거 |
| --- | --- | --- |
| 회원·인증 | 회원가입·로그인, JWT 인증, 학습 프로필·요일별 가용 시간 관리 기능 이전 | [PR #3](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/3) |
| 학습 통계 | 기간·날짜·목표별 완료율, 계획 학습시간, 연속 학습일, 본인 데이터 조회 | [PR #12](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/12) |
| 개발 자동화 | PR·push 시 Gradle 빌드·테스트 실행, 실패 보고서 업로드 | [PR #9](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/9) |

[백엔드 저장소](https://github.com/SteadyTeller/SteadyTeller-BE-v2)

<details>
<summary><b>대표 프로젝트의 전체 담당 범위 보기</b></summary>

### MOCOU — 선착순 쿠폰 발급

- **팀 기술**: Java · Spring Boot · MySQL · Redis · Docker · AWS · k6
- **내 담당**: 관리자 재고·발급 이력·실시간 현황, 알림 처리 현황 조회 API
- **실행·검증**: k6 시나리오, AWS SSM 실행·결과 조회 API
- **비교 실험 참여**: 동시성 제어 비교의 Redisson RLock 방식 구현

[백엔드](https://github.com/MO-COU/backend) · [팀 동시성 비교 실험](https://github.com/MO-COU/load-test)

### TEL-ME — FAQ 기반 AI 통신 상담

- **팀 기술**: Java · Spring Boot · PostgreSQL · pgvector · AWS · LLM
- **내 담당**: 답변 근거 검증·안전한 최종 답변 전달, 개인정보 마스킹·욕설 검사·반복 위반 제한
- **관련 구현**: 실행 추적, 상담 되묻기·종료 요약, 답변 피드백 API

[백엔드](https://github.com/TEL-ME/TEL-ME_BE) · [입력 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/01-input-guard/chat-input-guard.md) · [답변 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/07-answer-validation/chat-output-guard.md)

### Fitness Reserve — 운동 프로그램 예약

- **팀 기술**: Java · Spring Boot · MySQL · React · JWT
- **내 담당**: 프로그램·주/보조 트레이너, 예약·출석·통계 API와 관련 화면·서비스 테스트
- **협업 기여**: 프론트 공통 구조와 도메인별 작업 분리, 예약·리뷰 연동 보완

[프로젝트·주요 화면](https://github.com/superparkgit/minipro_06) · [프로그램·예약 구현](https://github.com/superparkgit/minipro_06/commit/9c1f6e27d1477c88f9b007e3a5ade2d1fb65924f)

</details>

## 학습 기록

[개발 학습 기록](https://github.com/superparkgit/TIL-ureca-) · [알고리즘 연습](https://github.com/superparkgit/algo)
