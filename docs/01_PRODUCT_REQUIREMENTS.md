# YÊU CẦU SẢN PHẨM — D2K TECH ENTERPRISE AI ASSISTANT PLATFORM

> Phiên bản 3.0 — Source of Truth cho business/product requirement. Tài liệu này mô tả **cái gì hệ thống phải làm và vì sao**, không khóa cứng chi tiết triển khai.

## 1. Bối cảnh và thay đổi phạm vi sản phẩm

Ý tưởng ban đầu là nền tảng B2B cho phép doanh nghiệp kết nối Zalo OA để tạo AI chatbot hỏi đáp tài liệu. Sau khi phân tích các tình huống thực tế, phạm vi cần mở rộng thành **Enterprise AI Assistant Platform**.

Lý do: nhiều câu hỏi quan trọng của doanh nghiệp không nằm trong tài liệu. Ví dụ thông số điện thoại có thể nằm trong catalogue, nhưng tồn kho phải lấy từ hệ thống kho; giá/khuyến mãi có thể đến từ commerce/ERP; doanh số và năng suất phải lấy từ BI/Data Warehouse. Do đó AI cần kết hợp **tri thức + dữ liệu realtime + analytics + workflow/action**.

### 1.1 Định vị sản phẩm

D2K TECH cung cấp một nền tảng AI đa tenant để doanh nghiệp:

- Kết nối kênh giao tiếp như Zalo OA, Web Chat, portal nội bộ hoặc API.
- Xây dựng kho tri thức từ tài liệu có cấu trúc và phi cấu trúc.
- Kết nối các hệ thống đang có của doanh nghiệp mà không buộc thay ERP/CRM/Inventory hiện hữu.
- Tạo các AI Persona/Copilot theo vai trò người dùng.
- Cho phép AI tra cứu, phân tích, soạn thảo, đặt lịch, phỏng vấn, tạo ticket, thực thi workflow có kiểm soát.
- Quản trị quyền, audit, chất lượng tri thức và chi phí AI tập trung.

### 1.2 Ba lớp dữ liệu bắt buộc phải phân biệt

| Lớp | Ví dụ | Cơ chế truy cập | Đặc tính |
|---|---|---|---|
| Tri thức | SOP, manual, catalogue, policy, quy trình, công thức | Knowledge Retrieval/RAG | Có version, provenance, citation |
| Dữ liệu nghiệp vụ | tồn kho, trạng thái đơn, lịch, CRM, bảo hành | Tool/Connector/API | Thay đổi thường xuyên, cần freshness |
| Dữ liệu phân tích | doanh thu, KPI, năng suất, xu hướng | Analytics/BI | Aggregate, time-series, phục vụ quản lý |

## 2. Mục tiêu sản phẩm

### 2.1 Mục tiêu chính

1. Một core platform phục vụ nhiều ngành: bán lẻ, bất động sản, thiết bị công nghiệp, hệ thống điện, bảo hiểm, đào tạo nội bộ và các domain mới.
2. Doanh nghiệp có thể tự upload tài liệu, cấu hình knowledge type/domain pack, kết nối nguồn dữ liệu mới mà không yêu cầu sửa business code cho từng ngành.
3. Một câu hỏi được trả lời theo **người đang hỏi + quyền + ngữ cảnh + độ mới dữ liệu**.
4. Mọi kết luận quan trọng phải có nguồn, thời điểm dữ liệu và audit trail khi thích hợp.
5. Hỗ trợ cả self-service SaaS lẫn enterprise onboarding có SME review.

### 2.2 Không phải mục tiêu giai đoạn đầu

- Không thay thế ERP/CRM/BI hiện hữu.
- Không xây một general-purpose data warehouse cho mọi use case.
- Không cho LLM tự chạy SQL tùy ý trên hệ thống khách hàng.
- Không tự động publish fact kỹ thuật rủi ro cao nếu confidence thấp hoặc có conflict.
- Không tách service quá nhỏ chỉ để đạt danh nghĩa microservices.

## 3. Đối tượng sử dụng và AI Persona

### 3.1 Khách hàng cuối

Mục đích: hỏi sản phẩm/dịch vụ, giá công khai, khuyến mãi, chính sách, địa điểm, trạng thái đơn hoặc đặt lịch.

