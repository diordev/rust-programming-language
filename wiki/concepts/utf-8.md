---
title: "UTF-8"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, strings, unicode]
source_count: 1
---

# UTF-8

## Short Definition

UTF-8 Unicode textni bytes sequence sifatida encode qilish usuli; Rust `String` va `&str` UTF-8 encoded bo'ladi.

## Why It Matters

UTF-8 sababli string length, indexing, slicing, va iteration oddiy ASCII mental modelidan murakkabroq. Rust bu murakkablikni yashirmaydi va invalid assumptionsni compile time yoki runtime checks bilan ushlaydi.

## Mental Model

`String` bytes saqlaydi, lekin bu bytes valid UTF-8 bo'lishi kerak. Bitta user-facing "harf" bir nechta byte, bir nechta `char`, yoki bitta [[grapheme-clusters|grapheme cluster]] sifatida ko'rinishi mumkin.

## Syntax and Examples

```rust
let hello = String::from("Здравствуйте");
assert_eq!(hello.len(), 24); // bytes count
```

```rust
for c in "Зд".chars() {
    println!("{c}");
}

for b in "Зд".bytes() {
    println!("{b}");
}
```

## Common Mistakes

- `.len()` human-perceived character count qaytaradi deb o'ylash.
- Byte indexni valid character index deb qabul qilish.
- `char` va grapheme cluster bir xil deb o'ylash.

## Related Concepts

- [[string-type|String]]
- [[string-slice|String slice]]
- [[char-type|char type]]
- [[grapheme-clusters|grapheme clusters]]
- [[string-indexing|string indexing]]

## Sources

- [[8-2-storing-utf-8-encoded-text-with-strings-the-rust-programming-language]]
