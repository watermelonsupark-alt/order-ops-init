# Lab 04 E2 공용 근거 조사

이 기록은 근거와 후속 활동 후보이며 AGENTS.md의 최종 문장이 아니다. 섹션 배치는 `labs/lecture04/README.md`의 지정 섹션을 따른다. 지속 기준·Skill·Hook 분류 검토는 아직 수행하지 않았다.

## 공용으로 재사용할 기준

### 작업 경계와 승인

- 발견한 문제 또는 기준: 기존 주문 요청의 탐색·결정·계획에는 단계별 경계가 있다. 공용으로 재사용할 부분은 요청 범위와 미결정 정책을 먼저 확인한다는 원칙이다. 모든 작업에 새로운 승인을 요구하거나 구현을 금지하는 근거는 아니다.
- 근거 파일: `notes/task-boundary.md`, `notes/pending-decisions.md`, `notes/next-actions.md`.
- 앞으로 필요한 Codex 활동: 요청을 탐색·결정·구현 단계로 구분하고, 미결정 정책은 사용자에게 확인한 뒤 진행한다.
- 적용할 AGENTS.md 섹션: `Before Editing`(요청 범위 확인), `Rules`(미결정 정책 임의 선택 방지).

### 코드 변경 전 탐색

- 발견한 문제 또는 기준: 상태와 저장소는 `src/store.ts`에 있고, 기능은 각 router에, 연결은 `src/server.ts`에, 현재 보장은 테스트에 분산되어 있다. 변경 전에는 해당 책임 파일과 대응 테스트를 함께 확인해야 한다.
- 근거 파일: `README.md`, `src/server.ts`, `src/store.ts`, `src/orders.ts`, `src/payments.ts`, `src/admin.ts`, `tests/orders.test.ts`.
- 앞으로 필요한 Codex 활동: 수정 대상의 책임, 연결 지점, 실제 동작, 테스트 공백을 먼저 조사하고 그 결과를 변경 범위에 반영한다.
- 적용할 AGENTS.md 섹션: `Before Editing`. README의 저장소 구조를 출발점으로 요청에 관련된 코드와 테스트를 선택하며, 모든 파일을 매번 읽는 고정 절차로 만들지는 않는다.

### 저장소 규약과 산출물 위치

- 발견한 문제 또는 기준: 문서, 작업 기록, 플랫폼 설정은 성격에 맞는 위치에 두며, `labs/`는 실습 자료로서 수정하지 않는다.
- 근거 파일: `labs/CONVENTIONS.md`, `README.md`.
- 앞으로 필요한 Codex 활동: 새 산출물의 목적을 판단해 정해진 위치에 작성하고, 실습 자료는 읽기 전용으로 취급한다.
- 적용할 AGENTS.md 섹션: `Before Editing`(규약 참조), `Rules`(실습 자료 변경 제한).

### 실행과 검증

- 발견한 문제 또는 기준: package scripts는 `npm run dev`(개발 실행), `npm start`(단발 실행), `npm test`(Vitest), `npm run lint`(ESLint), `npm run typecheck`(타입 검사)다. README의 `node labs/tools/check.mjs env`와 Lab 04의 `node labs/tools/check.mjs 04`는 별도 점검 명령이며 npm scripts가 아니다. 이번 조사는 명령 정의를 확인했으며 실행 성공을 검증한 것은 아니다.
- 근거 파일: `package.json`, `README.md`, `tests/orders.test.ts`.
- 앞으로 필요한 Codex 활동: 정의된 명령으로 적절한 검증을 선택해 실행하고, 수정한 행위를 의미 있게 검증하는 assertion이 있는지 확인한다.
- 적용할 AGENTS.md 섹션: `Commands`.

### 테스트 범위와 E3 금지 규칙 후보

