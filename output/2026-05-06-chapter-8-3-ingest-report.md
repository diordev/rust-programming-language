# Chapter 8.3 Ingest Report

Date: 2026-05-06

## Scope

Processed source:

- `raw/books/the_rust_programming_language/8.3. Storing Keys with Associated Values in Hash Maps - The Rust Programming Language.md`

## Wiki Updates

- Created source summary:
  - `wiki/sources/8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language.md`
- Expanded chapter page:
  - `wiki/chapters/8-common-collections.md`
- Expanded concept page:
  - `wiki/concepts/hash-map.md`
- Created concept pages:
  - `wiki/concepts/entry-api.md`
  - `wiki/concepts/hashing-function.md`
- Created example pages:
  - `wiki/examples/team-scores-hashmap.md`
  - `wiki/examples/word-count-hashmap.md`
- Created exercise page:
  - `wiki/exercises/chapter-8-collections-practice.md`
- Updated related pages:
  - `wiki/concepts/collections.md`
  - `wiki/concepts/ownership.md`
  - `wiki/concepts/copy-trait.md`
  - `wiki/concepts/borrowing.md`
  - `wiki/concepts/dereference-operator.md`
  - `wiki/concepts/option.md`
  - `wiki/concepts/use-declarations.md`
  - `wiki/concepts/standard-library.md`
  - `wiki/errors/e0382-borrow-of-moved-value.md`
  - `wiki/index.md`
  - `wiki/overview.md`
  - `wiki/roadmap/rust-learning-roadmap.md`
  - `wiki/log.md`

## Graph Relations Added

Appended Chapter 8.3 relations to `infranodus/rust-book-relations.txt`.

Main relation clusters:

- `HashMap<K, V>` -> key-value mapping, heap storage, homogeneous key/value types
- `get` -> `Option<&V>`, `copied`, `unwrap_or`
- `insert` -> move owned keys/values, overwrite duplicate keys
- `entry` / `or_insert` -> insert-if-missing and mutable reference update
- word count -> `split_whitespace`, `entry`, dereference update
- hashing function -> SipHash, security/performance tradeoff, custom hasher via traits/crates
- Chapter 8 practice -> median/mode, Pig Latin, employee departments

## Review Notes

- Chapter 8 is now complete through 8.3.
- Next book topic is error handling.

## Validation

- `wiki/sources` count: 36.
- Missing wiki wikilinks: 0.
- Frontmatter issues: 0.
- Unprocessed `raw/books` markdown files: 0.
- Trailing whitespace in `wiki`, `output`, and `infranodus`: none found.