Ví dụ:
- “iPhone 17 Pro 256GB hiện giá bao nhiêu?”
- “Cửa hàng Cầu Giấy còn màu xanh không?”
- “Chính sách đổi trả thế nào?”
- “Đặt lịch bảo hành chiều mai.”

Giới hạn: chỉ được dùng dữ liệu PUBLIC và các tool được doanh nghiệp công khai.

### 3.2 Nhân viên Sales

Mục đích: Sales Copilot hỗ trợ tư vấn, so sánh sản phẩm, kiểm tra tồn kho, khuyến mãi, tạo lead, tra cứu playbook.

Ví dụ:
- “Khách có ngân sách 25 triệu, ưu tiên camera và pin thì chọn máy nào?”
- “Kho Hà Nội còn bao nhiêu iPhone 17 Pro?”
- “Tạo lead cho khách này và hẹn gọi lại 10h sáng mai.”

### 3.3 CSKH / Service

Mục đích: tra cứu bảo hành, lịch sử đơn, policy, troubleshooting, tạo ticket.

### 3.4 Kho / Vận hành

Mục đích: tồn kho, ngưỡng tồn, quy trình nhập/xuất, lịch vận hành, SOP và cảnh báo.

### 3.5 Kỹ thuật / Field Engineer

Mục đích: tra cứu manual, sơ đồ, công thức, đặc tính thiết bị, tiêu chuẩn, lịch bảo trì và hướng dẫn xử lý sự cố.

### 3.6 Quản lý cửa hàng / quản lý bộ phận

Mục đích: xem KPI, doanh số, conversion, tồn kho, năng suất đội nhóm và nguyên nhân biến động.

### 3.7 Lãnh đạo / Executive

Mục đích: Executive Copilot — tổng hợp doanh thu, lợi nhuận, rủi ro, xu hướng, khuyến nghị; chỉ dùng dữ liệu được cấp quyền.

### 3.8 Knowledge Manager / SME

Mục đích: tạo collection/domain schema, kiểm duyệt migration, xử lý conflict, quản lý version/authority, golden dataset và chất lượng tri thức.

### 3.9 System Admin

Mục đích: tenant, user, role, connector, secret, channel, billing, audit, health, usage.

## 4. Kênh truy cập

### 4.1 Zalo OA

Mỗi doanh nghiệp kết nối OA của chính họ. D2K TECH đứng phía sau làm AI platform. Channel Gateway phải tách biệt provider-specific logic khỏi Agent Runtime.

### 4.2 Web Assistant

Dùng cho nhân viên hoặc khách hàng khi cần UI giàu hơn Zalo, xem citation, biểu đồ, form, tài liệu và action confirmation.

### 4.3 Admin Console

Quản trị tenant, tri thức, migration, agent, integration, báo cáo, người dùng và cấu hình.

### 4.4 Internal API

Cho phép hệ thống khác trong doanh nghiệp gọi assistant/retrieval/tool dưới policy được kiểm soát.

## 5. Yêu cầu chức năng — Tenant, Identity và Permission

### FR-IAM-001 — Multi-tenant isolation
Mọi record, index, object và audit event phải gắn tenant. Không được có cross-tenant retrieval hoặc tool execution.

### FR-IAM-002 — Identity resolution
Hệ thống phải map identity từ Zalo/Web/SSO về user/customer identity khi cần. Public conversation có thể anonymous nhưng quyền phải giới hạn.

### FR-IAM-003 — Persona và role
Hỗ trợ role/persona theo tenant: Customer, Sales, Support, Operations, Technician, Manager, Executive, Knowledge Manager, Admin và role tùy chỉnh.

### FR-IAM-004 — Resource/action policy
Permission theo actor + action + resource + context. Ví dụ Sales Cầu Giấy chỉ được `inventory.read` trên store Cầu Giấy; Regional Manager có scope Hà Nội.

### FR-IAM-005 — Data classification
Tối thiểu PUBLIC, INTERNAL, CONFIDENTIAL, RESTRICTED. Permission filtering phải xảy ra trước khi content được đưa vào context LLM.

## 6. Yêu cầu chức năng — Knowledge Platform

### FR-KNW-001 — Dynamic knowledge collection
Admin tự tạo collection/bộ tri thức mà không cần deploy code.

