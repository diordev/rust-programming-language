---
title: "OnceLock"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, globals, initialization, sync]
source_count: 1
---

# OnceLock

## Short Definition

`std::sync::OnceLock<T>` value'ni ko'pi bilan bir marta initialize qilinadigan global container.

## Why It Matters

Initialization path bir nechta code path'dan kelishi mumkin bo'lsa `LazyLock`dan ko'ra mosroq.

## Mental Model

Bo'sh quti. Bir marta `set()` yoki `get_or_init()` bilan to'ldirasiz. Keyin value o'sha holatda qoladi.

## Syntax and Examples

```rust
static O: std::sync::OnceLock<std::sync::Mutex<String>> =
    std::sync::OnceLock::new();
```

```rust
let _ = O.set(std::sync::Mutex::new("1".to_string()));
```

## Common Mistakes

- `get().unwrap()` init bo'lmagan holatda panic qilishini unutish.
- `set()` qaytargan `Err(value)`ni e'tiborsiz qoldirish.

## Related Concepts

- [[lazylock|LazyLock]]
- [[static-items]]
- [[global-data]]

## Sources

- [[wiki/sources/rust-for-backend-developers-global-data]]
