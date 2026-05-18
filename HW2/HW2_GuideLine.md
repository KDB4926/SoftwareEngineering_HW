# HW2 Use Case Description 작성 가이드라인

온라인 설문조사 플랫폼 HW2에서 `Use Case Description`부터 작성할 때 적용할 기준이다. HW1 산출물은 도메인 이해용으로만 참고하고, 최종 HW2 문서는 수정된 HW2 명세와 파일 입출력 형식을 우선한다.

## 1. HW1에서 가져올 것과 버릴 것

### 1.1 HW1에서 가져올 도메인 개념

HW1의 시스템은 온라인 설문조사 플랫폼이며, 관리자와 회원이 서로 다른 역할을 가진다.

- 관리자는 설문을 등록할 수 있다.
- 회원은 설문에 응답할 수 있다.
- 회원은 자신이 응답한 설문을 조회할 수 있다.
- 설문과 응답은 별개의 정보이며, 하나의 설문에는 여러 응답이 연결될 수 있다.

### 1.2 HW2에서 제외할 HW1 기능

HW2의 메뉴와 파일 입출력 형식에 없는 기능은 Use Case Description에 넣지 않는다.

- 회원 가입
- 회원 탈퇴
- 로그인
- 로그아웃
- 설문 검색
- 설문 상세 조회
- 설문 삭제
- 응답 수정
- 응답 취소
- 통계 조회
- 설문 설명, 문항 내용, 시작 시각, 마감 시각

이 기능들을 넣으면 HW1을 그대로 확장한 것처럼 보이므로, HW2의 수정 요구사항과 불일치할 수 있다.

## 2. HW2 Use Case Description의 기준 범위

HW2의 Use Case Description은 과제 명세서의 수정된 기능 3개만 작성한다. 메뉴 입출력 형식에는 `4. 종료`가 있지만, 이는 프로그램 제어용 메뉴 항목이므로 Use Case Diagram 및 Use Case Description의 독립 use case로 작성하지 않는다.

| UC ID | Use case | Actor | 입력 형식 | 핵심 출력 |
|---|---|---|---|---|
| UC-01 | 설문 등록 | 관리자 | `1 [설문주제] [항목수]` | `1. 설문 등록` 및 입력값 |
| UC-02 | 설문 응답 | 회원 | `2 [설문주제] [응답항목번호]` | `2. 설문 응답` 및 입력값 |
| UC-03 | 응답한 설문 조회 | 회원 | `3` | `3. 응답한 설문 조회` 및 응답 목록 |

## 2.1 `4. 종료` 처리 기준

메뉴 및 파일입출력 형식에는 `4. 종료`가 출력 예시에 포함되어 있다. 그러나 HW2 과제 명세서의 수정된 기능은 `설문 등록`, `설문 응답`, `응답한 설문 조회` 3개이며, 이미 완성된 Use Case Diagram도 이 3개 use case를 기준으로 작성되어 있다.

따라서 `종료`는 다음처럼 처리한다.

- Use Case Diagram: 별도 use case로 추가하지 않는다.
- Use Case Description: 별도 UC로 작성하지 않는다.
- Communication Diagram / Design Class Diagram: 파일 입출력 프로그램의 반복 입력을 끝내는 제어 흐름 또는 boundary의 보조 동작으로만 반영한다.
- HW3 구현: 입출력 형식 준수를 위해 `4. 종료` 출력은 구현하되, 요구사항 분석 산출물의 핵심 기능 use case로 간주하지 않는다.

## 3. 작성 원칙

### 3.1 Step-by-step으로 작성한다

각 use case는 반드시 단계별로 작성한다. 표는 `no.`, `actor action`, `system response` 형태를 권장한다.

- actor action에는 외부 사용자가 입력하거나 선택하는 행위만 쓴다.
- system response에는 시스템이 출력하거나 상태를 갱신하는 결과만 쓴다.
- 한 단계에 actor action과 system response를 섞어 쓰지 않는다.

### 3.2 파일 입출력 기반임을 반영하되 내부 구현은 쓰지 않는다

HW2/3은 파일 입력을 읽고 파일 출력 형식으로 결과를 쓰는 구조이므로, `화면을 출력한다`보다 `메뉴명과 입력값을 출력한다`가 더 적합하다.

다만 Use Case Description은 요구사항 산출물이므로 다음과 같은 내부 설계 문장은 쓰지 않는다.

- `Boundary가 파일을 읽는다.`
- `Controller가 Entity를 생성한다.`
- `Repository에 저장한다.`
- `DB에 insert한다.`

이 내용은 Communication Diagram과 Design Class Diagram에서 다룬다.

### 3.3 HW2의 축소된 데이터만 사용한다

Use Case Description에 등장하는 설문 데이터는 다음으로 제한한다.

- 설문 주제
- 객관식 항목 수
- 응답 항목 번호

`설문 제목`, `설문 설명`, `설문 문항`, `응답 내용`, `응답 일시`, `설문 시작/마감 시각`은 HW2 명세의 입력/출력 형식에 없으므로 쓰지 않는다.

### 3.4 정상 흐름 중심으로 작성한다

