# Use Case Descriptions - 온라인 설문조사 플랫폼

## 0. 문서 정보

| 항목 | 내용 |
|---|---|
| 과제 | 소프트웨어공학 과제 2: Design - 온라인 설문조사 플랫폼 |
| 개인코드 | 49263 |
| 기준 Use Case Diagram | `HW2/49263_UseCaseDiagram.pdf` |
| 시스템 경계 | 온라인 설문조사 플랫폼 |
| 작성 목적 | Use Case Diagram에 표현된 유스케이스를 과제 2 명세와 메뉴/파일 입출력 형식에 맞춰 step-by-step breakdown으로 상세화한다. |

## 1. 분석 기준

### 1.1 과제 명세에서 반영한 기능 범위

과제 2의 수정된 기능 명세는 다음 3개 업무 기능과 종료 메뉴를 기준으로 한다.

1. **설문 등록**
   - 관리자는 단일 문항 객관식 설문 정보를 등록할 수 있다.
   - 입력 정보는 설문 주제와 설문 응답 객관식 항목 수이다.
2. **설문 응답**
   - 회원은 설문 리스트 중 하나의 설문에 대해 항목 번호를 선택하여 응답할 수 있다.
3. **응답한 설문 조회**
   - 회원은 자신이 응답한 모든 설문 정보를 조회할 수 있다.
   - 각 응답한 설문에 대해 설문 주제와 응답 항목 번호가 출력된다.
4. **종료**
   - 메뉴 및 파일 입출력 형식에 포함된 종료 기능이다.

### 1.2 Use Case Diagram에서 확인한 Actor와 Use Case

`49263_UseCaseDiagram.pdf`에 표현된 actor와 use case는 다음과 같다.

| Actor | 연결된 Use Case |
|---|---|
| 관리자 | 설문 등록, 종료 |
| 회원 | 설문 응답, 응답한 설문 조회, 종료 |

### 1.3 메뉴 및 파일 입출력 형식

| 메뉴 번호 | Use Case | 입력 형식 | 출력 형식 |
|---:|---|---|---|
| 1 | 설문 등록 | `1 [설문주제] [항목수]` | `1. 설문 등록`<br>`> [설문주제] [항목수]` |
| 2 | 설문 응답 | `2 [설문주제] [응답항목번호]` | `2. 설문 응답`<br>`> [설문주제] [응답항목번호]` |
| 3 | 응답한 설문 조회 | `3` | `3. 응답한 설문 조회`<br>`> { [설문주제] [응답항목번호] }*` |
| 4 | 종료 | `4` | `4. 종료` |

### 1.4 작성 원칙

- Use Case Diagram에 없는 회원가입, 로그인, 설문 검색, 설문 삭제, 통계 조회, 응답 수정/취소 기능은 본 문서에 포함하지 않는다.
- 파일 시스템, 데이터베이스, repository, controller 등 시스템 내부 구현 요소는 actor로 다루지 않는다.
- Use case description은 actor action과 system response를 분리하여 작성한다.
- 내부 설계 수준의 객체 생성, 저장소 탐색, 데이터베이스 저장 방식은 communication diagram 또는 design class diagram에서 다룰 내용이므로 본 문서에서는 상세히 설명하지 않는다.
- 출력 문구는 과제에서 제시한 메뉴 및 파일 입출력 형식을 따른다.

---

## UC-01. 설문 등록

| 항목 | 내용 |
|---|---|
| Use Case ID | UC-01 |
| Use Case Name | 설문 등록 |
| Primary Actor | 관리자 |
| Supporting Actor | 없음 |
| Goal | 관리자가 단일 문항 객관식 설문을 등록한다. |
| Scope | 온라인 설문조사 플랫폼 |
| Level | User goal |
| Trigger | 관리자가 설문 등록 메뉴 번호 `1`을 입력한다. |
| Precondition | 관리자는 설문 등록에 필요한 설문 주제와 객관식 항목 수를 알고 있다. |
| Minimal Guarantee | 설문 등록 처리가 완료되지 않더라도 시스템은 종료되지 않고 다음 메뉴 처리를 계속할 수 있다. |
| Success Guarantee | 입력된 설문 주제와 객관식 항목 수가 등록되어 이후 회원이 해당 설문에 응답할 수 있다. |

