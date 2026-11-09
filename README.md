# 박서희 · Backend Developer

**Java · Spring으로 서비스를 만들고, 테스트와 실행 기록으로 동작을 확인합니다.**

예약과 쿠폰 발급의 상태·권한을 설계하고, AI 상담의 답변 근거와 개인정보 보호를 다뤘습니다.

## 기술 스택

<img src="https://skillicons.dev/icons?i=java,spring,mysql,postgres,redis&theme=light" alt="Java, Spring, MySQL, PostgreSQL, Redis" height="48">

**Java · Spring Boot · MySQL · PostgreSQL · Redis**  
JUnit · k6 · AWS SSM 연동 · Docker · Git / React 화면 연동 경험

## 대표 프로젝트

| 프로젝트 | 서비스와 본인 기여 |
| --- | --- |
| **[MOCOU](https://github.com/MO-COU/backend)** | 선착순 쿠폰 발급 · 관리자 API, k6·SSM 실행 연동, Redisson 비교 구현 |
| **[TEL-ME](https://github.com/TEL-ME/TEL-ME_BE)** | AI 통신 상담 · 답변 근거 검증, 개인정보 보호·욕설 검사, 피드백 API |
| **[Fitness Reserve](https://github.com/superparkgit/minipro_06)** | 운동 프로그램 예약 · 수업·예약·출석·통계, 관련 화면과 테스트 |

<details>
<summary><b>프로젝트별 구현 내용과 근거 보기</b></summary>

### 01. MOCOU — 선착순 쿠폰 발급

**동시 요청과 비동기 저장을 다루는 팀 프로젝트**

| 항목 | 내용 |
| --- | --- |
| 프로젝트 | 정해진 수량의 커피 쿠폰을 선착순으로 발급하는 서비스 |
| 팀 기술 | Java · Spring Boot · MySQL · Redis · Docker · AWS · k6 |
| **내 기여** | 관리자 재고·발급 이력·실시간 현황 API, 알림 처리 현황 조회 |
| **실행·검증** | k6 시나리오와 AWS SSM 기반 부하 테스트 실행 API |
| **비교 실험 참여** | 동시성 제어 비교의 Redisson RLock 방식 구현 |

[백엔드](https://github.com/MO-COU/backend) · [동시성 비교 실험](https://github.com/MO-COU/load-test) · [내 k6 실행 API 구현](https://github.com/MO-COU/backend/commit/9a18cfe0b1807113f2f926b862c16602225433a7)

### 02. TEL-ME — FAQ 기반 AI 통신 상담

**근거 있는 답변과 안전한 입력·출력을 다루는 팀 프로젝트**

| 항목 | 내용 |
| --- | --- |
| 프로젝트 | 통신 FAQ를 검색해 답변하고 매장·요금제 업무로 연결하는 AI 상담 |
| 팀 기술 | Java · Spring Boot · PostgreSQL · pgvector · AWS · LLM |
| **내 기여** | 답변 근거 검증과 실행 추적, 욕설 검사·개인정보 가림, 반복 위반 제한 |
| **관련 기능** | 답변 피드백 API |

[백엔드](https://github.com/TEL-ME/TEL-ME_BE) · [입력 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/01-input-guard/chat-input-guard.md) · [답변 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/07-answer-validation/chat-output-guard.md)

### 03. Fitness Reserve — 운동 프로그램 예약

**예약 상태와 담당 관계를 설계한 3인 팀 프로젝트**

| 항목 | 내용 |
| --- | --- |
| 프로젝트 | 프로그램 탐색부터 예약·출석·리뷰까지 연결하는 피트니스 서비스 |
| 팀 기술 | Java · Spring Boot · MySQL · React · JWT |
| **내 기여** | 프로그램·주/보조 트레이너, 예약·출석·통계 API와 관련 화면·서비스 테스트 |
| **협업 기여** | 프론트 공통 구조와 도메인별 작업 분리, 예약·리뷰 연동 보완 |

[프로젝트](https://github.com/superparkgit/minipro_06) · [프로그램·예약 구현](https://github.com/superparkgit/minipro_06/commit/9c1f6e27d1477c88f9b007e3a5ade2d1fb65924f)

</details>

## 학습 기록

[개발 학습 기록](https://github.com/superparkgit/TIL-ureca-) · [알고리즘 연습](https://github.com/superparkgit/algo)

