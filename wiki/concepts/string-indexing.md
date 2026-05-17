---
title: "String Indexing"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, strings, unicode]
source_count: 1
---

# String Indexing

## Short Definition

String indexing `String` yoki `&str` ichidan integer index orqali "bitta character" olishga urinish; Rust buni qo'llab-quvvatlamaydi.

## Why It Matters

UTF-8 stringda byte index, Unicode scalar value, va grapheme cluster bir xil emas. Rust integer string indexingni rad qilib, noaniq va noto'g'ri assumptionsni oldini oladi.

## Mental Model

`String` `Vec<u8>`ga o'xshash bytes saqlaydi, lekin text sifatida valid UTF-8 guarantee qiladi. `s[0]` nimani qaytarishi kerakligi noaniq: byte, `char`, grapheme cluster, yoki slice?

## Syntax and Examples

Invalid:

```rust
let s1 = String::from("hi");
let h = s1[0];
```

Explicit alternatives:

```rust
for c in "Зд".chars() {
    println!("{c}");
}

for b in "Зд".bytes() {
    println!("{b}");
}
```

Range bilan slice olish mumkin, lekin byte boundaries valid bo'lishi kerak:

```rust
let hello = "Здравствуйте";
let s = &hello[0..4]; // "Зд"
```

## Common Mistakes

- `.len()` character count deb o'ylash.
- `s[0]` birinchi harfni beradi deb kutish.
- Byte range UTF-8 character boundaryda tugashini tekshirmaslik.
- `char` va grapheme cluster bir xil deb o'ylash.

## Related Concepts

- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[char-type|char type]]
- [[grapheme-clusters|grapheme clusters]]
- [[panic]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings]]
