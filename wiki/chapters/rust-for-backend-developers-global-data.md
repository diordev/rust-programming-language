---
title: "Rust for Backend Developers: Global Data"
type: chapter
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, global-data, chapter, advance]
source_count: 1
---

# Rust for Backend Developers: Global Data

## Learning Goal

`const`, `static`, `static mut`, `LazyLock`, `OnceLock`, va synchronized global state orasidagi boundary'larni backend use-case bilan ajratish.

## Main Ideas

- `const` compile-time value, `static` esa bitta shared allocation.
- `static mut` safe default emas; global mutable state synchronization'siz UB yo'nalishiga olib boradi.
- `Mutex<T>` va `RwLock<T>` global data uchun ham ordinary shared-state primitivlari kabi ishlaydi.
- `LazyLock` non-const initializer muammosini runtime first-use init bilan hal qiladi.
- `OnceLock` explicit yoki branch-based initialization uchun mos.
- Session store kabi global registry dizaynida tashqi registry lock va ichki session lock scope'lari alohida boshqarilishi kerak.

## Concepts

- [[global-data]]
- [[global-state]]
- [[constants]]
- [[static-items]]
- [[mutable-static]]
- [[lazylock|LazyLock]]
- [[oncelock|OnceLock]]
- [[mutex-t|Mutex<T>]]
- [[rwlock|RwLock]]
- [[arc-t|Arc<T>]]

## Examples

```rust
static COUNTER: std::sync::Mutex<u64> = std::sync::Mutex::new(0);
```

```rust
static M: std::sync::LazyLock<
    std::sync::Mutex<std::collections::HashMap<String, i32>>
> = std::sync::LazyLock::new(|| {
    std::sync::Mutex::new(std::collections::HashMap::new())
});
```

```rust
static O: std::sync::OnceLock<std::sync::Mutex<String>> =
    std::sync::OnceLock::new();
```

## Exercises

- `static mut` counter'ni `AtomicUsize`, `Mutex<u64>`, va `OnceLock<Mutex<_>>` bilan qayta yozing.
- `LazyLock` va `OnceLock` uchun qaysi init scenariylar mosligini yozing.
- Session store'da `RwLock<HashMap<...>>` va `Arc<Mutex<UserSession>>` rollarini alohida izohlang.

## Review Questions

- Nega `static mut` odatiy mutable global emas?
- `LazyLock` qaysi compile-time cheklovni chetlab o'tadi?
- `OnceLock::set` va `get_or_init` o'rtasidagi semantik farq nima?
- Session registry lock ni nima uchun juda qisqa ushlash kerak?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-global-data]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-multithreading]]
