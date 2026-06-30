# search

Portable CLJC search workspace model for `search.gftd.ai`.

The core is a small EDN index: documents, tokenization, field weights, and
deterministic scoring. It is intentionally host-neutral; a browser, Worker, or
kotoba-backed service can replace storage without changing query semantics.

## Test

```bash
clojure -X:test
```
