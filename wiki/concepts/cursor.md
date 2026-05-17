---
title: "Cursor"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, io, testing]
source_count: 1
---

# Cursor

## Short Definition

`std::io::Cursor<T>` in-memory buffer ustida read/write cursor saqlovchi wrapper.

## Why It Matters

`Vec<u8>` yoki byte slice'ni file/socket kabi stream sifatida test qilishga imkon beradi.

## Mental Model

`Cursor` data'ni ko'chirib tashlamaydi; current position'ni yuritadi. `T: AsRef<[u8]>` kabi byte view bera oladigan inner type bilan ishlaydi.

## Syntax and Examples

```rust
use std::io::{Cursor, Read};

let mut c = Cursor::new(vec![65, 66, 67]);
let mut s = String::new();
c.read_to_string(&mut s)?;
```

## Common Mistakes

- `Cursor` bound'ini `AsRef<u8>` deb o'qish.
- Uni faqat reading uchun deb o'ylash.

## Related Concepts

- [[read-trait|Read]]
- [[write-trait|Write]]
- [[std-io|std::io]]

## Sources

- [[wiki/sources/rust-for-backend-developers-io]]

