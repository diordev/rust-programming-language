---
title: "11. Writing Automated Tests"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, unit-tests, integration-tests, assert, tdd]
source_count: 4
---

# 11. Writing Automated Tests

## Learning Goal

Rust'da testlarni qanday yozish, ishlatish va tashkil qilishni o'rganish. `assert!`, `assert_eq!`, `#[should_panic]`, unit tests va integration tests o'rtasidagi farqni tushunish.

## Main Ideas

### Nima uchun test?

Dijkstra (1972): "Test buglarning borligini ko'rsatadi, lekin yo'qligini isbotlay olmaydi." Shunga qaramay testlar muhim — compiler ushlolmagan mantiqiy xatolar uchun.

Rust'da [[correctness]] uch qatlamda ta'minlanadi:
1. Type system — noto'g'ri turdagi qiymatlar
2. Borrow checker — xotira xatolari
3. Testlar — mantiqiy xatolar (masalan, `add_two(3) == 5` kutilishi)

### 11.1 — Test Yozish

**`#[test]` va `cargo test`:**

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

Har bir test yangi thread'da ishlaydi. Thread panic bilan tugasa — FAILED.

**Uch assert macro:**

| Macro | Maqsad |
|-------|--------|
| `assert!(expr)` | `true` bo'lishini tekshiradi |
| `assert_eq!(a, b)` | `a == b` bo'lishini tekshiradi, xatoda ikkalasini chiqaradi |
| `assert_ne!(a, b)` | `a != b` bo'lishini tekshiradi |

`assert_eq!` va `assert_ne!` uchun type `PartialEq` + `Debug` implement qilishi kerak — `#[derive(PartialEq, Debug)]` yetarli.

**Custom failure message:**

```rust
assert!(
    result.contains("Carol"),
    "Greeting did not contain name, value was `{result}`"
);
```

**`#[should_panic]`:**

```rust
#[test]
#[should_panic(expected = "less than or equal to 100")]
fn greater_than_100() {
    Guess::new(200);
}
```

**`Result<T, E>` qaytaruvchi test:**

```rust
#[test]
fn it_works() -> Result<(), String> {
    if add(2, 2) == 4 { Ok(()) }
    else { Err(String::from("two plus two does not equal four")) }
}
```

`?` operator ishlatish imkonini beradi. Lekin `#[should_panic]` bilan birlashtirib bo'lmaydi.

### 11.2 — Testlarni Boshqarish

**Parallel vs ketma-ket:**

```shell
cargo test                     # parallel (default)
cargo test -- --test-threads=1 # ketma-ket
```

Shared state (fayl, env var) bo'lsa parallel testlar o'zaro xalaqit qilishi mumkin.

**Output ushlash:**

O'tgan testlarning `println!` outputi default ushlanadi. Ko'rish uchun:

```shell
cargo test -- --show-output
```

**Test filtrlash:**

```shell
cargo test add           # "add" so'zi bor test nomlarini ishlatadi
cargo test tests::it     # modul + nom prefiks bilan
```

**`#[ignore]` va ignored testlarni ishlatish:**

```rust
#[test]
#[ignore]
fn expensive_test() { ... }
```

```shell
cargo test -- --ignored          # faqat ignored testlar
cargo test -- --include-ignored  # hammasi
```

### 11.3 — Test Tashkillash

| | Unit Tests | Integration Tests |
|---|---|---|
| Joyi | `src/` — kod bilan birga | `tests/` papkasida |
| Kirish | Private + public API | Faqat public API |
| `#[cfg(test)]` | Kerak | Kerak emas |
| Import | `use super::*` | `use crate_name::func;` |

**Unit tests — private funksiyalarni test qilish:**

`mod tests` child module bo'lgani uchun `use super::*` bilan parent modulning private itemlari ko'rinadi.

**Integration tests:**

```
adder/
├── src/lib.rs
└── tests/
    └── integration_test.rs
```

```rust
// tests/integration_test.rs
use adder::add_two;

#[test]
fn it_adds_two() {
    assert_eq!(add_two(2), 4);
}
```

Integration tests faqat `src/lib.rs` bo'lgan library cratelarda ishlaydi. Binary-only crate (`src/main.rs`) uchun integration test bo'lmaydi — shuning uchun logika `src/lib.rs`ga chiqariladi.

**Umumiy test helper kodi:**

`tests/common/mod.rs` yoki `tests/common.rs` — `tests/` ichidagi umumiy funksiyalar uchun. `mod.rs` uslubi afzal — alohida test file sifatida ko'rinmaydi.

```rust
// tests/common/mod.rs
pub fn setup() { ... }

// tests/integration_test.rs
mod common;
common::setup();
```

**Faqat bitta integration test faylini ishlatish:**

```shell
cargo test --test integration_test
```

## Concepts

- [[testing]]
- [[cfg-test|#[cfg(test)] va mod tests]]
- [[test-macros|test macros (assert!, assert_eq!, assert_ne!)]]
- [[should-panic|#[should_panic]]]
- [[test-filtering|test filtering]]
- [[unit-tests|unit tests]]
- [[integration-tests|integration tests]]
- [[correctness]]
- [[result|Result]] — testlarda qaytarish
- [[tdd|Test-Driven Development]]

## Examples

- Adder library unit testi: `assert_eq!(add(2,2), 4)`
- Rectangle `can_hold` testi: `assert!(larger.can_hold(&smaller))`
- Greeting custom message testi
- Guess `#[should_panic(expected = ...)]`
- Integration test bilan `adder::add_two`

## Exercises

- `add_two` funksiyasida qasddan bug kiritib `assert_eq!` failure xabarini o'rganing.
- `#[should_panic(expected = "...")]` bilan aniq panic xabarini tekshiring.
- `Result<T, E>` qaytaruvchi test yozing va `?` bilan xatolarni propagate qiling.
- `--test-threads=1` bilan parallel muammosini simulatsiya qiluvchi test yozing.
- Shared fayl ishlatadigan ikkita test yozing va parallel xalaqitni kuzating.
- `tests/` papkasida integration test yarating va library funksiyangizni sinang.

## Review Questions

1. `#[test]` va `#[cfg(test)]` qanday farq qiladi?
2. `assert_eq!` xato bo'lganda nima chiqaradi va buning uchun qanday trait kerak?
3. `#[should_panic(expected = "...")]` va `#[should_panic]` qanday farq qiladi?
4. Parallel testlarning qachon muammo chiqarishi mumkin va yechimi nima?
5. Unit test `private` funksiyaga kira oladimi? Nima uchun?
6. Integration test nima uchun `#[cfg(test)]` talab qilmaydi?
7. `tests/common/mod.rs` va `tests/common.rs` qanday farq qiladi?
8. Binary-only crate uchun integration test yozib bo'lmaydi — nima uchun?

## Related Pages

- [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]] — oldingi bob
- [[12-an-io-project|12. An I/O Project]] — keyingi bob
- [[wiki/sources/11-writing-automated-tests]] (source)
- [[11-1-how-to-write-tests]] (source 11.1)
- [[11-2-controlling-how-tests-are-run]] (source 11.2)
- [[11-3-test-organization]] (source 11.3)
