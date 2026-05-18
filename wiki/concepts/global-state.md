---
title: "Global State"
type: concept
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, globals, state, concurrency]
source_count: 1
---

# Global State

## Short Definition

Runtime davomida o'zgaradigan va programning turli qismlari ulashadigan global data.

## Why It Matters

Mutable global state synchronization, initialization order, va testability muammolarini keskinlashtiradi.

## Mental Model

Muammo "global" bo'lishda emas, "shared mutable" bo'lishda.

## Syntax and Examples

```rust
static SESSIONS: std::sync::LazyLock<
    std::sync::RwLock<std::collections::HashMap<String, std::sync::Arc<std::sync::Mutex<UserSession>>>>
> = std::sync::LazyLock::new(|| std::sync::RwLock::new(std::collections::HashMap::new()));
```

## Common Mistakes

- `static mut`ni direct ishlatish.
- Lock scope'ini haddan tashqari katta qilish.

## Related Concepts

- [[global-data]]
- [[mutable-static]]
- [[mutex-t|Mutex<T>]]
- [[rwlock|RwLock]]

## Sources

- [[wiki/sources/rust-for-backend-developers-global-data]]
