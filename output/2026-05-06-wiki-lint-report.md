# Rust Wiki Lint Report

Date: 2026-05-06

## Scope

Checked `wiki/`, `raw/books/`, `output/`, and `infranodus/` for wiki health after the Chapter 8.3 ingest.

## Summary

Final state:

- Wiki markdown pages: 276
- `wiki/sources` summaries: 36
- `raw/books` markdown files: 36
- Missing wikilinks: 0
- Frontmatter issues: 0
- Invalid `type` or `status` values: 0
- Non-kebab-case filenames: 0
- Duplicate page stems: 0
- Orphan wiki pages: 0
- Wiki pages missing from `wiki/index.md`: 0
- Source summaries missing from `wiki/index.md`: 0
- Raw book files without matching source summaries: 0
- Source summaries without matching raw book files: 0
- Structural `source_count` mismatches: 0
- Trailing whitespace in `wiki`, `output`, or `infranodus`: 0

## Fixes Applied

- Fixed broken wikilink `[[crates.io]]` -> [[crates-io|crates.io]] in `wiki/concepts/grapheme-clusters.md`.
- Fixed broken wikilink `[[variables and mutability]]` -> [[variables-and-mutability|variables and mutability]] in `wiki/log.md`.
- Updated stale heading `Chapter 8 synthesis so far` -> `Chapter 8 synthesis` in `wiki/overview.md`.

## Review Backlog

The wiki is structurally healthy, but there are 70 intentional `needs-review` stubs:

- Concepts: 32
- Glossary pages: 32
- Tools: 6

These are not lint failures. They are backlog pages created to keep wikilinks resolvable until deeper ingest expands them.

## Concept Candidates

Repeated terms that may deserve durable pages, aliases, or snippet notes in future maintenance:

- `SipHash`
- `split_whitespace`
- `copied`
- `unwrap_or`
- `median`
- `Pig Latin`
- `Iterator`
- `closure`
- `std::collections`
- `String::from`
- `Vec::new`
- `HashMap::new`
- `or_insert`
- `Unicode scalar value`
- `hash table`

Some of these already have nearby concept coverage, so they should be reviewed before creating new pages.

## Snippet Candidates

Pages with dense Rust code examples that may be worth extracting into `wiki/snippets/` later:

- `wiki/questions/if-loop-while-for-va-ularga-bogliq-control-flow.md` — 19 Rust fences
- `wiki/sources/6-1-defining-an-enum-the-rust-programming-language.md` — 17 Rust fences
- `wiki/sources/8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language.md` — 14 Rust fences
- `wiki/sources/8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language.md` — 11 Rust fences
- `wiki/sources/8-1-storing-lists-of-values-with-vectors-the-rust-programming-language.md` — 11 Rust fences
- `wiki/chapters/6-enums-and-pattern-matching.md` — 11 Rust fences
- `wiki/chapters/8-common-collections.md` — 10 Rust fences

## Recommendation

No major restructuring is needed. Next useful maintenance pass: expand or retire high-value `needs-review` stubs, then extract repeated example patterns into snippets.
