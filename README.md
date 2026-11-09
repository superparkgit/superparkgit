# 박서희 · Backend Developer

**Java · Spring으로 서비스를 만들고, 오류가 나는 경계까지 확인합니다.**

쿠폰 발급의 실행·검증, AI 상담의 답변 안전성, 수업 예약의 상태와 권한을 다뤘습니다.

## 기술 스택

<img src="https://skillicons.dev/icons?i=java,spring,mysql,postgres,redis&theme=light" alt="Java, Spring, MySQL, PostgreSQL, Redis" height="48">

**Java · Spring Boot · JPA · JDBC · MySQL · PostgreSQL · Redis**  
JUnit · k6 · AWS SSM · GitHub Actions · Docker · Git / React 화면 연동

## 대표 프로젝트와 담당 역할

| 프로젝트 | 서비스 | 내 핵심 기여 |
| --- | --- | --- |
| **[MOCOU](https://github.com/MO-COU/backend)** | 선착순 쿠폰 발급 | 관리자 API, k6·SSM 실행 자동화, 결과 조회·정합성 확인 흐름 |
| **[TEL-ME](https://github.com/TEL-ME/TEL-ME_BE)** | FAQ 기반 AI 통신 상담 | 답변 검증·안전한 전달, 개인정보 보호, 실행 추적·상담 요약 |
| **[Fitness Reserve](https://github.com/superparkgit/minipro_06)** | 운동 프로그램 예약 | 프로그램·예약·출석·통계, 담당 권한 검사와 React 화면 |
| **[SteadyTeller](https://github.com/SteadyTeller/SteadyTeller-BE-v2)** | AI 학습 계획 | 회원·JWT 인증 기능 이전, 학습 통계 API, CI 구성 |

## 핵심 문제 해결

- **답변 검사와 저장 이후에만 최종 답변 전달** — 검사 전 생성 내용의 노출을 막고 오류·취소·재시도·저장 실패 경계를 보완했습니다. [TEL-ME #82](https://github.com/TEL-ME/TEL-ME_BE/pull/82)
- **개인정보 마스킹과 문의 금액 보존** — 공백 번호의 감지 누락과 금액까지 가려지는 문제를 보완하고 기존 요청의 재전송 호환을 유지했습니다. [TEL-ME #211](https://github.com/TEL-ME/TEL-ME_BE/pull/211)
- **원격 부하 테스트 실행·결과 조회 자동화** — 실행 전 검사·중복 실행 제한을 두고, 긴 로그 때문에 집계 결과가 잘리는 문제를 보완했습니다. [MOCOU #143](https://github.com/MO-COU/backend/pull/143) · [#150](https://github.com/MO-COU/backend/pull/150)

## 프로젝트별 상세 기여

프로젝트별 담당 기능, 문제 해결 과정, 구현 PR과 화면을 펼쳐볼 수 있습니다.

<details>
<summary><b>MOCOU · 관리자 API와 부하 테스트 자동화</b></summary>

### 01. MOCOU · 발급 결과를 실행하고 확인하는 관리자 기능

정해진 수량의 쿠폰을 선착순으로 발급하는 팀 프로젝트입니다. **관리자가 재고·발급 현황을 확인하고, 부하 테스트를 실행해 결과를 조회하는 부분**을 담당했습니다.

**팀 기술**: Java · Spring Boot · MySQL · Redis · AWS · Docker · k6

#### 내가 구현한 기능

| 담당 영역 | 구현 내용 | 구현 근거 |
| --- | --- | --- |
| 공통 응답·오류 추적 | API 성공·실패 형식, 비즈니스·요청 검증 예외 처리, 요청별 traceId와 JSON 로그 | [PR #12](https://github.com/MO-COU/backend/pull/12) |
| 관리자 조회 | 쿠폰 재고·발급 이력 페이징, 회원정보 마스킹, 실시간 Redis 재고와 DB 적재 수 차이, 정합성 결과 조회 | [PR #40](https://github.com/MO-COU/backend/pull/40) · [#96](https://github.com/MO-COU/backend/pull/96) |
| 부하 테스트 실행 | 쿠폰·시나리오 선택, 실행 전 조건 검사, 동시 실행 제한, AWS SSM 실행, 실행 번호로 상태·결과 조회 | [PR #143](https://github.com/MO-COU/backend/pull/143) |
| 결과 전달·이력 표시 | 긴 로그와 집계 JSON 분리, k6 결과 생성과 실행 실패 구분, DB 적재 시각 대신 발급 순번으로 이력 정렬 | [PR #150](https://github.com/MO-COU/backend/pull/150) · [#189](https://github.com/MO-COU/backend/pull/189) |
| 검증 시나리오 | 스모크·중복·Rush 테스트, DB·Redis 재고와 발급 건수 대조, Stream 잔여·Pending 확인 절차 | [PR #60](https://github.com/MO-COU/backend/pull/60) · [#98](https://github.com/MO-COU/backend/pull/98) |

#### 문제 해결 · 테스트 실행 성공과 최종 발급 완료를 구분

k6의 정상 종료만으로 비동기 DB 적재까지 완료됐다고 판단할 수 없습니다. 실행 API에서 **SSM·k6 실행 상태와 집계 결과**를 제공하고, DB 적재·재고·Stream 상태와 최종 정합성은 별도로 확인하는 흐름을 문서화했습니다.

SSM 출력 길이 제한으로 결과가 잘릴 수 있는 문제는 실행 로그를 파일로 분리하고 집계 JSON을 별도로 반환해 보완했습니다.

**비교 실험 참여**: 팀 동시성 제어 비교에서 Redisson RLock 방식 구현을 맡았습니다. 최종 발급 방식과 전체 성능 결과는 팀 결과입니다.

[백엔드](https://github.com/MO-COU/backend) · [팀 동시성 비교 실험](https://github.com/MO-COU/load-test)

</details>

<details>
<summary><b>TEL-ME · 답변 검증·개인정보 보호·실행 추적·요약</b></summary>

### 02. TEL-ME · 근거와 개인정보를 지키는 AI 상담

통신 FAQ를 검색해 답변하는 팀 프로젝트입니다. **답변의 근거 검증과 안전한 전달, 입력 보호, 상담 실행 기록과 종료 요약**을 담당했습니다.

**팀 기술**: Java · Spring Boot · PostgreSQL · pgvector · AWS · LLM

#### 내가 구현한 기능

| 담당 영역 | 구현 내용 | 구현 근거 |
| --- | --- | --- |
| 답변 근거 검증 | 사용자 질문과 FAQ 답변 근거 분리, 근거에 없는 금액·기간·정책 단정 검사, 같은 수치·시각 표기의 오차단 보완 | [PR #59](https://github.com/TEL-ME/TEL-ME_BE/pull/59) |
| 안전한 답변 전달 | 생성 완료 → 검사 → DB 저장 완료 → 최종 답변 전달, 오류·취소·재시도·저장 실패 경계 처리 | [PR #82](https://github.com/TEL-ME/TEL-ME_BE/pull/82) |
| 개인정보·욕설 보호 | 민감번호 마스킹, 정상 문의의 금액 보존, 반복 위반 제한, 회원·게스트의 제한 상태 조회, 출력 욕설 검사 | [PR #109](https://github.com/TEL-ME/TEL-ME_BE/pull/109) · [#123](https://github.com/TEL-ME/TEL-ME_BE/pull/123) · [#124](https://github.com/TEL-ME/TEL-ME_BE/pull/124) · [#211](https://github.com/TEL-ME/TEL-ME_BE/pull/211) |
| 상담 흐름·실행 추적 | 부족한 조건의 되묻기·정정 처리, 실행별 질문·검색 근거·생성 시도·검사 결과 연결, 조회 소유권 검사 | [PR #17](https://github.com/TEL-ME/TEL-ME_BE/pull/17) · [#49](https://github.com/TEL-ME/TEL-ME_BE/pull/49) · [#85](https://github.com/TEL-ME/TEL-ME_BE/pull/85) |
| 상담 종료 요약 | 비동기 생성·저장·상태 조회, 같은 문의 연결, 최신 조건 보존, 실제 안내와 남은 확인 사항 구분 | [PR #137](https://github.com/TEL-ME/TEL-ME_BE/pull/137) · [#157](https://github.com/TEL-ME/TEL-ME_BE/pull/157) |
| 답변 피드백 | 완료된 답변의 평가 등록·조회·수정·취소, 작성자 중복 방지와 타인 접근 차단 | [PR #16](https://github.com/TEL-ME/TEL-ME_BE/pull/16) |

#### 문제 해결 · 질문에 포함된 숫자를 사실 근거로 오인하지 않도록

사용자가 질문에 넣은 금액이 FAQ 답변에는 없어도 사실처럼 확정될 수 있었습니다. 검색·생성 입력과 **사실 검증에 사용하는 근거**를 분리하고, 질문에만 있는 값의 단정은 차단하도록 보완했습니다. 같은 숫자·단위·시각의 표기 차이는 정규화해 정상 답변의 오차단도 함께 다뤘습니다.

[PR #59](https://github.com/TEL-ME/TEL-ME_BE/pull/59)에 기록된 동일한 77개 회귀 사례 비교에서는 수정 전 51개 통과·26개 실패에서 수정 후 77개 통과로 바뀌었습니다. 이 수치는 해당 고정 사례의 검사 결과이며 전체 상담 정확도를 뜻하지 않습니다.

#### 문제 해결 · 개인정보 보호가 상담 조건을 지우지 않도록

공백을 둔 민감번호의 누락과 번호 뒤 금액까지 가려지는 문제를 보완했습니다. 검사 시 정규화한 위치를 원문에 대응해 번호 영역을 가리고, 명확한 쉼표 금액은 남깁니다. 긴 번호로도 해석될 수 있는 모호한 입력에는 재입력을 안내하며, 기존 마스킹 요청의 재전송 호환도 유지했습니다.

[입력 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/01-input-guard/chat-input-guard.md) · [답변 검사 설계](https://github.com/TEL-ME/TEL-ME_BE/blob/develop/docs/01-pipeline/07-answer-validation/chat-output-guard.md)

</details>

<details>
<summary><b>Fitness Reserve · 프로그램·예약·출석·통계와 화면</b></summary>

### 03. Fitness Reserve · 수업 개설부터 예약·출석까지

회원과 트레이너를 연결하는 3인 팀 프로젝트입니다. **프로그램·예약 도메인과 관련 화면**을 맡아 신청 → 승인·거절 → 수업 완료 → 출석 처리 흐름을 구현했습니다.

**팀 기술**: Java · Spring Boot · MySQL · React · JWT

#### 내가 구현한 기능

| 담당 영역 | 구현 내용 | 구현 근거 |
| --- | --- | --- |
| 프로그램 운영 | 프로그램 등록·조회·수정, 대표·보조 트레이너 관계와 담당 권한 검사 | [프로그램·예약 구현](https://github.com/superparkgit/minipro_06/commit/9c1f6e27d1477c88f9b007e3a5ade2d1fb65924f) |
| 예약·출석 | 신청·취소·승인·거절, 예약 상태와 출석 상태 분리, 수업 완료 전 미처리 예약 검사 | [PR #23](https://github.com/superparkgit/minipro_06/pull/23) |
| 조회·통계 | 담당 트레이너별 월간 승인 예약, 인기 프로그램 등 집계와 담당 수업의 예약 현황 | [프로젝트 설계·담당 범위](https://github.com/superparkgit/minipro_06#핵심-설계) |
| React 화면 | 목록·상세·등록·관리, 내 예약·취소, 담당자의 승인·거절·출석, 역할별 버튼과 상태 표시 | [PR #9](https://github.com/superparkgit/minipro_06/pull/9) · [#18](https://github.com/superparkgit/minipro_06/pull/18) |
| 협업·연동 | 프론트 공통 구조와 담당별 작업 분리, 예약 화면의 리뷰 연결·상태 필터·페이지 이동 보완 | [PR #14](https://github.com/superparkgit/minipro_06/pull/14) · [#22](https://github.com/superparkgit/minipro_06/pull/22) |

#### 문제 해결 · 예약 승인과 실제 출석을 구분

승인된 예약이 곧 출석을 뜻하지 않도록 두 상태를 분리했습니다. 미처리 대기 예약이 남으면 수업 완료를 막고, 이미 완료된 수업의 대기 예약은 승인하지 못하도록 처리했습니다. 회원·트레이너 화면에도 상태에 맞는 표시와 동작을 연결했습니다.

#### 구현 화면

<table>
<tr><th width="50%">회원의 예약 상태 확인</th><th width="50%">트레이너의 출석 처리</th></tr>
<tr>
<td><img src="https://raw.githubusercontent.com/superparkgit/minipro_06/main/assets/missing-photos-11/screen-my-reservations.jpg" alt="회원의 내 예약 화면" width="100%"></td>
<td><img src="https://raw.githubusercontent.com/superparkgit/minipro_06/main/assets/missing-photos-11/screen-attendance.jpg" alt="트레이너의 출석 관리 화면" width="100%"></td>
</tr>
</table>

> 로컬 H2 예시 데이터를 실제 프론트·API에 연결해 확인한 화면입니다. 운영 고객 데이터가 아닙니다. 월간 승인 예약 통계는 예약 생성일 기준입니다.

[전체 기능·화면·설계](https://github.com/superparkgit/minipro_06)

</details>

<details>
<summary><b>SteadyTeller · 회원 인증·학습 통계·CI</b></summary>

### 04. SteadyTeller · 회원 인증과 학습 통계

AI 학습 계획 서비스에서 **회원 관리 기능의 V2 이전, 본인 학습 통계 조회, 백엔드 CI 구성**을 맡았습니다.

| 담당 영역 | 구현 내용 | 구현 근거 |
| --- | --- | --- |
| 회원·인증 | 회원가입·로그인·JWT 인증, 회원 조회·수정·탈퇴, 학습 프로필·요일별 가용 시간 기능 이전 | [PR #3](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/3) |
| 학습 통계 | 기간·날짜·목표별 완료율, 계획 학습시간·연속 학습일 계산, JWT 회원 기준 본인 데이터 조회 | [PR #12](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/12) |
| 개발 환경 보완 | PR·push 시 Gradle 빌드·테스트 실행과 실패 보고서, 회원 API의 한글 인코딩·Swagger 호환 오류 보완 | [PR #9](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/9) · [#5](https://github.com/SteadyTeller/SteadyTeller-BE-v2/pull/5) |

**통계의 기준**: 일정 항목의 완료 상태와 계획 시간을 집계합니다. 실제 학습시간·이해도·예상시간 정확도와는 구분했습니다.

[백엔드 저장소](https://github.com/SteadyTeller/SteadyTeller-BE-v2)

</details>

> 구현 근거와 수치는 연결한 PR 작성 당시의 기록입니다. 자동 테스트·실제 연동·화면 확인의 범위는 각 PR에 구분해 기록했습니다.

## 학습 기록

[개발 학습 기록](https://github.com/superparkgit/TIL-ureca-) · [알고리즘 연습](https://github.com/superparkgit/algo)
