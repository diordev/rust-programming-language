---
title: "Rust for Backend Developers: Option"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, option, chapter]
source_count: 1
---

# Rust for Backend Developers: Option

## Learning Goal

`Option<T>`ni null surrogate emas, absence modelining type-safe shakli sifatida tushunish; extraction usullari va `map`/`flatten`/`and_then` combinatorlarini amaliy data-flow bilan bog'lash.

## Main Ideas

- `Option<T>` sentinel value, qo'shimcha flag, va null pointer muammolariga Rustning asosiy javobi.
- `Some(T)` va `None` generic enum modeli optional field va optional lookup result'ni tabiiy qiladi.
- `unwrap` qulay shortcut, lekin `None` holatda panic qilishi mumkin; bu error handling emas.
- `match` va `if let` explicit handling beradi, `unwrap_or` esa default value shortcut.
- `map`, `flatten`, va `and_then` optional chaining va optional transformation uchun asosiy combinatorlar.

## Concepts

- [[option|Option]]
- [[generic-enums|generic enums]]
- [[unwrap]]
- [[match]]
- [[if-let|if let]]
- [[closures]]

## Examples

```rust
let value: Option<i32> = Some(5);
let next = value.map(|x| x + 1);
```

```rust
let nested: Option<Option<i32>> = Some(Some(2));
let flat = nested.flatten();
```

```rust
let value = maybe_user.and_then(|user| user.middle_name);
```

## Exercises

- `Option` va empty string bilan bir xil domainni ikki xil model qiling.
- `map(...).flatten()` va `and_then(...)` bilan bir xil natija chiqaring.
- `unwrap_or`, `match`, va `if let`ni bir xil `Option` ustida solishtiring.

## Review Questions

- Nega `Option<T>` sentinel value'dan ko'ra aniqroq signal beradi?
- `unwrap` nega `unsafe` emas, lekin baribir xavfli convenience bo'lishi mumkin?
- `flatten` va `and_then` qaysi muammoni yechadi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[option|Option]]
- [[unwrap]]
- [[match]]
- [[if-let|if let]]
- [[closures]]