- 발견한 문제 또는 기준: 현재 네 테스트는 주문 생성·조회 응답 코드, 결제 결과의 `ok` 필드 존재, 관리자 결과의 배열 여부만 확인하며 각 테스트 전 저장소를 초기화한다. 상태 전이와 재고 일관성까지 보장하지 않는다. Lab 04 E3는 미결 정책 선택 외에도 실패 은폐, 테스트 없는 상태 변경, 실결제 연결을 검토 대상으로 명시한다. 현재 결제는 카드 토큰으로 결과를 모의하며 예외에서도 `ok: false`를 반환한다.
- 근거 파일: `tests/orders.test.ts`의 네 테스트와 `beforeEach`, `src/store.ts`의 `resetStore`, `src/payments.ts`의 `parseCardToken`과 `catch`, `labs/lecture04/README.md`의 E3 실행·검증 기준.
- 앞으로 필요한 Codex 활동: E3에서 각 상황의 금지 행동과 이유 후보를 검토하고, 상태 변경에 필요한 검증과 현재 assertion의 차이를 확인. 현재 결함을 고치는 구현 지시로 변환하거나 특정 실패 응답 정책을 선택하지 않음.
- 적용할 AGENTS.md 섹션: `Rules`, `Before Final Response`(테스트 통과의 실제 보장 범위).

### 완료 보고와 중단 기준

- 발견한 문제 또는 기준: 완료 보고에는 실행한 검증 결과와 미실행 검증 및 남은 위험을 구분해야 한다. 책임 파일, 현재 동작, 필요한 결정 사실 또는 검증 방법을 확인할 수 없으면 이유와 재개 조건을 남기고 멈춘다.
- 근거 파일: `notes/task-boundary.md`의 ‘멈춤 조건’, `labs/lecture04/README.md`의 E4 실행·검증 기준, `package.json`의 `scripts`.
- 앞으로 필요한 Codex 활동: 검증 결과를 근거와 함께 보고하고, 정보 부족·범위 초과·검증 불가 상황에서는 사용자에게 다음 결정을 요청한다.
- 적용할 AGENTS.md 섹션: `Before Final Response`(실행 결과·미실행 검증·남은 위험), `Rules`(범위 밖 결정 방지). 필요한 탐색 후에도 해결되지 않은 장애를 대상으로 하며, 단순히 아직 조사하지 않은 상태와 구별한다.

## 특정 주문 요청에만 해당하는 근거

### 주문·결제 정책의 미결정 사항

- 발견한 문제 또는 기준: 주문 상태 추가 범위, 실패 응답, 재결제 허용, 관리자 실패 정보 범위, 테스트 범위는 현재 해당 주문 변경 요청에서만 미결정인 항목이다.
- 근거 파일: `notes/pending-decisions.md`, `notes/next-actions.md`.
- 앞으로 필요한 Codex 활동: 이 주문·결제 변경을 진행할 때에만 관련 파일과 실제 흐름을 확인한 뒤 사용자 결정을 받는다.
- 적용할 AGENTS.md 섹션: 구체적인 선택지는 수록 대상 아님. 공용 원칙은 위의 `Before Editing`과 `Rules` 후보에서만 활용.

### 요청별 단계와 범위 제한

- 발견한 문제 또는 기준: 탐색 중 수정 순서 확정 금지, 결정 단계에서 변경 목록 작성 금지, 계획 단계에서 구현 금지 및 주문 상태·결제·재고·관리자 조회 밖의 변경 제한은 해당 요청의 작업 경계다.
- 근거 파일: `notes/task-boundary.md`의 ‘요청 분해’·‘아직 하지 말 것’·‘Human Gate’·‘멈춤 조건’.
- 앞으로 필요한 Codex 활동: 해당 요청을 재개할 때 단계별 종료 상태와 승인 여부 확인. 다른 요청에 이 순서와 도메인 범위를 그대로 적용하지 않음.
- 적용할 AGENTS.md 섹션: 구체적인 단계·도메인 목록은 수록 대상 아님. 요청 범위 확인과 진행 불가 보고라는 공용 근거만 재사용.
