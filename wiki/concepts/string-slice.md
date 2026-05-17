---
title: "String Slice"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, string, slices]
source_count: 5
---

# String Slice

## Short Definition

String slice `&str` type bilan yoziladi va string data'ning contiguous qismiga reference beradi.

## Why It Matters

`&str` Rustda string APIs uchun idiomatic parameter type. U string literals, `String` slices, va whole `String` references bilan moslashuvchan ishlaydi.

## Mental Model

```rust
let s = String::from("hello world");
let hello = &s[0..5];
```

`hello` pointer + length bo'lgan borrowed view.

Struct field sifatida `&str` saqlash mumkin, lekin bu borrowed data struct instance'dan kam yashamasligini ko'rsatish uchun [[lifetimes]] talab qiladi.

Rustda `&str` UTF-8 encoded borrowed view. String literals program binary'sida saqlanadi va `&str` hisoblanadi. Range bilan string slice olish byte boundariesga tayanadi va UTF-8 character boundary buzilsa panic bo'lishi mumkin.

Owned `String`dan `&str` olish uchun `as_str()` ishlatilishi mumkin:

```rust
let s = String::from("text");
let slice: &str = s.as_str();
```

## Syntax and Examples

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

UTF-8 boundaryga mos slice:

```rust
let hello = "Здравствуйте";
let s = &hello[0..4]; // "Зд"
```

## Common Mistakes

- `&String`ni default parameter type deb olish; `&str` ko'proq flexible.
- String literalning type'i `&str` ekanini unutish.
- UTF-8 character boundary rulesni hisobga olmaslik.
- Struct field uchun `&str` yozib, lifetime annotation kerakligini unutish.
- Range indices bytes ekanini, user-facing characters emasligini unutish.

## Related Concepts

- [[slices]]
- [[string-type|String]]
- [[string-literal-vs-string]]
- [[deref-coercions|deref coercions]]
- [[lifetimes]]
- [[structs]]
- [[utf-8|UTF-8]]
- [[string-indexing|string indexing]]
- [[panic]]
- [[dynamically-sized-types|DST]] — `str` o'zi DST, `&str` fat pointer
- [[sized-trait|Sized trait]]

## Sources

- [[4-3-the-slice-type]]
- [[5-1-defining-and-instantiating-structs]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-strings]]
