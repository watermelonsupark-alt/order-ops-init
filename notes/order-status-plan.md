## 확인된 후보와 분류

- `src/store.ts` — 기능 수정 후보. 주문 상태와 주문·결제 시도 저장 모델의 정의 위치다.
- `src/payments.ts` — 기능 수정 후보. 결제 요청 처리와 결제 시도 기록이 연결된 위치다.
- `src/orders.ts` — 기능 수정 후보. 주문 생성·조회와 상품 재고 상태가 연결된 위치다.
- `src/admin.ts` — 기능 수정 후보. 관리자 주문 목록과 주문·결제 시도 기반 표시 상태가 연결된 위치다.
- `tests/orders.test.ts` — 검증 변경 후보. 주문·결제·관리자 조회를 통합 테스트하는 현재 보장 위치이며, 상태 변경 동작에는 이를 검증하는 테스트가 필요하다.
- `src/server.ts` — 탐색 전용. 주문·결제·관리자 라우터의 현재 등록 연결을 확인하는 위치이며, 확인된 후보 동작의 직접 책임 파일은 아니다.
- `package.json` — 검사 범위 근거. 테스트와 타입 검사를 실행하는 명령을 정의하는 위치다.

## 현재 동작

| 경우 | 시작 조건 | 현재 응답 | 처리 후 상태 | 주문 저장 여부 | 재고 변화 | 후속 조회 | 코드 근거 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 결제 성공 | `orders`에 존재하며 상태가 `pending`, 총액이 0보다 큰 주문. 카드 토큰은 유효하고 `FAIL`로 시작하지 않음 | `POST /orders/:id/pay`는 HTTP 200과 `{ ok: true, order }`를 반환 | 주문 객체의 `status`가 `paid`가 됨 | `orders`에서 삭제하지 않으므로 유지됨 | 결제 처리에서는 재고를 변경하지 않음. 재고는 주문 생성 때 차감됨 | `GET /orders/:id`는 HTTP 200과 `paid` 주문을 반환. 관리자 목록에는 주문이 포함되고 표시 상태는 결제 이력으로 계산됨 | `src/payments.ts:35`, `src/payments.ts:52`, `src/payments.ts:93`, `src/orders.ts:59`, `src/orders.ts:78`, `src/admin.ts:14` |
| 결제 실패 | `orders`에 존재하며 상태가 `pending`, 총액이 0보다 큰 주문. 정리된 카드 토큰이 `FAIL`로 시작함 | `POST /orders/:id/pay`는 HTTP 200과 `{ ok: false }`를 반환 | 상태를 변경하는 대입이 없음. 삭제 직전 주문 객체는 `pending`이며, 삭제 후 저장된 주문 상태는 없음 | 실패 결제 시도는 `paymentAttempts`에 저장되지만 주문은 `orders`에서 삭제됨 | 주문 생성 때 차감된 재고를 되돌리는 처리가 없어 차감된 값이 유지됨 | `GET /orders/:id`는 HTTP 404. 관리자 목록은 `orders` 값만 순회하므로 삭제된 주문은 포함되지 않음 | `src/payments.ts:29`, `src/payments.ts:86`, `src/orders.ts:59`, `src/orders.ts:81`, `src/admin.ts:33` |
| 재고 부족 | 존재하는 상품에 대해 양의 정수 수량을 요청하지만, 요청 수량이 현재 `stock`보다 큼 | 재고량 비교 분기가 없으므로 `POST /orders`는 HTTP 201과 생성 주문을 반환 | 생성 주문의 상태는 `pending` | 생성 주문을 `orders`에 저장함 | 현재 재고에서 요청 수량을 그대로 빼므로 결과가 음수가 될 수 있음 | 직후 `GET /orders/:id`는 HTTP 200과 저장된 `pending` 주문을 반환 | `src/orders.ts:15`, `src/orders.ts:29`, `src/orders.ts:59`, `src/orders.ts:66`, `src/orders.ts:74`, `src/orders.ts:78` |
