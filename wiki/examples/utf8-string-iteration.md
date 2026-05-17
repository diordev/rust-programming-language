---
title: "UTF-8 string iteration"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, strings, unicode]
source_count: 1
---

# UTF-8 string iteration

## Goal

UTF-8 stringda bytes, `char` values, va string slices bir xil narsa emasligini ko'rsatish.

## Code

```rust
fn main() {
    let text = "Зд";

    for c in text.chars() {
        println!("char: {c}");
    }

    for b in text.bytes() {
        println!("byte: {b}");
    }

    let hello = "Здравствуйте";
    let first_two = &hello[0..4];

    println!("{first_two}");
}
```

## Explanation

`"Зд"` ikki Unicode scalar value'dan iborat, lekin UTF-8 encodingda to'rt byte saqlaydi. `.chars()` scalar valuesni, `.bytes()` raw bytesni beradi.

`&hello[0..4]` byte range bilan `&str` slice yaratadi. Bu valid, chunki range UTF-8 character boundarylarda tugaydi. `&hello[0..1]` kabi range esa panic qiladi, chunki byte 1 character o'rtasiga tushadi.

## Related Pages

- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[utf-8|UTF-8]]
- [[string-indexing|string indexing]]
- [[string-slice|String slice]]
- [[grapheme-clusters|grapheme clusters]]
- [[panic]]
