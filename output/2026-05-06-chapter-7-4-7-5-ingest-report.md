# Chapter 7.4-7.5 Ingest Report

Date: 2026-05-06

## Scope

Processed sources:

- `raw/books/the_rust_programming_language/7.4. Bringing Paths Into Scope with the use Keyword.md`
- `raw/books/the_rust_programming_language/7.5. Separating Modules into Different Files.md`

## Wiki Updates

- Created source summaries:
  - `wiki/sources/7-4-bringing-paths-into-scope-with-the-use-keyword.md`
  - `wiki/sources/7-5-separating-modules-into-different-files.md`
- Expanded chapter page: `wiki/chapters/7-packages-crates-and-modules.md`
- Created concept pages:
  - `wiki/concepts/pub-use.md`
  - `wiki/concepts/use-aliases.md`
  - `wiki/concepts/nested-use-paths.md`
  - `wiki/concepts/glob-operator.md`
  - `wiki/concepts/module-file-layout.md`
  - `wiki/concepts/mod-declarations.md`
- Created example page:
  - `wiki/examples/restaurant-reexports-and-file-layout.md`
- Updated related pages:
  - `wiki/concepts/use-declarations.md`
  - `wiki/concepts/module-system.md`
  - `wiki/concepts/module.md`
  - `wiki/concepts/module-tree.md`
  - `wiki/concepts/paths.md`
  - `wiki/concepts/scope.md`
  - `wiki/concepts/privacy.md`
  - `wiki/concepts/pub-keyword.md`
  - `wiki/concepts/public-api.md`
  - `wiki/concepts/crate.md`
  - `wiki/concepts/dependencies.md`
  - `wiki/concepts/standard-library.md`
  - `wiki/crates/rand.md`
  - `wiki/tools/cargo-toml.md`
  - `wiki/patterns/api-design.md`
  - `wiki/index.md`
  - `wiki/overview.md`
  - `wiki/roadmap/rust-learning-roadmap.md`
  - `wiki/log.md`

## Graph Relations Added

Appended Chapter 7.4-7.5 relations to `infranodus/rust-book-relations.txt`.

Main relation clusters:

- `use` -> scope-local shortcut, privacy-checked import
- idiomatic imports -> parent module for functions, direct item path for structs/enums
- `as` -> local alias for name conflicts
- `pub use` -> re-export / public API shape
- external packages -> `Cargo.toml` dependency plus crate-name paths
- nested use paths -> common prefix and `self`
- glob operator -> all public items with collision risk
- `mod` declarations -> module file loading
- module file layout -> modern `name.rs` / `name/submodule.rs` and older `mod.rs`

## Review Notes

- Chapter 7 is now complete through 7.5.
- Wikilink/frontmatter lint passed for current `wiki/**/*.md`.
- `raw/books/the_rust_programming_language/8. Common Collections.md` and `8.1 Storing Lists of Values with Vectors.md` are present but outside this ingest scope, so they were left unprocessed.
