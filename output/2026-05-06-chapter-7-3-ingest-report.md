# Chapter 7.3 Ingest Report

Date: 2026-05-06

## Scope

Processed source:

- `raw/books/the_rust_programming_language/7.3. Paths for Referring to an Item in the Module Tree - The Rust Programming Language.md`

## Wiki Updates

- Created source summary: `wiki/sources/7-3-paths-for-referring-to-an-item-in-the-module-tree-the-rust-programming-language.md`
- Expanded chapter page: `wiki/chapters/7-packages-crates-and-modules.md`
- Created concept pages:
  - `wiki/concepts/absolute-path.md`
  - `wiki/concepts/relative-path.md`
  - `wiki/concepts/super-keyword.md`
  - `wiki/concepts/public-api.md`
- Created compiler error page:
  - `wiki/errors/e0603-private-item.md`
- Created example page:
  - `wiki/examples/restaurant-paths-and-privacy.md`
- Updated related pages:
  - `wiki/concepts/paths.md`
  - `wiki/concepts/privacy.md`
  - `wiki/concepts/pub-keyword.md`
  - `wiki/concepts/module-tree.md`
  - `wiki/concepts/module-system.md`
  - `wiki/concepts/structs.md`
  - `wiki/concepts/struct-fields.md`
  - `wiki/concepts/enums.md`
  - `wiki/concepts/enum-variants.md`
  - `wiki/concepts/binary-crate.md`
  - `wiki/concepts/library-crate.md`
  - `wiki/patterns/api-design.md`
  - `wiki/index.md`
  - `wiki/overview.md`
  - `wiki/roadmap/rust-learning-roadmap.md`
  - `wiki/log.md`

## Graph Relations Added

Appended Chapter 7.3 relations to `infranodus/rust-book-relations.txt`.

Main relation clusters:

- absolute path -> `crate` / crate root
- relative path -> current module / `self` / `super`
- correct path -> still requires privacy access
- E0603 -> private module/function access
- public API -> contract with crate users
- binary + library package -> binary crate as library API client
- `pub struct` -> fields private by default
- `pub enum` -> variants public

## Review Notes

- 7.3 mentions `use` as the next module-system feature but does not yet explain it deeply; `use`, `as`, glob operator, and `pub use` remain pending for later section ingest.
- Sanity check should verify new links and required frontmatter after this ingest.
