# TESTING & EVALUATION STRATEGY

## 1. Test pyramid

- Unit: policy, mapping, normalization, unit conversion, parser helpers.
- Contract: service API/event/tool schema.
- Integration: Postgres/object storage/queue/connector mock.
- E2E: upload→publish→ask; ask→tool→answer; permission denial; Zalo/web canonical flow.
- Evaluation: RAG quality, tool routing, groundedness, citation, persona policy.

## 2. Security test bắt buộc

- cross-tenant IDOR.
- prompt injection từ document/tool output.
- unauthorized tool call.
- restricted knowledge leakage.
- secret leakage/logging.
- SQL/template injection.

## 3. Golden Dataset

Mỗi dataset item:
- question;
- persona;
- expected source/tool;
- expected key facts;
- forbidden facts;
- temporal context;
- reference answer optional.

## 4. Retrieval metrics

- Recall@K source đúng.
- citation correctness.
- answer groundedness.
- authority/version correctness.
- zero-hit rate.

## 5. Tool metrics

- router chooses correct tool.
- input mapping correctness.
- permission enforcement.
- side-effect confirmation.
- no fabricated fallback when source fails.

## 6. Migration metrics

- page coverage.
- table/formula/figure detection recall trên fixture.
- numeric fact precision.
- unit normalization correctness.
- conflict detection.

## 7. CI gate

PR thay đổi retrieval/parser/prompt/model config phải chạy relevant evaluation subset. Release chạy full critical suite. Không chấp nhận “model output nhìn có vẻ đúng” thay cho regression result.
