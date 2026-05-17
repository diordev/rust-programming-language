---
title: "OsString"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, filesystem, ffi]
source_count: 1
---

# OsString

## Short Definition

`OsString` operating system native string representation'ni ownership bilan ushlovchi type.

## Why It Matters

Filesystem nomlari har doim valid UTF-8 bo'lishi shart emas.

## Mental Model

`String` Rust UTF-8 text'i; `OsString` esa OS-level filename/path component representation'i.

## Syntax and Examples

```rust
let name: std::ffi::OsString = entry.file_name();
```

## Common Mistakes

- Har filesystem nomini majburan `String` deb qabul qilish.

## Related Concepts

- [[osstr|OsStr]]
- [[path|Path]]

## Sources

- [[wiki/sources/rust-for-backend-developers-file-system]]

