---
title: "String Concatenation"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, strings, ownership]
source_count: 1
---

# String Concatenation

## Short Definition

String concatenation bir nechta string bo'laklarini bitta `String`ga birlashtirish.

## Why It Matters

Rustda concatenation ownership semanticsga bo'ysunadi. `+` operatori left operandni move qiladi; [[format-macro|format! macro]] esa ko'pincha readable va ownershipni olmaydigan variant beradi.

## Mental Model

`s1 + &s2` aslida `s1` ownershipini olib, unga `s2` contentsini append qilib, yangi `String` qaytaradi. `s2` reference sifatida beriladi va valid qoladi.

## Syntax and Examples

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2;
```

`s1` endi invalid, `s2` valid.

Murakkabroq case:

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
```

## Common Mistakes

- `+` ikkala `String`ni clone qiladi deb o'ylash.
- `s1 + s2` ishlaydi deb kutish; right side `&str` sifatida kerak.
- Murakkab concatenationda readabilityni yomonlashtirish.

## Related Concepts

- [[string-type|String]]
- [[move-semantics|move semantics]]
- [[borrowing]]
- [[deref-coercions|deref coercions]]
- [[format-macro|format! macro]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings]]