### FR-KNW-002 — Dynamic knowledge schema
Hỗ trợ định nghĩa entity type, attribute, relation, metadata schema, unit profile, document type và validation rule bằng configuration.

### FR-KNW-003 — Multi-format ingestion
Hỗ trợ PDF, DOCX, XLSX, PPTX, TXT, HTML/web và mở rộng connector nguồn tài liệu.

### FR-KNW-004 — Multimodal preservation
Không flatten mọi thứ thành text. Phải giữ text, table, formula, figure, chart, layout, page image và source locator khi có.

### FR-KNW-005 — Source of Truth
File gốc là immutable source. Derived knowledge phải tham chiếu ngược được tới document/version/page/region.

### FR-KNW-006 — Versioning
Tài liệu có active/superseded/draft, effective_from/effective_to, authority level và lịch sử phiên bản.

### FR-KNW-007 — Entity và relation
Cho phép extract/link entity như Product, Equipment, Procedure, Standard, Location, Person... nhưng type phải dynamic theo tenant/domain.

### FR-KNW-008 — Unit normalization
Lưu cả giá trị/đơn vị gốc và canonical value/unit cho các domain kỹ thuật.

### FR-KNW-009 — Conflict detection
Phát hiện các fact mâu thuẫn giữa source/version; dùng authority + effective date + confidence để gợi ý, không tự chọn bừa khi không chắc chắn.

### FR-KNW-010 — Human review queue
Các item confidence thấp, technical numeric fact rủi ro cao, formula/chart uncertain hoặc conflict phải vào hàng đợi kiểm duyệt.

### FR-KNW-011 — Hybrid retrieval
Retrieval kết hợp vector + keyword + metadata + entity/relation + version/authority + permission filter.

### FR-KNW-012 — Citation
Trả lời từ tri thức phải cung cấp source/citation đủ để user kiểm chứng nếu channel hỗ trợ.

## 7. Yêu cầu chức năng — Integration & Tool Platform

### FR-INT-001 — Tool Registry
Platform có registry các canonical tool, ví dụ `inventory.getStock`, `crm.findCustomer`, `calendar.getSlots`, `analytics.salesSummary`.

### FR-INT-002 — Connector abstraction
Agent chỉ biết canonical tool contract; connector chuyển đổi contract sang Odoo/SAP/KiotViet/custom API/DB cụ thể.

### FR-INT-003 — Real-time API connector
Hỗ trợ REST/GraphQL/OAuth2/API key/Bearer và mapping request/response.

### FR-INT-004 — Sync connector
Hỗ trợ lịch đồng bộ định kỳ cho nguồn không phù hợp realtime; response phải mang `data_freshness`/`updated_at`.

### FR-INT-005 — Webhook/event connector
Hỗ trợ nhận event từ hệ thống khách để cập nhật dữ liệu hoặc kích hoạt workflow.

### FR-INT-006 — Read-only database connector
Cho phép query thông qua approved query template; LLM không tự sinh/chạy arbitrary SQL.

### FR-INT-007 — Secret management
Credential phải mã hóa/vault, không nằm trong prompt/log/source code.

### FR-INT-008 — Tool permission
Mỗi tool/action phải qua policy check theo tenant, persona, resource scope.

### FR-INT-009 — Confirmation gate
Action thay đổi dữ liệu hoặc có tác động thực tế như tạo đơn, hủy lịch, gửi báo giá phải hỗ trợ xác nhận trước khi execute.

### FR-INT-010 — Tool audit
Lưu actor, agent, tool, input đã sanitize, outcome, latency, source system và correlation id.

## 8. Yêu cầu chức năng — AI Agent Runtime

### FR-AI-001 — Context building
Context tối thiểu: tenant, user identity, persona, role, channel, conversation, locale, allowed knowledge scope, allowed tools.

### FR-AI-002 — Query routing
Phân biệt knowledge query, operational query, analytics query, mixed query, workflow/action và conversational request.

### FR-AI-003 — Multi-source orchestration
Một câu hỏi có thể dùng nhiều nguồn. Ví dụ “tồn iPhone 17 có đáng lo không?” cần inventory + sales velocity + incoming purchase orders.

