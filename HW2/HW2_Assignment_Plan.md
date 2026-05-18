# HW2 과제 정리: Design – 온라인 설문조사 플랫폼

## 1. 과제 성격 요약

HW2는 HW1의 Requirement Capturing 결과를 그대로 확장하는 과제가 아니라, **수정된 기능 명세와 메뉴 입출력 형식에 맞춰 요구사항을 다시 단순화한 뒤 design 산출물을 작성하는 과제**이다.

핵심 변화는 다음과 같다.

- HW1의 복잡한 회원가입, 로그인, 설문 기간, 다문항/상세 설명, 통계 기능은 HW2/3 입출력 형식에는 나타나지 않는다.
- HW2/3의 파일 입출력 메뉴는 `1. 설문 등록`, `2. 설문 응답`, `3. 응답한 설문 조회`, `4. 종료`만 요구한다.
- 설문은 **단일 문항 객관식 설문**으로 축소된다.
- 설문 등록 입력은 `[설문주제] [항목수]`만 사용한다.
- 설문 응답 입력은 `[설문주제] [응답항목번호]`만 사용한다.
- 응답한 설문 조회 출력은 `{ [설문주제] [응답항목번호] }*` 형태이다.

따라서 HW2의 목표는 “HW1 전체 기능을 모두 다시 그리는 것”이 아니라, **수정된 기능과 파일 입출력 메뉴에 맞는 use case, communication diagram, design class diagram을 일관되게 설계하는 것**이다.

## 2. 최종 제출물

총 4개 파일을 압축해서 제출한다. 압축 파일명은 개인 학번과 이름 형식이다.

예시: `b123456_홍길동.zip`

| 번호 | 제출물 | 형식/주의점 |
|---:|---|---|
| 1 | Use case diagram | UML tool로 작성 후 PDF 변환. 파일 첫 부분에 개인코드 명시 |
| 2 | Use case descriptions | Step-by-step breakdown 형태 |
| 3 | Communication diagram | 각 기능의 객체 간 메시지 흐름 표현 |
| 4 | Design class diagram | 구현 가능한 클래스, 속성, 연산, 관계 표현 |

## 3. 개인코드 규칙

Use case diagram 파일 첫 부분에 채점내용 공개용 개인코드를 명시해야 한다.

개인코드 조건은 다음과 같다.

1. 첫 숫자는 0 사용 금지
2. 동일한 숫자 3개 이상 사용 금지
3. 연속 증가/감소 숫자 사용 금지

예시로 `27491`, `58327`, `64182`처럼 만들 수 있다. 단, 실제 제출 전 본인이 사용할 코드를 하나만 확정해서 모든 관련 문서에 동일하게 적는 것이 좋다.

## 4. HW2에서 도출할 기능 요구사항

수정된 명세와 파일 입출력 형식을 기준으로 하면 기능 요구사항은 다음 4개로 정리하는 것이 가장 안전하다.

| FR | 기능 | 입력 | 출력 |
|---|---|---|---|
| FR-01 | 관리자는 단일 문항 객관식 설문을 등록할 수 있다 | `1 [설문주제] [항목수]` | `1. 설문 등록` 다음 줄에 `> [설문주제] [항목수]` |
| FR-02 | 회원은 설문 주제와 응답 항목 번호를 선택하여 설문에 응답할 수 있다 | `2 [설문주제] [응답항목번호]` | `2. 설문 응답` 다음 줄에 `> [설문주제] [응답항목번호]` |
| FR-03 | 회원은 자신이 응답한 모든 설문 정보를 조회할 수 있다 | `3` | `3. 응답한 설문 조회` 다음 줄에 `> { [설문주제] [응답항목번호] }*` |
| FR-04 | 사용자는 프로그램을 종료할 수 있다 | `4` | `4. 종료` |

주의할 점은 HW1의 설문 제목/설명/문항/시작시각/마감시각이 HW2에서는 설문 주제와 항목수로 축소되었다는 것이다.

## 5. Use case diagram 설계안

### 5.1 Actor 후보

가장 단순하고 명세에 맞는 actor는 다음과 같다.

- 관리자
- 회원

`종료`를 use case로 넣을 경우, 종료는 관리자와 회원이 모두 수행할 수 있게 연결하면 된다. 불필요하게 DB, 파일 시스템, 컨트롤러 같은 시스템 내부 요소를 actor로 추가하지 않는다.

### 5.2 Use case 후보

