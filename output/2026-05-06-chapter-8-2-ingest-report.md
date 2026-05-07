# Chapter 8.2 Ingest Report

Date: 2026-05-06

## Scope

Processed source:

- `raw/books/the_rust_programming_language/8.2. Storing UTF-8 Encoded Text with Strings - The Rust Programming Language.md`

## Wiki Updates

- Created source summary:
  - `wiki/sources/8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language.md`
- Expanded chapter page:
  - `wiki/chapters/8-common-collections.md`
- Created concept pages:
  - `wiki/concepts/utf-8.md`
  - `wiki/concepts/string-concatenation.md`
  - `wiki/concepts/format-macro.md`
  - `wiki/concepts/string-indexing.md`
  - `wiki/concepts/grapheme-clusters.md`
- Created example pages:
  - `wiki/examples/string-updating-and-concatenation.md`
  - `wiki/examples/utf8-string-iteration.md`
- Created tool page:
  - `wiki/tools/crates-io.md`
- Updated related pages:
  - `wiki/concepts/string-type.md`
  - `wiki/concepts/string-slice.md`
  - `wiki/concepts/collections.md`
  - `wiki/concepts/deref-coercions.md`
  - `wiki/concepts/move-semantics.md`
  - `wiki/concepts/display-formatting.md`
  - `wiki/concepts/slices.md`
  - `wiki/glossary/char-type.md`
  - `wiki/comparisons/string-literal-vs-string.md`
  - `wiki/errors/e0277-trait-bound-not-satisfied.md`
  - `wiki/index.md`
  - `wiki/overview.md`
  - `wiki/roadmap/rust-learning-roadmap.md`
  - `wiki/log.md`

## Graph Relations Added

Appended Chapter 8.2 relations to `infranodus/rust-book-relations.txt`.

Main relation clusters:

- `String` / `&str` -> UTF-8 encoded text
- `String` -> wrapper over `Vec<u8>`
- `push_str` / `push` -> String updates
- `+` -> moves left `String`, borrows right string slice
- `format!` -> returns `String` without taking ownership
- string indexing -> E0277 and UTF-8 ambiguity
- bytes / scalar values / grapheme clusters
- string slices -> byte ranges and char boundaries

## Review Notes

- Chapter 8 is now complete through 8.2.
- Hash map details remain pending for the next Chapter 8 source.
- Wikilink/frontmatter lint passed for current `wiki/**/*.md`.
