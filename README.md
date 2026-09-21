# D2K TECH Enterprise AI Assistant Platform — Bộ tài liệu nền tảng v3

Phiên bản này tổng hợp các quyết định sản phẩm và kiến trúc mới nhất cho nền tảng AI B2B của D2K TECH.

## Tầm nhìn

Nền tảng không chỉ là chatbot RAG trên Zalo. Core product là **Enterprise AI Assistant Platform** gồm ba năng lực chính:

1. **Knowledge Platform** — tiếp nhận, chuẩn hóa, quản trị và truy xuất tri thức doanh nghiệp.
2. **Integration & Tool Platform** — kết nối dữ liệu nghiệp vụ realtime như ERP, CRM, tồn kho, lịch, HRM và API tùy chỉnh.
3. **AI Agent Runtime** — nhận diện persona/quyền, định tuyến câu hỏi, gọi tri thức/tool/analytics, thực hiện workflow và trả lời trên Zalo/Web/Internal App.

## Tài liệu chính

- `docs/01_PRODUCT_REQUIREMENTS.md` + `.docx`: yêu cầu sản phẩm, persona, use case, scope, acceptance criteria.
- `docs/02_SYSTEM_ARCHITECTURE.md` + `.docx`: kiến trúc microservices monorepo, service boundaries, data flow, deployment, security.
- `docs/03_UIUX_DESIGN_RULES.md`: quy tắc giao diện D2K TECH, tiếng Việt, nền trắng, tối giản, vertical dashboard.
- `docs/04_IMPLEMENTATION_ROADMAP.md`: roadmap triển khai theo phase/sprint để giao AI/Codex.
- `docs/05_INTEGRATION_AND_TOOL_CONTRACTS.md`: chuẩn Tool Registry, connector, realtime/sync/webhook/DB integration.
- `docs/06_KNOWLEDGE_MIGRATION_RULES.md`: chuẩn migration đa phương thức, provenance, confidence, conflict, quality gate.
- `docs/07_TESTING_AND_EVALUATION.md`: test pyramid, golden dataset, RAG/tool/permission regression.
- `docs/08_GO_TO_MARKET_VIETNAM.md`: chiến lược Go-To-Market Việt Nam — ICP, positioning, pricing hypothesis, pilot, funnel, content 90 ngày, sales motion, partner và KPI.
- `AGENTS.md`: luật bắt buộc cho AI/Codex khi sửa repo.
- `skills/d2k-enterprise-ai-platform/SKILL.md`: skill hướng dẫn AI xử lý task trong repo.
- `uiux-reference/`: các màn hình tham chiếu đã chốt style.

## Nguyên tắc bất biến

- Không hard-code domain như `phone`, `valve`, `real_estate`, `electrical` trong business logic.
- Không coi RAG là database realtime.
- Không cho LLM gọi trực tiếp endpoint/database của khách hàng.
- Mọi dữ liệu kỹ thuật được chuẩn hóa phải giữ provenance về tài liệu/trang/vùng nguồn.
- Mọi query phải được tenant-filter và permission-filter trước retrieval/tool execution.
- Microservices theo bounded context, không tách thành nano-services.
- Monorepo nhưng mỗi service/worker phải có thể build, test và deploy độc lập.
- UI mặc định tiếng Việt, nền trắng, tối giản; scrolling được chấp nhận, visual overload thì không.
