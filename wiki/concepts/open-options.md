---
title: "OpenOptions"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, filesystem]
source_count: 1
---

# OpenOptions

## Short Definition

`std::fs::OpenOptions` file ochish rejimlarini builder usulida beradigan type.

## Why It Matters

Append, create, truncate, read/write kombinatsiyalarini aniq ifodalash uchun kerak.

## Mental Model

`File::open` va `File::create` oddiy shortcut; `OpenOptions` esa mode matrix'ni explicit boshqaradi.

## Syntax and Examples

```rust
let file = std::fs::OpenOptions::new()
    .append(true)
    .create(false)
    .open("file.txt")?;
```

## Common Mistakes

- Desired mode'larni aniq bermaslik.

## Related Concepts

- [[std-fs|std::fs]]
- [[file-io]]

## Sources

- [[wiki/sources/rust-for-backend-developers-file-system]]