### FR-AI-004 — Model routing
Cho phép model tier theo độ khó/chi phí; không buộc mọi request dùng model lớn nhất.

### FR-AI-005 — Grounded answer
Câu trả lời phải phân biệt fact có nguồn với suy luận/khuyến nghị. Với dữ liệu realtime phải thể hiện freshness khi có ý nghĩa.

### FR-AI-006 — Human handoff
Khi confidence thấp, policy yêu cầu hoặc user muốn người thật, chuyển ticket/context cho nhân viên.

### FR-AI-007 — Conversation memory
Memory ngắn hạn theo phiên; dữ liệu dài hạn chỉ lưu nếu tenant policy cho phép. Không tự biến conversation thành knowledge đã publish.

### FR-AI-008 — Drafting
Hỗ trợ soạn thảo văn bản từ template + knowledge + tool data; document generation phải tách khỏi free-form answer khi cần file chính thức.

### FR-AI-009 — Interview/booking workflow
Hỗ trợ hỏi từng bước, đánh giá theo rubric và tạo booking qua tool có permission.

## 9. Yêu cầu chức năng — Analytics

### FR-ANA-001 — Business KPI query
Cho phép manager/executive hỏi KPI từ analytics source được cấu hình.

### FR-ANA-002 — Explain change
Cho phép kết hợp KPI, operational signals và business context để giải thích biến động; AI phải ghi rõ đây là phân tích/suy luận nếu không có causal proof.

### FR-ANA-003 — Dashboard metrics
Admin Console hiển thị adoption, conversations, knowledge quality, processing status, tool usage, cost và error rate.

### FR-ANA-004 — Freshness
Analytics response phải biết khoảng thời gian dữ liệu và thời điểm cập nhật cuối.

## 10. Yêu cầu chức năng — Workflow và Action

- Workflow cấu hình được: trigger → ask/collect → condition → tool → approval → notification.
- Workflow có version và audit.
- Step có retry/idempotency với action quan trọng.
- Action side-effect phải có permission và có thể yêu cầu confirmation/approval.

## 11. Yêu cầu giao diện quản trị

UI mặc định tiếng Việt. Menu chính gọn: **Tổng quan, Tri thức, Tài liệu, Trợ lý AI, Báo cáo, Cài đặt**.

Các màn hình bắt buộc:
- Dashboard tổng quan.
- Kho tri thức/collection.
- Danh sách tài liệu và trạng thái migration.
- Duyệt chuẩn hóa tài liệu.
- Agent/persona configuration.
- Integration/Data Source.
- Báo cáo chất lượng tri thức.
- Usage/cost/audit.

Nguyên tắc: vertical storytelling, chấp nhận scroll, tối đa 4 KPI cards một hàng, một visualization chính mỗi section, tránh dense grid kiểu Grafana.

## 12. Use case tham chiếu — Doanh nghiệp bán điện thoại

| Người dùng | Câu hỏi | Nguồn chính | Quyền |
|---|---|---|---|
| Khách hàng | Giá và khuyến mãi iPhone 17? | Public product + pricing/promo tool | PUBLIC |
| Khách hàng | Cửa hàng X còn màu xanh? | Inventory tool | store-public scope |
| Sales | So sánh iPhone 17 với 16 để tư vấn? | Knowledge + sales playbook | INTERNAL Sales |
| Sales | Kho Hà Nội còn bao nhiêu? | Inventory tool | Sales region scope |
| CSKH | Máy mua 5 tháng trước còn bảo hành? | Order/CRM + warranty policy | Support scope |
| Manager | Doanh số tuần này? | Analytics | Manager store/region |
| Manager | Vì sao Samsung giảm? | Analytics + promotion + inventory | Manager scope |
| Executive | Rủi ro tồn kho toàn hệ thống? | Analytics + inventory + PO | Executive scope |

## 13. Use case tham chiếu — Van công nghiệp / hệ thống kỹ thuật

Platform phải xử lý tài liệu có sơ đồ, bảng đặc tính, công thức, curve, tiêu chuẩn và version kỹ thuật.

Ví dụ:
- “Max pressure của model ABC?” → fact có provenance tới bảng/trang.
- “Cho tôi sơ đồ cấu tạo butterfly valve.” → trả figure/source page, không tự bịa hình.
- “Công thức tính lưu lượng?” → formula object + variable semantics + source.
- “Thiết bị này kỳ bảo trì tiếp theo khi nào?” → maintenance knowledge + operational maintenance tool.

