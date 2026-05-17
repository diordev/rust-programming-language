---
title: "std::fs"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, filesystem, standard-library]
source_count: 1
---

# std::fs

## Short Definition

`std::fs` filesystem objectlari bilan ishlash moduli.

## Why It Matters

File yaratish, ochish, directory o'qish, metadata olish kabi operationlar shu yerda.

## Mental Model

`std::io` stream interface bo'lsa, `std::fs` file system object layer. Ikkalasi ko'pincha birga ishlaydi.

## Syntax and Examples

```rust
let file = std::fs::File::open("file.txt")?;
```

## Common Mistakes

- Path/string boundary'ni e'tiborsiz qoldirish.

## Related Concepts

- [[file-io]]
- [[open-options|OpenOptions]]
- [[path|Path]]
- [[std-io|std::io]]

## Sources

- [[wiki/sources/rust-for-backend-developers-file-system]]

