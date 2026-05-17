---
title: "OsStr"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, filesystem, ffi]
source_count: 1
---

# OsStr

## Short Definition

`OsStr` `OsString`ning borrowed view'i.

## Why It Matters

Path va filename API'larida ownershipsiz OS-native string view kerak bo'ladi.

## Mental Model

`String` : `&str` juftligiga o'xshab, `OsString` : `&OsStr` juftligi bor.

## Syntax and Examples

```rust
let os_str: &std::ffi::OsStr = os_string.as_os_str();
```

## Common Mistakes

- Uni `&str` bilan bir xil deb o'ylash.

## Related Concepts

- [[osstring|OsString]]
- [[path|Path]]

## Sources

- [[wiki/sources/rust-for-backend-developers-file-system]]

