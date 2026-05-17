---
title: "Path"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, filesystem]
source_count: 1
---

# Path

## Short Definition

`std::path::Path` filesystem path semantics uchun borrowed view type.

## Why It Matters

File API'lari text emas, path abstraction qabul qiladi.

## Mental Model

`Path` `OsStr` ustidagi path-aware wrapper sifatida ishlaydi. Shu sabab ko'p API `P: AsRef<Path>` qabul qiladi.

## Syntax and Examples

```rust
fn open_any<P: AsRef<std::path::Path>>(path: P) {}
```

## Common Mistakes

- `&str`ni barcha filesystem API'lar uchun universal model deb o'ylash.
- Path'ni faqat UTF-8 string deb ko'rish.

## Related Concepts

- [[osstr|OsStr]]
- [[osstring|OsString]]
- [[asref-trait]]
- [[std-fs|std::fs]]

## Sources

- [[wiki/sources/rust-for-backend-developers-file-system]]