## 14. Yêu cầu phi chức năng

### NFR-SEC — Bảo mật
- Tenant isolation bắt buộc.
- TLS in transit, encryption at rest.
- Secrets qua secret manager.
- Audit immutable/append-oriented cho action nhạy cảm.
- PII masking/log sanitization.
- Prompt/tool output không được vô tình lộ credential/internal config.

### NFR-PERF — Hiệu năng
- UI API thông thường mục tiêu p95 < 500 ms khi không gọi external AI/tool.
- Retrieval search mục tiêu p95 < 1.5 s ở quy mô MVP.
- Streaming AI response khi channel hỗ trợ.
- Tool timeout/retry/circuit breaker cấu hình theo connector.

### NFR-SCALE — Mở rộng
- Service stateless khi có thể, horizontal scale độc lập.
- Worker scale theo queue depth/document complexity.
- Có quota/rate limit theo tenant/plan.

### NFR-OBS — Quan sát
- Correlation ID end-to-end.
- Metrics: request, latency, token/cost, retrieval hit, tool success, queue lag, document quality, connector freshness.
- Trace được câu trả lời đến retrieval/tool/model version.

### NFR-REL — Tin cậy
- Job ingest idempotent.
- Retry có backoff và dead-letter queue.
- Connector failure không làm mất conversation state.

## 15. Business model gợi ý

Subscription nên bán theo năng lực thay vì token thô:
- Số OA/channel.
- Số AI interactions/conversations.
- Số agent/persona.
- Storage và số tài liệu.
- Số connector/tool.
- Analytics/enterprise security/SLA.

Phí provider như Zalo nên tách với phí D2K TECH khi khả thi. Enterprise có thể có onboarding fee do cần domain discovery, migration quality review và integration.

## 16. Phạm vi MVP đề xuất

### MVP-1 — Core Knowledge
Tenant, user/role, collection, upload, parser, multimodal extraction cơ bản, hybrid retrieval, citation, web assistant.

### MVP-2 — Agent + Integration
Persona, tool registry, generic REST connector, inventory sample connector, confirmation gate, audit.

### MVP-3 — Zalo + Analytics
Channel gateway/Zalo, analytics connector/query, manager persona, dashboard reports.

### MVP-4 — Enterprise hardening
SSO, advanced ABAC, domain packs, formula/chart review, golden dataset, enterprise observability, SLA.

## 17. Tiêu chí nghiệm thu cấp sản phẩm

1. Một tenant mới có thể tạo domain/collection mới không sửa backend.
2. Tài liệu mới có thể qua ingest → quality gate → publish và trả lời kèm citation.
3. Câu hỏi tồn kho không được trả bằng stale RAG nếu inventory tool khả dụng.
4. Customer không thể xem margin/doanh số nội bộ dù hỏi bằng prompt injection.
5. Sales và Manager hỏi cùng entity nhưng nhận phạm vi dữ liệu khác nhau theo policy.
6. Connector mới có thể map về canonical tool contract mà không sửa Agent Runtime.
7. Technical fact có thể truy ngược về source document/page/region.
8. Low-confidence/conflicting fact không auto-publish nếu rule yêu cầu review.
9. Mỗi tool action có audit và correlation id.
10. Regression suite phát hiện thay đổi retrieval/model làm giảm chất lượng golden dataset.

## 18. Glossary

- **Tenant**: một doanh nghiệp/khách hàng trên platform.
- **Persona**: vai trò hội thoại/khả năng của AI theo nhóm người dùng.
- **Knowledge Collection**: không gian tri thức logic theo mục đích/domain.
- **Domain Pack**: bộ ontology/schema/unit/extraction/retrieval defaults cho một ngành.
- **Tool**: capability chuẩn mà Agent được gọi, độc lập vendor.
- **Connector**: adapter nối Tool contract với hệ thống cụ thể.
- **Provenance**: bằng chứng nguồn của fact/knowledge.
- **Authority**: độ ưu tiên/chính thức của source.
- **Golden Dataset**: tập câu hỏi/kết quả kỳ vọng để regression/evaluation.
