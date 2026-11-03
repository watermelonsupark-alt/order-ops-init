## 다음 작업

1. 관련 파일 후보와 책임 범위를 정리한다.
   - 입력 파일: `notes/order-status-request.md`, `notes/pending-decisions.md`
   - 확인 대상: `src/store.ts`, `src/orders.ts`, `src/payments.ts`, `src/admin.ts`, `src/server.ts`

2. 주문 생성부터 결제 실패·성공, 관리자 조회까지의 현재 동작을 근거 경로와 함께 정리한다.
   - 입력 파일: `notes/order-status-request.md`
   - 확인 대상: `src/orders.ts`, `src/payments.ts`, `src/admin.ts`, `src/store.ts`

3. 현재 테스트가 다루지 않는 주문 상태·실패 결제·재고·관리자 조회 흐름을 목록화한다.
   - 입력 파일: `notes/order-status-request.md`
   - 확인 대상: `tests/orders.test.ts`, `package.json`

## 사람이 결정해야 할 지점

- `notes/pending-decisions.md`의 1번: `OrderStatus`에 추가할 상태 범위
- `notes/pending-decisions.md`의 2번: 결제 실패 응답
- `notes/pending-decisions.md`의 3번: 실패 주문의 재결제 허용 여부
- `notes/pending-decisions.md`의 4번: 관리자 실패 정보 표시 범위
- `notes/pending-decisions.md`의 5번: 테스트 범위
