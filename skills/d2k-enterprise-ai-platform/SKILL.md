---
name: d2k-enterprise-ai-platform
description: Hướng dẫn AI/Codex triển khai và chỉnh sửa D2K TECH Enterprise AI Assistant Platform theo product requirements, microservices architecture, knowledge migration, tool integrations và UI/UX rules.
---

# D2K TECH Enterprise AI Platform Skill

## Khi nào dùng skill này

Dùng cho mọi task trong repo liên quan:
- admin console / assistant web;
- knowledge upload/migration/retrieval;
- AI Agent/Persona;
- Zalo/channel integration;
- ERP/CRM/Inventory/BI connector;
- workflow/action;
- permission/audit;
- architecture/refactor.

## Context bắt buộc

Trước task, đọc:
1. `/AGENTS.md`
2. `/docs/01_PRODUCT_REQUIREMENTS.md`
3. `/docs/02_SYSTEM_ARCHITECTURE.md`

Sau đó đọc file chuyên môn:
- UI → `03_UIUX_DESIGN_RULES.md` + `uiux-reference/`
- integration → `05_INTEGRATION_AND_TOOL_CONTRACTS.md`
- migration/RAG → `06_KNOWLEDGE_MIGRATION_RULES.md`
- testing/evaluation → `07_TESTING_AND_EVALUATION.md`

## Workflow chuẩn

### Bước 1 — Xác định bounded context
Nêu rõ service/worker/app ownership. Không sửa nhiều service nếu không cần.

### Bước 2 — Xác định contract
API/event/tool/data schema nào thay đổi? Ưu tiên contract trước implementation.

### Bước 3 — Xác định security
Tenant, actor/persona, action, resource scope, classification và audit requirement.

### Bước 4 — Implement vertical slice
Làm từ request/event đến persistence/result, không tạo skeleton rỗng hàng loạt.

### Bước 5 — Test
Unit + contract + integration; thêm evaluation nếu chạm AI/retrieval.

### Bước 6 — Verify UI/UX
Nếu có UI: tất cả copy tiếng Việt, dùng design tokens/rules, có loading/empty/error/permission state.

## Patterns nên dùng

### Knowledge question
`Agent → Retrieval Service → hybrid search → citation → answer`.

### Realtime business data
`Agent → Tool Registry → Integration Service → Connector → Source → normalized result`.

### Analytics
`Agent → Analytics Service → semantic KPI/source → answer with time range/freshness`.

### Side effect
`Agent → policy → confirmation/approval → Workflow/Tool → audit`.

## Anti-patterns bị cấm

- `LLM → direct ERP URL`.
- `LLM → arbitrary SQL`.
- `RAG → current inventory` khi có source realtime.
- cross-service database access.
- hard-coded industry branching trong core.
- publish technical fact không provenance.
- permission filter sau khi content đã vào prompt.
- dense dashboard để “fit everything above the fold”.

## Output mong đợi từ AI

Khi hoàn thành task, báo ngắn:
- thay đổi ở service/app nào;
- contract/schema nào thay đổi;
- permission/audit nào áp dụng;
- test/evaluation đã thêm/chạy;
- assumption/risk còn lại.
