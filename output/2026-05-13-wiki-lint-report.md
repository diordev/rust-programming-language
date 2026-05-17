# Rust Wiki Lint Report

Date: 2026-05-13
Run: full wiki lint

## Scope

`wiki/`, `raw/`, `output/`, va `infranodus/` holati tekshirildi. Lintning asosiy tekshiruvlari `wiki/**/*.md` va `raw/**/*.md` ustida bajarildi.

## Summary

Wiki strukturasi hozir sog'lom holatda. Real link va index muammolari tuzatildi.

| Tekshiruv | Natija |
|-----------|--------|
| Wiki sahifalari | 654 |
| `raw/` source fayllar | 111 |
| `wiki/sources/` sahifalari | 111 |
| Wikilinks | 10087 |
| Buzilgan wikilinks | 0 |
| Noaniq wikilinks | 0 |
| Orphan sahifalar | 0 |
| Frontmatter muammolari | 0 |
| Noto'g'ri filename | 0 |
| `raw/` source summary qoplanmagan | 0 |
| `wiki/index.md`dan tushib qolgan content page | 0 |
| `index/overview` source_count mismatch | 0 |

## Bajarilgan Fixlar

### 1. Singan wikilinks

`wiki/tools/compiler.md` ichida 3 ta release-channel linki retarget qilindi:

- `[[stable]]` -> `[[release-channels|stable]]`
- `[[beta]]` -> `[[release-channels|beta]]`
- `[[nightly]]` -> `[[nightly-rust|nightly]]`

### 2. Index coverage

`wiki/index.md`ga 13 ta tushib qolgan page qo'shildi:

- Concepts: [[correctness]], [[interior-mutability]], [[memory-leak]], [[rc-t]], [[refcell-t]], [[reference-cycle]], [[weak-t]]
- Examples: [[rc-shared-list]], [[tree-with-weak-parent]]
- Errors: [[e0005-refutable-pattern-in-local-binding]], [[e0040-explicit-use-of-destructor]]
- Glossary: [[destructor]], [[double-free]]

### 3. Overview source inventory

`wiki/overview.md` yangilandi:

- `source_count: 102` -> `source_count: 111`
- Ingested materials ro'yxatiga 15.3-16.4 source pages qo'shildi.

## Qolgan Review Nuqtalari

Quyidagi 3 sahifa ataylab `needs-review` holatda qoldi, chunki 17.4 raw source qisman olingan:

- [[wiki/sources/17-4-streams-futures-in-sequence]]
- [[wiki/chapters/17-4-streams-futures-in-sequence]]
- [[stream]]

Bu strukturaviy lint xatosi emas; source to'liq bo'lganda ingestni davom ettirish kerak.

## Snippet Kandidatlari

Ko'p code example sahifalari normal chapter/example content sifatida qolishi mumkin. Reusable snippet sifatida ajratishga arziydigan kandidat sahifalar:

- [[threadpool-drop-and-join]]
- [[sender-option-take-shutdown]]
- [[worker-recv-disconnect-shutdown]]
- [[split-at-mut-unsafe]]
- [[arc-mutex-mpsc-receiver]]
- [[job-boxed-fnonce-alias]]
- [[while-let-mutexguard-pitfall]]
- [[turbofish-parse-example]]
- [[io-result-alias]]

## Ontology

Yangi Rust semantic relation qo'shilmadi. Bu run index, overview, va link hygiene cleanup bo'lgani uchun `infranodus/rust-book-relations.txt`ga append qilinmadi; mavjud relationlarda `release channels` va `nightly rust` allaqachon bor.

## Final State

To'liq lint qayta ishga tushirilgandan keyingi holat:

- frontmatter: 0 muammo
- wikilinks: 0 singan, 0 noaniq
- orphan pages: 0
- raw/source coverage: 111/111
- index coverage: 0 missing
- source_count consistency: 0 mismatch
