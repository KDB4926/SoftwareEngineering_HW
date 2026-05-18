# HW2 분석 메모: 온라인 설문조사 플랫폼 Design 과제

## 1. HW1에서 학습한 원래 시스템 범위

HW1의 온라인 설문조사 플랫폼은 requirement capturing 과제로, 회원과 관리자가 사용하는 비교적 큰 범위의 시스템을 대상으로 했다. 주요 기능은 다음과 같다.

- 회원 가입
- 회원 탈퇴
- 로그인 및 로그아웃
- 관리자 설문 등록, 조회, 삭제
- 회원 설문 검색
- 회원 설문 상세정보 조회 및 응답
- 회원 응답한 설문 조회, 수정, 취소
- 관리자 통계 조회

HW1에서는 기능 요구사항 목록, UI 화면, use case diagram, use case description을 작성하는 것이 핵심이었다. 특히 채점 기준상 기능 누락, use case 미표현, step-by-step description 누락, 불필요한 actor 추가, 시스템 내부 기능을 use case description에 쓰는 실수를 피해야 했다.

## 2. HW2에서 변경된 핵심 방향

HW2는 HW1 산출물을 그대로 확장하는 과제가 아니라, 수정된 요구사항과 메뉴 파일 입출력 형식에 맞춰 requirement capturing을 다시 단순화한 뒤 design 산출물을 작성하는 과제이다.

가장 중요한 변화는 다음과 같다.

- 설문은 단일 문항 객관식 설문으로 축소된다.
- 설문 등록 입력 정보는 설문 주제와 객관식 항목 수만 사용한다.
- 설문 응답은 설문 주제와 응답 항목 번호만 사용한다.
- 응답한 설문 조회는 설문 주제와 응답 항목 번호만 출력한다.
- 파일 입출력 메뉴에 없는 HW1 기능은 HW2 산출물에 넣지 않는 것이 안전하다.

따라서 HW2의 설계 기준은 HW1 전체 기능이 아니라, 다음 4개 메뉴이다.

| 메뉴 번호 | 기능 | 입력 형식 | 출력 제목 |
|---:|---|---|---|
| 1 | 설문 등록 | `1 [설문주제] [항목수]` | `1. 설문 등록` |
| 2 | 설문 응답 | `2 [설문주제] [응답항목번호]` | `2. 설문 응답` |
| 3 | 응답한 설문 조회 | `3` | `3. 응답한 설문 조회` |
| 4 | 종료 | `4` | `4. 종료` |

## 3. HW2 제출물 해석

HW2 제출물은 총 4개이다.

1. Use case diagram
2. Use case descriptions
3. Communication diagram
4. Design class diagram

Use case diagram 파일 첫 부분에는 개인코드를 명시해야 한다. 개인코드는 첫 숫자가 0이면 안 되고, 동일한 숫자가 3개 이상 포함되면 안 되며, 12345나 98765처럼 연속 증가 또는 감소하는 숫자도 피해야 한다.

## 4. HW2 기능 요구사항 정리

HW2의 기능 요구사항은 다음 4개로 정리하는 것이 가장 일관적이다.

### FR-01. 설문 등록

관리자는 설문 주제와 객관식 항목 수를 입력하여 단일 문항 객관식 설문을 등록할 수 있다.

### FR-02. 설문 응답

회원은 등록된 설문 중 하나에 대해 설문 주제와 응답 항목 번호를 입력하여 응답할 수 있다.

### FR-03. 응답한 설문 조회

회원은 자신이 응답한 모든 설문에 대해 설문 주제와 응답 항목 번호를 조회할 수 있다.

### FR-04. 종료

사용자는 종료 메뉴를 선택하여 프로그램 실행을 종료할 수 있다.

## 5. Use case diagram 기준

### Actor

- 관리자
- 회원

파일 입출력 프로그램이라는 구현 환경을 이유로 파일 시스템, 데이터베이스, 컨트롤러 같은 내부 요소를 actor로 추가하지 않는다.

### Use case

- 설문 등록
- 설문 응답
- 응답한 설문 조회
- 종료

### 연결 관계

- 관리자 → 설문 등록
- 관리자 → 종료
- 회원 → 설문 응답
- 회원 → 응답한 설문 조회
- 회원 → 종료

