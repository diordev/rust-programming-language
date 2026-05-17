# Rust Wiki Lint Report

Date: 2026-05-17
Run: professional wiki lint after `byte-buffer` fix

## Scope

`wiki/`, `raw/`, `output/`, va `infranodus/` holati tekshirildi. Asosiy lint `wiki/**/*.md` va `raw/**/*.md` ustida bajarildi.

Inline code va fenced code block ichidagi `[[...]]` literal sintaksis wikilink sifatida sanalmadi. Bu TOML `[[bin]]` kabi valid code misollarini false positive qilmaslik uchun kerak.

## Summary

Joriy strukturaviy holat sog'lom. Bitta broken wikilink tuzatildi: `[[byte-buffer]]`.

| Tekshiruv | Natija |
|-----------|--------|
| Wiki sahifalari | 811 |
| `raw/` source fayllar | 162 |
| `wiki/sources/` sahifalari | 162 |
| Wikilinks checked | 13263 |
| Buzilgan wikilinks | 0 |
| Noaniq wikilinks | 0 |
| Orphan sahifalar | 0 |
| Frontmatter muammolari | 0 |
| Noto'g'ri filename | 0 |
| `wiki/index.md`dan tushib qolgan content page | 0 |
| `index/overview` source_count mismatch | 0 |
| `needs-review` sahifalar | 3 |

## Bajarilgan Fixlar

### 1. Broken wikilink

`wiki/sources/rust-for-backend-developers-io.md` ichidagi `[[byte-buffer]]` linki uchun yangi concept yaratildi:

- [[byte-buffer|byte buffer]]

Bu sahifa Rust I/O kontekstidagi `u8` buffer, `Read`, `Write`, `Cursor`, partial read/write, va valid data length mental modelini tushuntiradi.

### 2. Index coverage

`wiki/index.md` Concepts bo'limiga quyidagi link qo'shildi:

- [[byte-buffer|byte buffer]]

### 3. Ontology

`infranodus/rust-book-relations.txt` avval o'qildi, keyin faqat yangi relationlar append qilindi:

```text
[[byte-buffer]] [supports] [[read-trait]]
[[byte-buffer]] [supports] [[write-trait]]
[[cursor]] [wraps] [[byte-buffer]]
```

Ontology qayta generatsiya qilinmadi.

## Qolgan Review Nuqtalari

Quyidagi sahifalar hali `needs-review`; bu strukturaviy lint blocker emas:

- [[wiki/chapters/17-4-streams-futures-in-sequence]]
- [[stream]]
- [[wiki/sources/17-4-streams-futures-in-sequence]]

## Final State

- frontmatter: 0 muammo
- wikilinks: 0 broken, 0 ambiguous
- orphan pages: 0
- raw/source coverage: 162/162
- index coverage: 0 missing
- source_count consistency: 0 mismatch