### Main Success Scenario

| Step | Actor Action | System Response |
|---:|---|---|
| 1 | 관리자가 메뉴 번호 `1`, 설문 주제, 객관식 항목 수를 입력한다. |  |
| 2 |  | 시스템은 설문 등록 요청을 인식한다. |
| 3 |  | 시스템은 입력된 설문 주제와 객관식 항목 수를 등록한다. |
| 4 |  | 시스템은 출력 제목 `1. 설문 등록`을 출력한다. |
| 5 |  | 시스템은 입력된 설문 주제와 객관식 항목 수를 `> [설문주제] [항목수]` 형식으로 출력한다. |

### Input / Output Example

입력:

```text
1 survey1 4
```

출력:

```text
1. 설문 등록
> survey1 4
```

### Notes

- 설문은 단일 문항 객관식 설문으로 해석한다.
- 항목 수는 해당 설문에서 선택 가능한 객관식 응답 번호의 범위를 결정하는 정보이다.
- Use Case Diagram에 따라 이 use case는 관리자만 수행한다.

---

## UC-02. 설문 응답

| 항목 | 내용 |
|---|---|
| Use Case ID | UC-02 |
| Use Case Name | 설문 응답 |
| Primary Actor | 회원 |
| Supporting Actor | 없음 |
| Goal | 회원이 등록된 설문 중 하나에 대해 응답 항목 번호를 선택하여 응답한다. |
| Scope | 온라인 설문조사 플랫폼 |
| Level | User goal |
| Trigger | 회원이 설문 응답 메뉴 번호 `2`를 입력한다. |
| Precondition | 회원이 응답하려는 설문이 등록되어 있다. |
| Minimal Guarantee | 설문 응답 처리가 완료되지 않더라도 시스템은 종료되지 않고 다음 메뉴 처리를 계속할 수 있다. |
| Success Guarantee | 회원의 응답 정보로 설문 주제와 응답 항목 번호가 기록되어 이후 응답한 설문 조회에서 확인할 수 있다. |

### Main Success Scenario

| Step | Actor Action | System Response |
|---:|---|---|
| 1 | 회원이 메뉴 번호 `2`, 설문 주제, 응답 항목 번호를 입력한다. |  |
| 2 |  | 시스템은 설문 응답 요청을 인식한다. |
| 3 |  | 시스템은 입력된 설문 주제에 대한 회원의 응답 항목 번호를 반영한다. |
| 4 |  | 시스템은 출력 제목 `2. 설문 응답`을 출력한다. |
| 5 |  | 시스템은 입력된 설문 주제와 응답 항목 번호를 `> [설문주제] [응답항목번호]` 형식으로 출력한다. |

### Input / Output Example

입력:

```text
2 survey1 2
```

출력:

```text
2. 설문 응답
> survey1 2
```

### Notes

- 회원은 설문 주제로 응답할 설문을 지정한다.
- 응답 항목 번호는 해당 설문의 객관식 항목 중 회원이 선택한 번호이다.
- Use Case Diagram에 따라 이 use case는 회원만 수행한다.

---

## UC-03. 응답한 설문 조회

| 항목 | 내용 |
|---|---|
| Use Case ID | UC-03 |
| Use Case Name | 응답한 설문 조회 |
| Primary Actor | 회원 |
| Supporting Actor | 없음 |
| Goal | 회원이 자신이 응답한 모든 설문 정보를 조회한다. |
| Scope | 온라인 설문조사 플랫폼 |
| Level | User goal |
| Trigger | 회원이 응답한 설문 조회 메뉴 번호 `3`을 입력한다. |
| Precondition | 회원이 응답한 설문 정보가 조회 가능하다. 응답 내역이 없을 수도 있다. |
| Minimal Guarantee | 조회할 응답 내역이 없더라도 시스템은 조회 메뉴 제목을 출력하고 다음 메뉴 처리를 계속할 수 있다. |
| Success Guarantee | 회원이 응답한 각 설문에 대해 설문 주제와 응답 항목 번호가 출력된다. |

### Main Success Scenario

