---
title: "Byte Buffer"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, io, memory]
source_count: 1
---

# Byte Buffer

## Short Definition

Byte buffer - `u8` qiymatlar ketma-ketligini vaqtincha saqlaydigan xotira bo'lagi.

## Why It Matters

Rust I/O API'lari text emas, avval byte stream bilan ishlaydi. `Read` buffer ichiga byte yozadi, `Write` esa byte slice'dan byte yuboradi.

## Mental Model

Buffer ma'lumot egasi bo'lishi mumkin (`Vec<u8>`) yoki mavjud xotiraga mutable view bo'lishi mumkin (`&mut [u8]`). `read(&mut [u8])` bufferning faqat qaytgan `usize` qismigacha yangi data yozgan deb hisoblanadi; qolgan qismi eski qiymat saqlashi mumkin.

## Syntax and Examples

```rust
use std::io::Read;

let mut buf = [0_u8; 1024];
let n = reader.read(&mut buf)?;
let received = &buf[..n];
```

```rust
let mut out = Vec::<u8>::new();
out.extend_from_slice(b"hello");
```

## Common Mistakes

- `read()` buffer'ni har doim to'liq to'ldiradi deb o'ylash.
- `usize` qaytgan uzunlikni e'tiborsiz qoldirib, butun buffer'ni valid data deb talqin qilish.
- Text va byte boundary'larni aralashtirish.

## Related Concepts

- [[read-trait|Read]]
- [[write-trait|Write]]
- [[cursor|Cursor]]
- [[std-io|std::io]]
- [[vector|Vec<T>]]
- [[slices|slice]]

## Sources

- [[wiki/sources/rust-for-backend-developers-io]]