HW1 명세에는 include와 actor/use case generalization 금지 조건이 있었고, HW2에서도 불필요한 include, extend, generalization을 사용하지 않는 것이 안전하다.

## 6. Use case description 작성 기준

Use case description은 반드시 step-by-step으로 작성한다. 각 step은 actor action과 system response가 분리되도록 구성하는 것이 좋다.

주의할 점은 시스템 내부 구현을 설명하지 않는 것이다. 예를 들어 use case description에는 DB 저장 요청, repository 탐색, 내부 객체 생성 같은 상세 설계 문장을 쓰지 않는 편이 안전하다. 그런 내용은 communication diagram과 design class diagram에서 표현한다.

## 7. Communication diagram 설계 기준

Communication diagram은 Boundary-Control-Entity 스타일로 작성하면 HW3 구현까지 자연스럽게 이어진다.

권장 객체는 다음과 같다.

| 객체 | 스테레오타입 | 역할 |
|---|---|---|
| `:MenuBoundary` | boundary | 파일 입력을 읽고 출력 형식에 맞춰 결과를 표시한다. |
| `:SurveyController` | control | 메뉴 요청을 받아 설문 등록, 응답, 조회, 종료를 조정한다. |
| `:SurveyRepository` | entity 또는 collection | 등록된 설문 목록을 관리한다. |
| `:ResponseRepository` | entity 또는 collection | 회원이 응답한 설문 목록을 관리한다. |
| `survey:Survey` | entity | 설문 주제와 객관식 항목 수를 가진다. |
| `response:SurveyResponse` | entity | 설문 주제와 선택한 응답 항목 번호를 가진다. |

Use case description에서는 내부 설계를 피하되, communication diagram에서는 객체 간 메시지로 내부 협력을 표현한다.

## 8. Design class diagram 설계 기준

HW3 구현 가능성을 고려하면 클래스는 다음 정도로 잡는 것이 적절하다.

### `Survey`

- `topic: string`
- `optionCount: int`
- `getTopic(): string`
- `getOptionCount(): int`

### `SurveyResponse`

- `topic: string`
- `optionNumber: int`
- `getTopic(): string`
- `getOptionNumber(): int`

### `SurveyRepository`

- `surveys: List<Survey>`
- `addSurvey(topic: string, optionCount: int): void`
- `findSurvey(topic: string): Survey`

### `ResponseRepository`

- `responses: List<SurveyResponse>`
- `addResponse(topic: string, optionNumber: int): void`
- `getResponses(): List<SurveyResponse>`

### `SurveyController`

- `surveyRepository: SurveyRepository`
- `responseRepository: ResponseRepository`
- `registerSurvey(topic: string, optionCount: int): void`
- `answerSurvey(topic: string, optionNumber: int): void`
- `getResponses(): List<SurveyResponse>`
- `exit(): void`

### `MenuBoundary`

- `run(): void`
- `printRegisterSurvey(topic: string, optionCount: int): void`
- `printAnswerSurvey(topic: string, optionNumber: int): void`
- `printResponses(responses: List<SurveyResponse>): void`
- `printExit(): void`

## 9. HW2 산출물 간 일관성 원칙

네 개 제출물은 같은 용어와 같은 기능 범위를 공유해야 한다.

- Use case diagram의 use case 이름과 use case description 제목을 일치시킨다.
- Use case description의 입출력 흐름과 메뉴 파일 입출력 형식을 일치시킨다.
- Communication diagram의 메시지 이름과 design class diagram의 operation 이름을 일치시킨다.
- HW1의 상세 기능을 HW2 설계에 섞지 않는다.

권장 용어는 다음과 같다.

| 한국어 | 영문 설계 용어 |
|---|---|
| 설문 주제 | `topic` |
| 항목 수 | `optionCount` |
| 응답 항목 번호 | `optionNumber` |
| 설문 | `Survey` |
| 설문 응답 | `SurveyResponse` |
| 설문 컨트롤러 | `SurveyController` |
| 메뉴 경계 객체 | `MenuBoundary` |

## 10. 최종 판단

HW2의 핵심은 기능을 많이 넣는 것이 아니라, 수정된 명세와 파일 입출력 형식에 맞게 단순하고 일관적인 설계를 만드는 것이다. 따라서 제출물은 설문 등록, 설문 응답, 응답한 설문 조회, 종료 4개 기능만 중심에 두고 작성하는 것이 가장 안전하다.
