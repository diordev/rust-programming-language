---
title: "Grapheme Clusters"
type: concept
status: draft
created: 2026-05-06
updated: 2026-05-06
tags: [rust, strings, unicode]
source_count: 1
---

# Grapheme Clusters

## Short Definition

Grapheme cluster odam ko'pincha bitta "harf" deb ko'radigan Unicode text birligi.

## Why It Matters

Rust `char` Unicode scalar value'ni bildiradi, lekin user-facing textda bitta ko'rinadigan harf bir nechta scalar valuesdan tuzilishi mumkin. Shuning uchun `.chars()` har doim user-perceived letters bilan bir xil emas.

## Mental Model

Hindi "नमस्ते" example'ida:

- bytes: 18 ta `u8`;
- scalar values / `char`: 6 ta;
- grapheme clusters: 4 ta user-facing birlik.

## Syntax and Examples

```text
bytes:     [224, 164, 168, ...]
chars:     ['न', 'म', 'स', '्', 'त', 'े']
graphemes: ["न", "म", "स्", "ते"]
```

Standard library grapheme cluster iteration bermaydi; kerak bo'lsa crates.io'dagi Unicode crates ishlatiladi.

## Common Mistakes

- `char` human letter degani deb o'ylash.
- `.chars().count()` user-facing character count uchun har doim yetarli deb o'ylash.
- Grapheme processing standard libraryda bor deb kutish.

## Related Concepts

- [[utf-8|UTF-8]]
- [[char-type|char type]]
- [[string-type|String]]
- [[string-indexing|string indexing]]
- [[crates-io|crates.io]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language]]