| Step | Actor Action | System Response |
|---:|---|---|
| 1 | 회원이 메뉴 번호 `3`을 입력한다. |  |
| 2 |  | 시스템은 응답한 설문 조회 요청을 인식한다. |
| 3 |  | 시스템은 출력 제목 `3. 응답한 설문 조회`를 출력한다. |
| 4 |  | 시스템은 회원이 응답한 모든 설문의 설문 주제와 응답 항목 번호를 `> [설문주제] [응답항목번호]` 형식으로 반복 출력한다. |

### Input / Output Example

입력:

```text
3
```

출력:

```text
3. 응답한 설문 조회
> survey1 2
```

응답 내역이 여러 개인 경우 출력 예시는 다음과 같다.

```text
3. 응답한 설문 조회
> survey1 2
> survey2 4
```

### Notes

- 과제 명세의 `{ [설문주제] [응답항목번호] }*`는 응답 내역이 0개 이상 반복될 수 있음을 의미한다.
- Use Case Diagram에 따라 이 use case는 회원만 수행한다.

---

## UC-04. 종료

| 항목 | 내용 |
|---|---|
| Use Case ID | UC-04 |
| Use Case Name | 종료 |
| Primary Actor | 관리자, 회원 |
| Supporting Actor | 없음 |
| Goal | 사용자가 온라인 설문조사 플랫폼 실행을 종료한다. |
| Scope | 온라인 설문조사 플랫폼 |
| Level | User goal |
| Trigger | 사용자가 종료 메뉴 번호 `4`를 입력한다. |
| Precondition | 시스템이 메뉴 입력을 받을 수 있는 상태이다. |
| Minimal Guarantee | 시스템은 종료 요청을 인식하고 종료 출력 형식을 생성한다. |
| Success Guarantee | 시스템은 종료 메시지를 출력한 뒤 더 이상 메뉴 입력을 처리하지 않는다. |

### Main Success Scenario

| Step | Actor Action | System Response |
|---:|---|---|
| 1 | 사용자, 즉 관리자 또는 회원이 메뉴 번호 `4`를 입력한다. |  |
| 2 |  | 시스템은 종료 요청을 인식한다. |
| 3 |  | 시스템은 출력 제목 `4. 종료`를 출력한다. |
| 4 |  | 시스템은 프로그램 실행을 종료한다. |

### Input / Output Example

입력:

```text
4
```

출력:

```text
4. 종료
```

### Notes

- Use Case Diagram에서 종료 use case는 관리자와 회원 모두에게 연결되어 있다.
- 종료는 파일 입출력 형식에 포함된 메뉴이므로 본 use case description에 포함한다.

---

## 2. Use Case 간 일관성 검토

| 검토 항목 | 결과 |
|---|---|
| Use Case Diagram의 use case 4개를 모두 description으로 작성했는가? | 예: 설문 등록, 설문 응답, 응답한 설문 조회, 종료 |
| Use Case Diagram의 actor 연결을 반영했는가? | 예: 관리자는 설문 등록/종료, 회원은 설문 응답/응답한 설문 조회/종료 |
| 과제 명세의 수정된 기능을 반영했는가? | 예: 단일 문항 객관식 설문, 설문 주제, 항목 수, 응답 항목 번호 중심으로 작성 |
| 메뉴 및 파일 입출력 형식을 반영했는가? | 예: 메뉴 번호 1~4와 각 출력 제목/`>` 형식을 반영 |
| HW1의 불필요한 기능을 제외했는가? | 예: 회원가입, 로그인, 설문 검색, 설문 삭제, 통계 조회, 응답 수정/취소 제외 |
| 내부 설계 요소를 use case actor로 추가하지 않았는가? | 예: 파일 시스템, DB, controller, repository를 actor로 추가하지 않음 |

## 3. 최종 요약

본 Use Case Description 문서는 `49263_UseCaseDiagram.pdf`에 표현된 온라인 설문조사 플랫폼의 4개 use case를 과제 2 명세와 메뉴/파일 입출력 형식에 맞춰 상세화한다. 전체 기능 범위는 설문 등록, 설문 응답, 응답한 설문 조회, 종료로 제한되며, 각 use case는 actor의 입력과 시스템의 출력 및 결과가 명확히 드러나도록 step-by-step으로 구성한다.
