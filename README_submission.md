# Lab 17 — Benchmark reflection

## Benchmark

Student đạt **11/11 (100%)**: short-term 2/2, long-term 4/4, episodic 2/2, semantic 2/2 và mixed 1/1, nên không có layer nào có hit rate thấp hơn trong practice set. Case retrieve nhiều token nhất là **E03: 1.355 token**. E07 cần kết hợp **long-term + semantic**, với hai evidence bắt buộc là **Python** và **Idempotency-Key**. Token reduction trung bình của student là **14,2%** so với full source context; no-memory giảm **81,8%** nhưng chỉ hit **18,2%** vì gần như không retrieve evidence, nên reduction cao không đồng nghĩa context đúng.

## Architecture and guardrails

Long-term là layer quan trọng nhất trong bộ test vì bao phủ 4/11 case, đặc biệt E08 (recency) và E09 (user isolation). Zep Context Block cung cấp managed cross-session relevance, provenance và graph scope; Redis nhanh, deterministic, có key/TTL rõ ràng, còn Qdrant cho quyền kiểm soát vector search nhưng phải tự quản embeddings, filtering, recency và deletion. Trước durable write hoặc heartbeat phải route đúng scope, kiểm tra consent/allowlist, lưu source–timestamp–confidence–TTL–validity, review thay đổi có tác động cao và coi retrieved text là dữ liệu chứ không phải instruction; heartbeat phải dry-run trước và không tự cấp quyền.

Ở E08, constraint mới `BLUEBIRD-42 → TypeScript/NestJS` được ưu tiên theo recency và project scope mà không xóa preference Python của `ORCHID-27`. Ở E10, compaction giữ `REVIEW-DEADLINE-1600`, Friday và `16:00` trong `<DURABLE_NOTES>` dù raw turns bị loại. `Sliding` kết hợp summary, durable notes và recent turns; `buffer` giữ transcript vô hạn nên có nguy cơ token explosion và tràn context window.

## Evidence

- [Long-term](submission/long_term.png)
- [Episodic](submission/episodic.png)
- [Semantic](submission/semantic.png)
- [Privacy verification](submission/privacy.png)
