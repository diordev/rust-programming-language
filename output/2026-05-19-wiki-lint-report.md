# Rust Wiki Lint Report

Date: 2026-05-19
Run: structural wiki lint after async ingest

## Scope

`wiki/` va `raw/` tekshirildi. Asosiy lint `wiki/**/*.md` va `raw/**/*.md` ustida bajarildi.

Inline code va fenced code block ichidagi `[[...]]` literal sintaksis wikilink sifatida sanalmadi. Bu TOML `[[bin]]` kabi valid code misollarini false positive qilmaslik uchun kerak.

## Summary

Joriy strukturaviy holat yaxshi. Bitta muammo klassi topildi va tuzatildi: `[[serde-enum-tagging]]` linklari concept va example sahifalari orasida noaniq edi.

| Tekshiruv | Natija |
|-----------|--------|
| Wiki sahifalari | 935 |
| `raw/` source fayllar | 173 |
| `wiki/sources/` sahifalari | 170 |
| Wikilinks checked | 14715 |
| Buzilgan wikilinks | 0 |
| Noaniq wikilinks | 0 |
| Orphan sahifalar | 0 |
| Frontmatter muammolari | 0 |
| Noto'g'ri filename | 0 |
| `wiki/index.md`dan tushib qolgan content page | 0 |
| `index/overview` source_count mismatch | 0 |
| Source summary yetishmayotgan raw fayllar | 3 |
| `needs-review` sahifalar | 3 |

## Bajarilgan Fixlar

### 1. Ambiguous wikilinks

`serde-enum-tagging` stem'i ikki joyda mavjud:

- [[wiki/concepts/serde-enum-tagging|serde enum tagging concept]]
- [[wiki/examples/serde-enum-tagging|serde enum tagging example]]

Shu sabab quyidagi linklar explicit path bilan aniqlashtirildi:

- concept linklari: `wiki/index.md`, `wiki/chapters/rust-for-backend-developers-serialization.md`, `wiki/chapters/rust-for-backend-developers-3-advance.md`, `wiki/sources/rust-for-backend-developers-serialization.md`, `wiki/examples/serde-enum-tagging.md`
- example linklari: `wiki/index.md`, `wiki/log.md`

Natija: ambiguous linklar 7 -> 0, orphan sahifalar 2 -> 0.

## Qolgan Review Nuqtalari

Quyidagi raw source fayllar uchun hali `wiki/sources/` summary yo'q. Bu lint blocker emas, lekin keyingi ingest scope:

- `raw/books/rust-for-backend-developer/4. async/59. Потоки и вводвывод.md`
- `raw/books/rust-for-backend-developer/4. async/61. Tokio.md`
- `raw/books/rust-for-backend-developer/4. async/62. async_trait.md`

Quyidagi sahifalar hali `needs-review`; bu strukturaviy lint blocker emas:

- [[wiki/chapters/17-4-streams-futures-in-sequence]]
- [[stream]]
- [[wiki/sources/17-4-streams-futures-in-sequence]]

## Final State

- frontmatter: 0 muammo
- wikilinks: 0 broken, 0 ambiguous
- orphan pages: 0
- raw/source coverage: 170/173
- index coverage: 0 missing
- source_count consistency: 0 mismatch