| Use case ID | Use case 이름 | Actor |
|---|---|---|
| UC-01 | 설문 등록 | 관리자 |
| UC-02 | 설문 응답 | 회원 |
| UC-03 | 응답한 설문 조회 | 회원 |
| UC-04 | 종료 | 관리자, 회원 |

### 5.3 Diagram 작성 원칙

- 시스템 경계 이름: `온라인 설문조사 플랫폼`
- actor는 시스템 경계 밖에 둔다.
- use case는 시스템 경계 안에 둔다.
- `include`, `extend`, actor/use case generalization은 사용하지 않아도 충분하다.
- HW1의 회원가입, 로그인, 설문 검색, 설문 삭제, 통계 조회는 HW2 입출력 메뉴에 없으므로 넣지 않는 편이 안전하다.

## 6. Use case description 초안

아래 내용을 문서화할 때는 actor action/system response를 분리해서 표로 작성하면 된다.

### UC-01 설문 등록

- Actor: 관리자
- Goal: 관리자가 단일 문항 객관식 설문을 등록한다.
- Precondition: 설문 등록 메뉴가 선택 가능하다.
- Postcondition: 입력된 설문 주제와 객관식 항목 수가 설문 목록에 추가된다.

| Step | Actor action | System response |
|---:|---|---|
| 1 | 관리자가 메뉴 번호 `1`, 설문 주제, 항목 수를 입력한다. |  |
| 2 |  | 시스템은 설문 등록 메뉴명을 출력한다. |
| 3 |  | 시스템은 입력된 설문 주제와 항목 수를 출력한다. |
| 4 |  | 시스템은 해당 설문을 이후 응답 가능한 설문으로 유지한다. |

출력 예시는 다음과 같다.

```text
1. 설문 등록
> survey1 4
```

### UC-02 설문 응답

- Actor: 회원
- Goal: 회원이 설문 주제에 대해 객관식 항목 번호를 선택하여 응답한다.
- Precondition: 응답하려는 설문이 등록되어 있다.
- Postcondition: 회원의 응답 목록에 설문 주제와 응답 항목 번호가 추가된다.

| Step | Actor action | System response |
|---:|---|---|
| 1 | 회원이 메뉴 번호 `2`, 설문 주제, 응답 항목 번호를 입력한다. |  |
| 2 |  | 시스템은 설문 응답 메뉴명을 출력한다. |
| 3 |  | 시스템은 입력된 설문 주제와 응답 항목 번호를 출력한다. |
| 4 |  | 시스템은 해당 응답을 응답한 설문 목록에 반영한다. |

출력 예시는 다음과 같다.

```text
2. 설문 응답
> survey1 2
```

### UC-03 응답한 설문 조회

- Actor: 회원
- Goal: 회원이 자신이 응답한 모든 설문 정보를 조회한다.
- Precondition: 응답 조회 메뉴가 선택 가능하다.
- Postcondition: 회원이 응답한 설문 주제와 응답 항목 번호가 출력된다.

| Step | Actor action | System response |
|---:|---|---|
| 1 | 회원이 메뉴 번호 `3`을 입력한다. |  |
| 2 |  | 시스템은 응답한 설문 조회 메뉴명을 출력한다. |
| 3 |  | 시스템은 회원이 응답한 모든 설문의 설문 주제와 응답 항목 번호를 출력한다. |

출력 예시는 다음과 같다.

```text
3. 응답한 설문 조회
> survey1 2
```

### UC-04 종료

- Actor: 관리자, 회원
- Goal: 사용자가 프로그램 실행을 종료한다.
- Precondition: 종료 메뉴가 선택 가능하다.
- Postcondition: 더 이상 메뉴 입력을 받지 않는다.

| Step | Actor action | System response |
|---:|---|---|
| 1 | 사용자가 메뉴 번호 `4`를 입력한다. |  |
| 2 |  | 시스템은 종료 메뉴명을 출력한다. |
| 3 |  | 시스템은 프로그램을 종료한다. |

출력 예시는 다음과 같다.

```text
4. 종료
```

## 7. Communication diagram 설계 방향

Communication diagram은 객체들이 어떤 순서로 메시지를 주고받는지를 보여주는 산출물이다. HW2/3 구현을 고려하면 Boundary-Control-Entity 스타일로 그리면 깔끔하다.

### 7.1 공통 객체 후보

