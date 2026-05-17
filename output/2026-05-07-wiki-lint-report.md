# Rust Wiki Lint Report

Date: 2026-05-07

## Scope

`wiki/`, `raw/`, `output/`, `infranodus/` va canonical bo'lmagan `Clippings/` source capture folder tekshirildi. `wiki/_templates/` template placeholders sifatida lint hisobidan chiqarildi.

## Summary

Chapter 10.2 ingestdan keyingi yakuniy holat:

- Wiki markdown pages: 324
- `wiki/sources` summaries: 43
- Canonical raw source files: 43
- Rust Book clipping files outside `raw/`: 67
- Missing wikilinks: 0
- Frontmatter issues: 0
- Invalid `type` or `status` values: 0
- Non-kebab-case wiki filenames: 0
- Duplicate page stems: 0
- Orphan wiki pages: 0
- Wiki pages missing from `wiki/index.md`: 0
- Source summaries missing from `wiki/index.md`: 0
- Raw source files without matching source summaries: 0
- Source summaries without matching raw source files: 0
- Structural `source_count` mismatches: 0
- Trailing whitespace in `wiki`, `output`, or `infranodus`: 0

## Fixes Applied

- Chapter 10.2 ingestdan keyingi wikilinks, index coverage, raw/source coverage, frontmatter, va trailing whitespace tekshirildi.
- `wiki/index.md`, `wiki/overview.md`, `wiki/log.md`, va [[rust-learning-roadmap]] `source_count` qiymatlari 43ga yangilangan holatda tekshirildi.

## Review Needed

`Clippings/` ichida canonical `raw/` tree tashqarisida turgan 67 ta Rust Book markdown file bor. Ular uchun hali canonical `raw/` source summary flow ishlatilmagan. Bu broken wikilink yoki frontmatter xatosi emas, lekin source organization masalasi.

Tavsiya: bu fayllar `raw/books/the_rust_programming_language/` ichiga ko'chiriladimi va keyingi Rust Book batch sifatida ingest qilinadimi - scope tasdiqlansin.

Clipping groups:

- Chapter 10: 1 file
- Chapter 11: 4 files
- Chapter 12: 7 files
- Chapter 13: 5 files
- Chapter 14: 6 files
- Chapter 15: 6 files
- Chapter 16: 5 files
- Chapter 17: 7 files
- Chapter 18: 4 files
- Chapter 19: 4 files
- Chapter 20: 6 files
- Chapter 21: 4 files
- Chapter 22: 8 files

## Review Backlog

65 ta intentional `needs-review` stub bor:

- Concepts: 28
- Glossary pages: 32
- Tools: 5

Bular lint failure emas. Ular deeper ingest bu pagesni kengaytirguncha yoki retire qilguncha wikilinks resolvable bo'lib turishi uchun yaratilgan.

## Snippet Candidates

Rust examples zich bo'lgan va keyinroq `wiki/snippets/`ga extraction qilinishi mumkin bo'lgan pages:

- `wiki/questions/if-loop-while-for-va-ularga-bogliq-control-flow.md`: 19 Rust fences
- `wiki/sources/6-1-defining-an-enum.md`: 17 Rust fences
- `wiki/sources/8-3-storing-keys-with-associated-values-in-hash-maps.md`: 14 Rust fences
- `wiki/sources/9-2-recoverable-errors-with-result.md`: 13 Rust fences
- `wiki/errors/e0277-trait-bound-not-satisfied.md`: 12 Rust fences
- `wiki/chapters/6-enums-and-pattern-matching.md`: 11 Rust fences
- `wiki/sources/8-1-storing-lists-of-values-with-vectors.md`: 11 Rust fences
- `wiki/sources/8-2-storing-utf-8-encoded-text-with-strings.md`: 11 Rust fences
- `wiki/sources/6-2-the-match-control-flow-construct.md`: 10 Rust fences
- `wiki/concepts/option.md`: 10 Rust fences
- `wiki/concepts/generics.md`: 10 Rust fences
- `wiki/chapters/8-common-collections.md`: 10 Rust fences

## Concept Candidates

Takror ko'p uchragan va reviewdan keyin alias, glossary expansion yoki concept page bo'lishi mumkin bo'lgan terms:

- `String`
- `&str`
- `Option<T>`
- `None`
- `impl`
- `self`
- `char`
- `HashMap`
- `Display`
- `&self`
- `&String`

Ularning ko'pi existing concept pages orqali qisman qoplangan, shuning uchun yangi page yaratishdan oldin review kerak.

## Recommendation

Major restructuring kerak emas. Wiki structurally healthy. Keyingi foydali maintenance pass - `Clippings/` bo'yicha qaror qilish, keyin ularni controlled batch sifatida ingest qilish yoki relocate qilish.