과제 명세와 파일 입출력 예시는 오류 처리 형식을 제시하지 않는다. 따라서 기본 Use Case Description은 정상 흐름 중심으로 작성한다.

오류 처리나 대체 흐름을 넣어야 한다면 별도 `Alternative flow`로 분리하고, 핵심 제출물에는 정상 입력 기준의 흐름을 우선한다.

## 4. UC별 권장 작성안

### UC-01 설문 등록

- Actor: 관리자
- Goal: 관리자가 단일 문항 객관식 설문을 등록한다.
- Precondition: 설문 등록 명령을 입력할 수 있다.
- Postcondition: 입력된 설문 주제와 객관식 항목 수가 등록된 설문 정보로 유지된다.

| no. | actor action | system response |
|---:|---|---|
| 1 | 관리자가 `1 [설문주제] [항목수]`를 입력한다. | - |
| 2 | - | 시스템은 `1. 설문 등록`을 출력한다. |
| 3 | - | 시스템은 `> [설문주제] [항목수]`를 출력한다. |
| 4 | - | 시스템은 입력된 설문 주제와 항목 수를 이후 설문 응답에 사용할 수 있도록 유지한다. |

### UC-02 설문 응답

- Actor: 회원
- Goal: 회원이 등록된 설문 하나에 대해 객관식 항목 번호를 선택하여 응답한다.
- Precondition: 응답 대상 설문이 등록되어 있다.
- Postcondition: 회원의 응답 목록에 설문 주제와 응답 항목 번호가 추가된다.

| no. | actor action | system response |
|---:|---|---|
| 1 | 회원이 `2 [설문주제] [응답항목번호]`를 입력한다. | - |
| 2 | - | 시스템은 `2. 설문 응답`을 출력한다. |
| 3 | - | 시스템은 `> [설문주제] [응답항목번호]`를 출력한다. |
| 4 | - | 시스템은 입력된 설문 응답을 이후 응답한 설문 조회에서 확인할 수 있도록 유지한다. |

### UC-03 응답한 설문 조회

- Actor: 회원
- Goal: 회원이 자신이 응답한 모든 설문 정보를 조회한다.
- Precondition: 응답한 설문 조회 명령을 입력할 수 있다.
- Postcondition: 회원이 응답한 설문 주제와 응답 항목 번호 목록이 출력된다.

| no. | actor action | system response |
|---:|---|---|
| 1 | 회원이 `3`을 입력한다. | - |
| 2 | - | 시스템은 `3. 응답한 설문 조회`를 출력한다. |
| 3 | - | 시스템은 응답한 각 설문에 대해 `> [설문주제] [응답항목번호]`를 출력한다. |

## 5. Communication Diagram으로 넘어갈 때 유지할 기준

Use Case Description을 끝낸 뒤 Communication Diagram을 작성할 때는 다음 원칙을 유지한다.

- 각 use case마다 boundary 객체 1개와 control 객체 1개를 둔다. 따라서 HW2의 핵심 use case 기준 boundary/control 쌍은 3개가 된다.
- boundary는 파일 입력을 받고 파일 출력 형식으로 결과를 쓰는 역할을 한다.
- control은 필요한 entity 또는 collection 객체에 요청하고 결과를 boundary에 반환한다.
- 설문은 여러 개 존재할 수 있고, 각 설문에는 여러 응답이 연결될 수 있음을 표현한다.
- control에서 boundary로 전달되는 결과는 별도 내부 함수 호출처럼 과하게 표현하지 말고, control 메시지의 반환값으로 boundary가 받는 방식으로 번호를 정리한다.

## 6. Design Class Diagram으로 넘어갈 때 유지할 기준

Design Class Diagram에서는 Use Case Description에 나온 데이터와 Communication Diagram의 메시지를 클래스 수준으로 일관되게 연결한다.

- `Survey`는 `topic`, `optionCount`를 가진다.
- `SurveyResponse`는 `topic` 또는 `Survey` 참조와 `optionNumber`를 가진다.
- 설문 여러 개를 관리하는 collection 클래스가 필요하다.
- 응답 여러 개를 관리하는 collection 클래스가 필요하다.
- public/private, return type, IS-A/HAS-A 관계를 표현한다.
- 단순 association 화살표를 과하게 늘리기보다, collection과 포함 관계를 명확히 표현한다.

## 7. 최종 검수 체크리스트

- [ ] Use Case Description이 UC-01~UC-03만 포함하는가?
- [ ] 모든 UC가 step-by-step breakdown 형식인가?
- [ ] `actor action`과 `system response`가 분리되어 있는가?
- [ ] `화면`, `버튼`, `팝업` 같은 HW1 UI 표현이 남아 있지 않은가?
- [ ] 입력/출력 형식이 HW2 메뉴 파일 형식과 일치하되, `4. 종료`를 별도 use case로 작성하지 않았는가?
- [ ] 설문 데이터가 `설문주제`, `항목수`, `응답항목번호`로 제한되어 있는가?
- [ ] 내부 설계 요소인 boundary/control/entity 설명을 Use Case Description 본문에 넣지 않았는가?
- [ ] Communication Diagram과 Design Class Diagram에서 사용할 용어와 이름이 Use Case Description과 일치하는가?
