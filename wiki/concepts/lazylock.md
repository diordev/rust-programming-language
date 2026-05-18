---
title: "LazyLock"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, globals, initialization, sync]
source_count: 1
---

# LazyLock

## Short Definition

`std::sync::LazyLock<T>` static global value'ni first use paytida safe initialize qiladigan wrapper.

## Why It Matters

`HashMap::new()` kabi non-const initializer'larni global `static`ga olib kirish uchun kerak.

## Mental Model

Container'ni compile time'da e'lon qilasiz, ichidagi haqiqiy value esa runtime'da birinchi accessda yaratiladi.

## Syntax and Examples

```rust
static M: std::sync::LazyLock<
    std::sync::Mutex<std::collections::HashMap<String, i32>>
> = std::sync::LazyLock::new(|| {
    std::sync::Mutex::new(std::collections::HashMap::new())
});
```

## Common Mistakes

- `LazyLock` global state'ni avtomatik yaxshi design qiladi deb o'ylash.
- Ichki lock scope'larini e'tiborsiz qoldirish.

## Related Concepts

- [[oncelock|OnceLock]]
- [[static-items]]
- [[global-data]]

## Sources

- [[wiki/sources/rust-for-backend-developers-global-data]]
