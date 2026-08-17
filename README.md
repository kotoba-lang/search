# search

[![CI](https://github.com/kotoba-lang/search/actions/workflows/ci.yml/badge.svg)](https://github.com/kotoba-lang/search/actions/workflows/ci.yml)

Portable CLJC model for kotoba-lang/search.

`search.model` scores a small in-memory document map (O(corpus)).
`search.postings` is the :search serving plane: term → sorted doc-ids,
AND by intersection, rebuildable from documents or datoms. Query cost is
posting length, not corpus size.

Pages editor: https://kotoba-lang.github.io/search/

The Pages UI is local to kotoba-lang and does not redirect to external hosts.

## Test

```bash
clojure -M:test
```

## Kotoba bounded profile

`src/search/bounded_index.kotoba` is a capability-free port of
`search.model`'s index **structure** — the name-keyed document registry
(doc-id → `{:title :body}` record) and the closed field-weight table — as
`kotoba-lang/compiler`'s canonical bounded typed-map (`[:map :string
[:record …]]`, ≤31 entries) plus closed keyword membership. The search
machinery (`tokenize`, `score-doc`, `search`, `term-count`) stays the CLJC
oracle: `tokenize` is a `re-seq` character-class tokenizer over a-z0-9 +
Japanese Unicode ranges, which needs a per-code-point string primitive the
compiler does not yet expose (its string ABI is UTF-8 byte-offset based, no
code-point-at/regex op). See
[migration/bounded-index-and-weights-v1.edn](migration/bounded-index-and-weights-v1.edn).
