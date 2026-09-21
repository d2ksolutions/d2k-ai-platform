# KẾ HOẠCH TRIỂN KHAI CHO AI/CODEX

## Nguyên tắc

Mỗi phase phải tạo vertical slice chạy được, có test và không phá service boundary. Không làm tất cả service skeleton rỗng trước rồi mới implement.

## Phase 0 — Nền móng monorepo

Deliverables:
- workspace/build/test/lint;
- shared contracts, observability, auth context;
- Docker local: Postgres, Redis, object storage, queue;
- CI affected-project build;
- health endpoints.

Acceptance:
- từng app/service build độc lập;
- trace/correlation id xuyên gateway → service;
- không có cross-service DB import.

## Phase 1 — Tenant, Access và Admin shell

- access-service: tenant/user/role/resource scope.
- admin-console shell theo UI rules.
- public-api-gateway.
- audit-service baseline.

Acceptance: hai tenant không nhìn được nhau; role/permission test bắt buộc.

## Phase 2 — Knowledge Catalog + Upload

- collection/domain schema/document/version.
- upload presigned/object storage.
- ingestion job/status.
- document list + migration status UI.

## Phase 3 — Parsing, Multimodal, Normalization

- parser worker.
- page coverage.
- table/figure/formula extraction interface.
- normalization worker.
- provenance/confidence.
- review queue UI.

Không cần hỗ trợ mọi diagram/chart hoàn hảo ngay; cần interface + fallback + review.

## Phase 4 — Retrieval + Web Assistant

- pgvector/full-text hybrid search.
- metadata/version/permission filter.
- rerank/context/citation.
- assistant-web.
- golden dataset baseline.

## Phase 5 — Agent Runtime

- persona/context.
- query router.
- model gateway.
- answer grounding.
- conversation session.
- human handoff hook.

## Phase 6 — Integration & Tool Platform

- tool registry.
- generic REST connector.
- secret reference.
- inventory.getStock sample.
- tool permission + confirmation + audit.
- integration settings UI.

Acceptance: cùng `inventory.getStock` chạy được với mock connector A/B mà agent code không đổi.

## Phase 7 — Zalo Channel

- channel gateway canonical message.
- Zalo adapter/webhook/outbound.
- mapping OA → tenant/agent.
- provider policy/rate handling.

## Phase 8 — Analytics + Manager Persona

- analytics-service canonical metrics.
- sample sales summary/trend.
- manager/executive persona.
- report dashboard.

## Phase 9 — Workflow

- workflow state machine.
- booking sample.
- approval/confirmation/idempotency.

## Phase 10 — Enterprise hardening

- SSO.
- advanced ABAC.
- domain pack management.
- audit retention/export.
- connector circuit breaker.
- cost/quota/billing metrics.
- security and load tests.

## Cách giao task cho AI

Mỗi task phải chứa:
1. mục tiêu business;
2. service ownership;
3. API/event contract;
4. migration/data change;
5. authorization rule;
6. test/acceptance;
7. UI reference nếu có.

Không giao prompt mơ hồ như “build knowledge service”. Hãy giao vertical task như “Upload document → create ingestion job → publish event → show status in Admin Console”.