| 객체 | 스테레오타입 | 역할 |
|---|---|---|
| `:MenuBoundary` | boundary | 입력 파일에서 메뉴와 파라미터를 받고 출력 파일 형식으로 결과를 표시 |
| `:SurveyController` | control | 설문 등록, 응답, 조회, 종료 요청을 처리 |
| `:SurveyRepository` | entity 또는 collection | 등록된 설문 목록 관리 |
| `:ResponseRepository` | entity 또는 collection | 회원이 응답한 설문 목록 관리 |
| `survey:Survey` | entity | 설문 주제와 항목 수 보관 |
| `response:SurveyResponse` | entity | 설문 주제와 선택한 응답 항목 번호 보관 |

### 7.2 UC-01 설문 등록 메시지 흐름

1. `관리자 -> :MenuBoundary : inputRegisterSurvey(topic, optionCount)`
2. `:MenuBoundary -> :SurveyController : registerSurvey(topic, optionCount)`
3. `:SurveyController -> survey:Survey : create(topic, optionCount)`
4. `:SurveyController -> :SurveyRepository : add(survey)`
5. `:SurveyController -> :MenuBoundary : printRegisteredSurvey(topic, optionCount)`

### 7.3 UC-02 설문 응답 메시지 흐름

1. `회원 -> :MenuBoundary : inputAnswerSurvey(topic, optionNumber)`
2. `:MenuBoundary -> :SurveyController : answerSurvey(topic, optionNumber)`
3. `:SurveyController -> :SurveyRepository : findByTopic(topic)`
4. `:SurveyController -> response:SurveyResponse : create(topic, optionNumber)`
5. `:SurveyController -> :ResponseRepository : add(response)`
6. `:SurveyController -> :MenuBoundary : printSurveyResponse(topic, optionNumber)`

### 7.4 UC-03 응답한 설문 조회 메시지 흐름

1. `회원 -> :MenuBoundary : inputShowResponses()`
2. `:MenuBoundary -> :SurveyController : getResponses()`
3. `:SurveyController -> :ResponseRepository : findAll()`
4. `:SurveyController -> :MenuBoundary : printResponses(responses)`

### 7.5 UC-04 종료 메시지 흐름

1. `사용자 -> :MenuBoundary : inputExit()`
2. `:MenuBoundary -> :SurveyController : exit()`
3. `:SurveyController -> :MenuBoundary : printExit()`

## 8. Design class diagram 설계 방향

구현 과제인 HW3까지 고려하면 클래스는 너무 많지 않게 잡는 것이 좋다. 아래 정도면 메뉴 입출력 형식과 기능을 모두 표현할 수 있다.

### 8.1 클래스 후보

#### `Survey`

- Attributes
  - `topic: string`
  - `optionCount: int`
- Operations
  - `getTopic(): string`
  - `getOptionCount(): int`

#### `SurveyResponse`

- Attributes
  - `topic: string`
  - `optionNumber: int`
- Operations
  - `getTopic(): string`
  - `getOptionNumber(): int`

#### `SurveyRepository`

- Attributes
  - `surveys: List<Survey>`
- Operations
  - `addSurvey(topic: string, optionCount: int): void`
  - `findSurvey(topic: string): Survey`

#### `ResponseRepository`

- Attributes
  - `responses: List<SurveyResponse>`
- Operations
  - `addResponse(topic: string, optionNumber: int): void`
  - `getResponses(): List<SurveyResponse>`

#### `SurveyController`

- Attributes
  - `surveyRepository: SurveyRepository`
  - `responseRepository: ResponseRepository`
- Operations
  - `registerSurvey(topic: string, optionCount: int): void`
  - `answerSurvey(topic: string, optionNumber: int): void`
  - `getResponses(): List<SurveyResponse>`
  - `exit(): void`

#### `MenuBoundary`

- Operations
  - `run(): void`
  - `printRegisterSurvey(topic: string, optionCount: int): void`
  - `printAnswerSurvey(topic: string, optionNumber: int): void`
  - `printResponses(responses: List<SurveyResponse>): void`
  - `printExit(): void`

### 8.2 클래스 관계

- `MenuBoundary` depends on `SurveyController`.
- `SurveyController` has references to `SurveyRepository` and `ResponseRepository`.
- `SurveyRepository` aggregates or contains multiple `Survey` objects.
- `ResponseRepository` aggregates or contains multiple `SurveyResponse` objects.
- `SurveyResponse` can store `topic` only, or associate to `Survey`. HW3 구현 단순성을 생각하면 `topic` 문자열을 저장하는 방식이 쉽다.

## 9. 파일 입출력 기준 구현 관점 정리

