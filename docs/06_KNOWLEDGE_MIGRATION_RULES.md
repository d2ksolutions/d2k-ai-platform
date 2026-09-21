# QUY TẮC KNOWLEDGE MIGRATION & NORMALIZATION

## 1. Mục tiêu

Đảm bảo migration từ mọi doanh nghiệp/domain không “thành công giả”: file được upload nhưng mất bảng, sơ đồ, công thức, trang scan hoặc chuẩn hóa sai fact.

## 2. Không flatten source

Mỗi document version giữ:
- original file;
- page image/locator;
- layout blocks;
- text;
- tables;
- figures/diagrams;
- formulas;
- chart/curve artifact;
- extracted entities/facts/relations.

## 3. Provenance bắt buộc

Fact tối thiểu:

```json
{
  "subject": "Valve ABC",
  "attribute": "max_pressure",
  "value": 40,
  "unit": "bar",
  "normalized_value": 4000000,
  "normalized_unit": "Pa",
  "source": {
    "document_id": "...",
    "version_id": "...",
    "page": 7,
    "region": "table_2_row_4"
  },
  "confidence": 0.98,
  "authority": 90,
  "review_status": "approved"
}
```

## 4. Page-level coverage

Mỗi trang có trạng thái text/table/figure/formula extraction. Document chỉ `processed` khi coverage rule thỏa; trang lỗi phải visible.

## 5. Domain Pack

Domain Pack là configuration, không code branch. Chứa:
- entity templates;
- relation templates;
- terminology/synonym;
- unit dictionary;
- extraction profile;
- validation thresholds;
- authority defaults;
- retrieval hints.

Tenant có thể extend/override.

## 6. Table

Giữ row/column/header/merged-cell semantics. Numeric fact từ table phải biết cell/row source. Không chỉ serialize table thành paragraph.

## 7. Figure/Diagram

Giữ image crop + caption + page. Vision description chỉ là derived interpretation. Khi user yêu cầu “xem sơ đồ”, trả figure/source thay vì generate hình mới.

## 8. Formula

Lưu original crop + normalized representation (LaTeX khi có) + variable definitions + units + source. Nếu parse ký hiệu không chắc, review.

## 9. Chart/Curve

Không tự digitize điểm chính xác từ curve nếu model không đủ tin cậy. Có thể lưu visual description nhưng fact số từ curve phải có rule review/verification.

## 10. Unit normalization

Lưu original + canonical. Không mất presentation unit. Conversion function phải deterministic/tested, không dựa LLM.

## 11. Version và authority

Source có authority score/type và effective period. Retrieval mặc định ưu tiên active + effective + authoritative source.

## 12. Conflict

Conflict engine nhóm cùng subject/attribute/time context nhưng value khác nhau. Nếu không resolve deterministic bằng version/authority, đưa review.

## 13. Confidence Gate

Threshold theo extraction type/risk. Ví dụ classification có thể auto-pass thấp hơn technical numeric fact. Threshold là config theo Domain Pack/tenant.

## 14. Human Review

Reviewer phải thấy source preview song song hoặc dễ jump đến source; sửa value không làm mất extracted original. Mọi edit có audit.

## 15. Migration Report

Tối thiểu:
- documents/pages;
- pages parsed/failed;
- text coverage;
- tables detected/extracted;
- figures/formulas;
- low confidence count;
- conflicts;
- review backlog;
- publish ratio.

## 16. Continuous ingestion

Upload phiên bản mới không xóa bản cũ. Sau publish:
- supersede version cũ theo rule;
- reindex affected chunks/entities;
- detect stale/conflicting derived fact;
- chạy affected golden dataset nếu có.

## 17. Regression

Mỗi tenant/domain quan trọng có golden question set. Thay parser/chunker/embedding/reranker/model phải chạy regression trước rollout.
