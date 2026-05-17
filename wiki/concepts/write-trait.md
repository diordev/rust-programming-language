---
title: "Write Trait"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, io, traits]
source_count: 1
---

# Write Trait

## Short Definition

`std::io::Write` byte stream'ga yozish uchun universal trait.

## Why It Matters

File, socket, `stdout`, memory buffer bir xil write abstraction bilan ishlaydi.

## Mental Model

`write(&[u8]) -> io::Result<usize>` partial write qilishi mumkin. `write_all()` barcha baytlarni yozishga urinadi. `flush()` buffered sink'lar uchun muhim.

## Syntax and Examples

```rust
use std::io::Write;

writer.write_all(b"hello")?;
writer.flush()?;
```

## Common Mistakes

- `write()` har doim hamma byte'ni yozadi deb o'ylash.
- `flush()`ni semantic boundary kerak joyda unutish.

## Related Concepts

- [[read-trait|Read]]
- [[std-io|std::io]]
- [[file-io]]

## Sources

- [[wiki/sources/rust-for-backend-developers-io]]