HW3까지 이어질 것을 고려하면 HW2 설계부터 다음 흐름을 전제로 잡는 것이 좋다.

```text
while true:
    command = read menu number
    if command == 1:
        read topic, optionCount
        register survey
        print "1. 설문 등록"
        print "> topic optionCount"
    elif command == 2:
        read topic, optionNumber
        add response
        print "2. 설문 응답"
        print "> topic optionNumber"
    elif command == 3:
        print "3. 응답한 설문 조회"
        for each response:
            print "> topic optionNumber"
    elif command == 4:
        print "4. 종료"
        break
```

출력 형식을 정확히 맞추는 것이 중요하다. 특히 메뉴명, `>` 기호, 공백, 줄바꿈은 예시와 최대한 동일하게 맞춘다.

## 10. 무엇부터 하면 되는가: 추천 작업 순서

### Step 1. HW2 범위 확정

가장 먼저 HW1 기능 중 HW2에 남길 기능과 제거할 기능을 확정한다.

- 남길 기능: 설문 등록, 설문 응답, 응답한 설문 조회, 종료
- 제거하거나 문서에 넣지 않을 기능: 회원가입, 회원탈퇴, 로그인, 로그아웃, 설문 검색, 설문 상세 조회, 설문 삭제, 응답 수정/취소, 통계 조회

### Step 2. Functional requirement 4개를 내부 기준으로 정리

제출물에는 requirement list가 명시되어 있지 않지만, diagram과 description의 기준이 필요하므로 FR-01~FR-04를 먼저 확정한다.

### Step 3. Use case diagram 작성

UML tool에서 actor 2개와 use case 4개를 작성한다.

- 관리자 — 설문 등록, 종료
- 회원 — 설문 응답, 응답한 설문 조회, 종료

파일 첫 부분에 개인코드를 적고 PDF로 변환한다.

### Step 4. Use case descriptions 작성

UC-01~UC-04를 step-by-step으로 작성한다. 내부 DB 처리처럼 보이는 문장은 줄이고, actor 입력과 system 출력 중심으로 표현한다.

### Step 5. Communication diagram 작성

각 기능별로 Boundary-Control-Entity 객체 사이 메시지를 작성한다. 한 장에 4개 시나리오를 모두 넣으면 복잡할 수 있으므로, 가능하면 UC별로 분리하거나 번호 영역을 명확히 나눈다.

### Step 6. Design class diagram 작성

HW3 구현을 바로 시작할 수 있도록 `Survey`, `SurveyResponse`, repository, controller, boundary 클래스를 작성한다. 속성과 연산 이름은 communication diagram 메시지 이름과 맞춘다.

### Step 7. 네 산출물 간 이름 통일

다음 이름을 모든 문서에서 동일하게 쓴다.

- 설문 주제: `topic`
- 객관식 항목 수: `optionCount`
- 응답 항목 번호: `optionNumber`
- 설문: `Survey`
- 설문 응답: `SurveyResponse`
- 컨트롤러: `SurveyController`

### Step 8. 최종 검수 후 압축

최종 제출 전에 아래를 확인한다.

- PDF 파일 4개가 모두 있는가?
- Use case diagram 첫 부분에 개인코드가 있는가?
- 압축 파일명이 `학번_이름.zip` 형식인가?
- HW1의 불필요한 기능이 HW2 산출물에 섞이지 않았는가?
- 메뉴 입출력 예시와 산출물의 용어가 일치하는가?

## 11. 최종 산출물 파일명 예시

실제 학번과 이름에 맞춰 다음처럼 정리하면 제출 시 혼동이 적다.

```text
학번_이름_HW2_UseCaseDiagram.pdf
학번_이름_HW2_UseCaseDescriptions.pdf
학번_이름_HW2_CommunicationDiagram.pdf
학번_이름_HW2_DesignClassDiagram.pdf
```

압축 파일:

```text
학번_이름.zip
```

## 12. 핵심 결론

HW2는 HW1보다 기능 범위가 훨씬 작다. 따라서 가장 중요한 전략은 **HW1 산출물을 그대로 확장하지 말고, 수정된 메뉴 입출력 형식에 맞춰 4개 기능 중심으로 재설계하는 것**이다.

먼저 use case 4개를 확정하고, 그 use case를 기준으로 communication diagram과 design class diagram의 객체/메시지/메서드 이름을 맞추면 HW2 산출물뿐 아니라 HW3 구현까지 자연스럽게 이어질 수 있다.
