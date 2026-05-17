---
title: "Slice Patterns"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, slices, patterns]
source_count: 1
---

# Slice Patterns

## Short Definition

`match` ichida `&[T]` yoki slice ko'rinishidagi qiymatni patternlar bilan branch qilish usuli.

## Why It Matters

Oddiy destructuringda compile-time length yo'qligi muammo bo'lsa, `match` arm'lari slice shape'larini xavfsiz tekshirishga imkon beradi. Parser va protocol decoderlar uchun juda foydali.

## Mental Model

Slice pattern plain `let` destructuring emas. `match` har bir armni tekshiradi; agar `[]` yoki `[head, tail @ ..]` mos kelmasa, keyingi armga o'tadi. Shu sabab runtime uzunlikli data bilan ham ishlaydi.

## Syntax and Examples

```rust
let v = vec![1, 2, 3, 4, 5];

match v.as_slice() {
    [] => println!("empty"),
    [first] => println!("one: {first}"),
    [a, b, c, ..] => println!("{a} {b} {c}"),
}
```

## Common Mistakes

- Slice patternlarni plain `let [a, b] = slice;` bilan aralashtirish.
- `Vec<T>`ni bevosita emas, odatda `as_slice()` orqali match qilish qulayroq ekanini unutish.
- `_` catch-all armni erta qo'yib, aniq slice shakllarini yutib yuborish.

## Related Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[pattern-destructuring|pattern destructuring]]
- [[array-destructuring|array destructuring]]
- [[slices]]

## Sources

- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
