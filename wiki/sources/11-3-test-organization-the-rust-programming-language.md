---
title: "11.3. Test Organization"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, unit-tests, integration-tests]
source_count: 1
---

# 11.3. Test Organization

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: Chapter 11.3
- URL: https://doc.rust-lang.org/stable/book/ch11-03-test-organization.html
- Raw fayl: `raw/books/the_rust_programming_language/11.3. Test Organization - The Rust Programming Language.md`

## Detailed Summary

### Ikki kategoriya

Rust community testlarni ikki kategoriyaga ajratadi:

| | Unit tests | Integration tests |
|---|---|---|
| Joyi | `src/` — kod bilan birga | `tests/` — loyiha ildizida |
| Miqyosi | Bitta modul, izolyatsiyada | Bir nechta modul birgalikda |
| Interface | Private va public | Faqat public API |
| `#[cfg(test)]` | Kerak | Kerak emas |
| Import | `use super::*` | `use crate_name::fn_name;` |

### Unit Tests

#### `#[cfg(test)]` va `mod tests`

`#[cfg(test)]` annotatsiyasi test modulini faqat `cargo test`da compile qilishni bildiradi — `cargo build` va `cargo build --release`da yo'q. Bu ikkita afzallik beradi:

1. **Compile vaqti** — test kodi production buildda compile qilinmaydi.
2. **Binary hajmi** — release binary'ga test kodi kirmaydi.

Integration tests `tests/` papkasida bo'lgani uchun ular `#[cfg(test)]`siz ishlaydi — Cargo bu papkani o'zi boshqaradi.

```rust
// src/lib.rs
pub fn add_two(a: u64) -> u64 {
    internal_adder(a, 2)
}

fn internal_adder(left: u64, right: u64) -> u64 {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn internal() {
        let result = internal_adder(2, 2);
        assert_eq!(result, 4);
    }
}
```

#### Private funksiyalarni test qilish

`mod tests` child module. `use super::*` parent modulning private itemlarini ham scopega olib kiradi. Shuning uchun `internal_adder` — `pub` bo'lmasa ham — testda chaqirilishi mumkin.

Bu Rust privacy qoidalariga to'liq mos: child module parent itemlarini ko'radi. Testlash jamoasida private funksiyalarni to'g'ridan-to'g'ri test qilish haqida bahslar bor — Rust sizi majburlamaydi, lekin imkon beradi.

### Integration Tests

Integration testlar kutubxonaga tashqaridan qaraydi — xuddi boshqa crate kabi faqat public API ishlatadi. Maqsad: modullar birlikda to'g'ri ishlashini tekshirish.

#### `tests/` papkasi

```
adder/
├── Cargo.lock
├── Cargo.toml
├── src/
│   └── lib.rs
└── tests/
    └── integration_test.rs
```

`tests/` papkasidagi har bir fayl — alohida crate. Shuning uchun kutubxonani import qilish kerak:

```rust
// tests/integration_test.rs
use adder::add_two;

#[test]
fn it_adds_two() {
    let result = add_two(2);
    assert_eq!(result, 4);
}
```

`#[cfg(test)]` kerak emas — Cargo `tests/` papkasini faqat `cargo test`da compile qiladi.

#### `cargo test` output — uch bo'lim

```
running 1 test
test tests::internal ... ok
test result: ok. 1 passed; ...

     Running tests/integration_test.rs
running 1 test
test it_adds_two ... ok
test result: ok. 1 passed; ...

   Doc-tests adder
running 0 tests
test result: ok. ...
```

**Muhim:** Birinchi bo'lim (unit tests) FAILED bo'lsa, integration tests va doc-tests ishlamaydi.

Faqat bitta integration test faylini ishlatish:

```shell
cargo test --test integration_test
```

#### Shared helper kodi — submodule pattern

Bir nechta integration test faylida umumiy helper kerak bo'lsa, `tests/common.rs` yaratish **noto'g'ri** — Cargo uni alohida crate sifatida ko'radi, `running 0 tests` chiqadi.

**To'g'ri yechim — `tests/common/mod.rs`:**

```
tests/
├── common/
│   └── mod.rs    ← subdirectory ichida, separate crate emas
└── integration_test.rs
```

```rust
// tests/common/mod.rs
pub fn setup() {
    // test uchun umumiy tayyorlov kodi
}
```

```rust
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

Subdirectory ichidagi fayllar separate crate sifatida compile qilinmaydi — test outputda ko'rinmaydi.

#### Binary crate cheklovi

`src/main.rs` faqat binary crate — boshqa cratelar `use` bilan uning funksiyalarini import qila olmaydi. Shuning uchun integration test yozib bo'lmaydi.

**Yaxshi dizayn pattern:** Logika `src/lib.rs`da, `src/main.rs` faqat lib'ni chaqiradi:

```
src/
├── lib.rs    ← asosiy logika; integration tests shu yerni test qiladi
└── main.rs   ← lib'ni chaqiradi; minimal kod, test shart emas
```

## Key Concepts

- [[unit-tests]] — `src/` ichida, private access, `#[cfg(test)]`
- [[integration-tests]] — `tests/` papkasida, faqat public API
- [[cfg-test]] — `#[cfg(test)]` annotation maqsadi
- [[testing]] — ikkala kategoriyaning umumiy ko'rinishi

## Code Examples

- **Private function test** — Listing 11-12: `internal_adder`
- **Integration test** — Listing 11-13: `tests/integration_test.rs`
- **Shared helper** — `tests/common/mod.rs` pattern

## Exercises or Practice Ideas

- `adder` library yarating, unit test va integration test ikkisini ham yozing.
- `internal_adder` kabi private funksiya yaratib, uni unit test orqali to'g'ridan-to'g'ri test qiling.
- `tests/common/mod.rs` vs `tests/common.rs` farqini `cargo test` outputida kuzating.
- Binary + library pattern: `src/lib.rs` va `src/main.rs` ni ajratiб, integration test yozing.

## Questions Raised

- Doc-tests qanday yoziladi? (Chapter 14)
- Test coverage tools Rust'da qanday ishlaydi?
- `#[cfg(test)]` bilan conditional compilation boshqa holatlarda qanday ishlatiladi?

## Links To Update

- [[unit-tests]] — yangi sahifa
- [[integration-tests]] — yangi sahifa
- [[testing]] — unit/integration farqini qo'shish
- [[index]], [[overview]]
