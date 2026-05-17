---
title: "Read Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, io, traits]
source_count: 1
---

# Read Trait

## Short Definition

`std::io::Read` byte stream'dan o'qish uchun universal trait.

## Why It Matters

File, socket, `stdin`, memory buffer bir xil read surface bilan ishlaydi.

## Mental Model

Primitive method `read(&mut [u8]) -> io::Result<usize>`. Bu partial read qilishi mumkin. Yuqori darajadagi helperlar shu ustiga quriladi.

## Syntax and Examples

```rust
use std::io::Read;

let n = reader.read(&mut buf)?;
```

## Common Mistakes

- `read` hamisha buffer'ni to'liq to'ldiradi deb o'ylash.
- `read_to_end`ni katta input uchun ehtiyotsiz ishlatish.

## Related Concepts

- [[write-trait|Write]]
- [[cursor|Cursor]]
- [[std-io|std::io]]
- [[io-error]]

## Sources

- [[wiki/sources/rust-for-backend-developers-io]]

