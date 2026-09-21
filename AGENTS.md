# AGENTS.md — D2K TECH Enterprise AI Platform

File này là luật bắt buộc cho Codex/AI coding agent trong repo.

## 1. Trước khi code

Đọc tối thiểu:
1. `README.md`
2. `docs/01_PRODUCT_REQUIREMENTS.md`
3. `docs/02_SYSTEM_ARCHITECTURE.md`
4. file domain liên quan (`03`–`07`).

Nếu làm UI, bắt buộc đọc `docs/03_UIUX_DESIGN_RULES.md` và xem `uiux-reference/`.

## 2. Architectural invariants

- Microservices theo bounded context, monorepo.
- Gateway mỏng; không chứa business logic/domain persistence.
- Service không đọc/ghi DB của service khác.
- Shared package không được trở thành “god package” chứa business state.
- Không tạo service mới khi responsibility hợp lý vẫn thuộc service hiện hữu.
- Background heavy work đi qua worker/queue.

## 3. Domain neutrality

CẤM hard-code core logic theo ngành như:

```ts
if (domain === 'valve') ...
if (domain === 'phone') ...
if (domain === 'real_estate') ...
```

Domain behavior phải đến từ schema/domain pack/configuration, trừ connector/domain plugin được yêu cầu rõ.

## 4. Knowledge rules

- Raw source immutable.
- Derived fact phải có provenance.
- Permission filter trước retrieval/context LLM.
- Version/effective date/authority phải được tôn trọng.
- Low-confidence/conflict không auto-publish nếu policy không cho.
- Không flatten table/figure/formula chỉ thành text rồi xóa cấu trúc.

## 5. Integration/tool rules

- Agent không gọi URL/database khách hàng trực tiếp.
- Chỉ gọi canonical Tool Registry contract.
- Connector map tool sang source cụ thể.
- Arbitrary LLM-generated SQL bị cấm.
- Side-effect tool: permission + audit + idempotency/confirmation khi cần.
- Tool failure → trả lỗi/fallback an toàn; không bịa dữ liệu.

## 6. Security

- Tenant id lấy từ trusted auth context.
- Không log secrets/token/raw credential.
- Validate all external payloads.
- Least privilege.
- Mọi permission change, publish, connector config, side effect action phải audit.

## 7. UI rules

- User-facing title/menu/label mặc định tiếng Việt.
- Nền trắng/sáng, xanh D2K primary.
- Maximum 4 KPI cards một row.
- Vertical storytelling; scroll được chấp nhận.
- Tránh dense grid/chart wall.
- Không tự thêm menu/top-level feature ngoài requirement.

## 8. Coding rules

- API/event/tool contracts có schema + version.
- New behavior phải có test.
- Migration DB backward-safe; tránh destructive migration không plan.
- Error dùng typed/domain code thay vì string ngẫu nhiên.
- Correlation ID xuyên service boundary.
- Time lưu UTC/offset-aware; UI render timezone tenant/user.

## 9. Definition of Done

Task chưa xong nếu thiếu một trong các mục liên quan:
- implementation;
- unit/contract/integration test;
- authorization test;
- telemetry/audit;
- migration/schema;
- docs nếu contract thay đổi;
- UI loading/empty/error/permission state;
- evaluation regression nếu chạm retrieval/parser/prompt/model/tool router.

## 10. Khi requirement không rõ

Ưu tiên phương án ít coupling, configuration-driven, giữ backward compatibility. Không tự mở rộng scope lớn; ghi assumption trong PR/task note.
