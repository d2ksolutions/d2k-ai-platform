# INTEGRATION & TOOL CONTRACTS

## 1. Mục tiêu

Tách Agent Runtime khỏi vendor/customer API. Agent chỉ gọi canonical tool; `integration-service` chọn connector và map request/response.

## 2. Canonical Tool Envelope

```json
{
  "tool": "inventory.getStock",
  "version": "1.0",
  "tenant_id": "tenant_001",
  "actor": {"id":"user_001","persona":"sales"},
  "resource_scope": ["store:CAUGIAY"],
  "input": {"sku":"IP17PRO-256-BLUE","storeId":"CAUGIAY"},
  "correlation_id": "..."
}
```

Canonical result:

```json
{
  "status": "success",
  "data": {"sku":"...","storeId":"CAUGIAY","availableQuantity":3},
  "freshness": "2026-09-16T00:20:00+07:00",
  "source": "erp_inventory",
  "correlation_id": "..."
}
```

## 3. Tool mẫu

### `product.search`
Input: keyword/category/filters. Output: canonical product summary. Không trả internal cost nếu caller không có permission.

### `inventory.getStock`
Input: sku/productVariant/store. Output: available/reserved/incoming nếu source hỗ trợ.

### `pricing.getPublicPrice`
Input: sku/channel/store. Output: price/promotion/effective period.

### `crm.findCustomer`
Input: customer id/phone/email đã policy-allow. Output phải mask field không cần thiết.

### `crm.createLead`
Side effect; requires confirmation hoặc workflow policy.

### `calendar.getSlots`
Read tool.

### `calendar.createBooking`
Side effect; idempotency + confirmation.

### `analytics.salesSummary`
Input: scope/period/groupBy. Output metric + time range + freshness + source.

## 4. Connector modes

1. Realtime REST/GraphQL.
2. OAuth2 SaaS connector.
3. Periodic sync.
4. Webhook/event.
5. Read-only DB approved template.
6. File/batch.

## 5. Generic REST Connector Config

```yaml
name: ERP Inventory
mode: realtime
base_url: https://erp.example.vn/api
auth:
  type: bearer
  secret_ref: vault://tenants/t001/erp-token
tools:
  inventory.getStock:
    method: GET
    path: /warehouse/stock
    request_mapping:
      input.sku: query.sku
      input.storeId: query.warehouse_id
    response_mapping:
      body.data.qty: data.availableQuantity
      body.data.updated_at: freshness
    timeout_ms: 3000
    retry: 1
```

## 6. Database Connector

LLM không được tự viết arbitrary SQL. Admin/dev tạo approved template:

```sql
SELECT available_qty, updated_at
FROM inventory
WHERE tenant_id = :tenant_id
  AND sku = :sku
  AND warehouse_id = :store_id
LIMIT 1;
```

Input binding phải validate type/allowlist. Credential read-only.

## 7. Permission flow

```text
Agent selects tool
→ access/policy check
→ resource scope resolution
→ confirmation check
→ connector execution
→ result filtering
→ audit
→ Agent receives normalized result
```

## 8. Freshness

Tool response bắt buộc có freshness khi dữ liệu có thể stale. UI/AI nên nói “Dữ liệu cập nhật lúc ...” cho tồn kho/KPI quan trọng.

## 9. Failure semantics

Chuẩn error codes:
- `TOOL_NOT_ALLOWED`
- `CONNECTOR_UNAVAILABLE`
- `SOURCE_TIMEOUT`
- `AUTH_EXPIRED`
- `MAPPING_ERROR`
- `DATA_STALE`
- `RESOURCE_NOT_FOUND`
- `CONFIRMATION_REQUIRED`

Agent không được bịa giá trị khi tool fail.

## 10. Connector SDK

`packages/connectors-sdk` cung cấp interface, auth helpers, mapping, retry, telemetry, schema validation và test harness. Connector vendor-specific có thể là package/plugin nhưng vẫn register vào `integration-service`.
