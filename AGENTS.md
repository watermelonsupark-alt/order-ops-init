## Project Overview

이 저장소는 주문 생성, 결제, 재고, 관리자 조회가 연결된 작은 API를 대상으로 개발 워크플로를 실습하기 위한 프로젝트입니다. 애플리케이션은 TypeScript와 Express로 구성되며, 테스트와 정적 검사를 위한 도구를 함께 사용합니다. 상품과 주문은 메모리의 Map에, 결제 시도는 메모리 배열에 저장되므로 서버를 다시 시작하면 기록이 사라집니다.

## Before Editing

- 산출물 위치와 변경 규약은 `labs/CONVENTIONS.md`를 따른다. `labs/`는 수정하지 않는다.
- 요청에 관련된 책임 파일, 연결 지점, 대응 테스트를 확인한다. 저장소 구조는 `README.md`에서 찾고, 서버 조립은 `src/server.ts`, 상태와 저장소는 `src/store.ts`, 기능은 각 router, 현재 보장은 `tests/`에서 확인한다.
- 요청에 아직 정해지지 않은 정책이 있으면 구현 전에 사용자에게 확인한다.

## Commands

- `npm run dev` — 개발 서버 실행
- `npm start` — 서버 한 번 실행
- `npm test` — 테스트 실행
- `npm run lint` — `src/`와 `tests/` 정적 검사
- `npm run typecheck` — TypeScript 타입 검사
