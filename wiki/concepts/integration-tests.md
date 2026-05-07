---
title: "Integration Tests"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, integration-tests]
source_count: 1
---

# Integration Tests

## Short Definition

Integration tests — kutubxona public API'sini tashqaridan, xuddi boshqa crate kabi test qiluvchi testlar. `tests/` papkasida yoziladi; har bir fayl alohida crate.

## Why It Matters

Alohida to'g'ri ishlaydigan modullar birgalikda muammo yaratishi mumkin. Integration tests shu "integratsiya" nuqtasini tekshiradi. Shuningdek, public API'ning haqiqiy foydalanuvchi nuqtai nazaridan ishlashini isbotlaydi.

## Mental Model

Integration test — kutubxonangizni ishlatadigan tashqi foydalanuvchi. U faqat `pub` deb belgilangan narsalarni ko'radi. `tests/` papkasidagi har bir fayl alohida crate bo'lgani uchun `use crate_name::fn_name` kerak — `use super::*` emas.

## Syntax and Examples

### Papka tuzilishi

```
adder/
├── Cargo.toml
├── src/
│   └── lib.rs
└── tests/
    └── integration_test.rs
```

### Asosiy integration test

```rust
// tests/integration_test.rs
use adder::add_two;   // kutubxonani import qilish kerak

#[test]
fn it_adds_two() {
    let result = add_two(2);
    assert_eq!(result, 4);
}
```

`#[cfg(test)]` kerak emas — Cargo `tests/` papkasini faqat `cargo test`da compile qiladi.

### Faqat bitta integration test faylini ishlatish

```shell
cargo test --test integration_test
```

### Shared helper kodi — `tests/common/mod.rs` pattern

**Xato yo'l** — `tests/common.rs` alohida crate sifatida ko'rinadi, outputda `running 0 tests` chiqadi:

```
tests/
└── common.rs          ← XATO: separate crate, outputda ko'rinadi
```

**To'g'ri yo'l** — subdirectory ichidagi `mod.rs`:

```
tests/
├── common/
│   └── mod.rs         ← TO'G'RI: separate crate emas, outputda yo'q
└── integration_test.rs
```

```rust
// tests/common/mod.rs
pub fn setup() {
    // umumiy tayyorlov
}

// tests/integration_test.rs
use adder::add_two;
mod common;

#[test]
fn it_adds_two() {
    common::setup();
    let result = add_two(2);
    assert_eq!(result, 4);
}
```

### Binary crate cheklovi

`src/main.rs` faqat binary crate — `use` bilan import qilib bo'lmaydi. Yaxshi dizayn: logika `src/lib.rs`da, `src/main.rs` minimal.

```
src/
├── lib.rs    ← integration test shu yerni test qiladi
└── main.rs   ← lib'ni chaqiradi, minimal
```

### `cargo test` output tuzilishi

```
# 1. Unit tests
running 1 test
test tests::internal ... ok

# 2. Integration tests (har bir fayl alohida bo'lim)
     Running tests/integration_test.rs
running 1 test
test it_adds_two ... ok

# 3. Doc-tests
   Doc-tests adder
running 0 tests
```

Unit tests FAILED bo'lsa, integration tests va doc-tests ishlamaydi.

## Common Mistakes

- `use super::*` ishlatishga urinish — integration test alohida crate, `super` yo'q.
- `#[cfg(test)]` qo'shish — kerak emas, Cargo o'zi boshqaradi.
- Shared helper uchun `tests/common.rs` yaratish — outputda shovqin chiqaradi; `tests/common/mod.rs` ishlatish kerak.
- Binary crate uchun integration test yozishga urinish — `src/main.rs` import qilib bo'lmaydi.

## Related Concepts

- [[unit-tests]]
- [[testing]]
- [[cfg-test]]
- [[module-file-layout]]
- [[library-crate]]
- [[binary-crate]]
- [[public-api]]

## Sources

- [[11-3-test-organization-the-rust-programming-language]]
