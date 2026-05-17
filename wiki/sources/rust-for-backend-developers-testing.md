---
title: "Тестирование - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, testing]
source_count: 1
---

# Тестирование - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/44. Тестирование.md`
- URL: https://rust-for-backend-developers.github.io/project/testing.html

## Detailed Summary

Bu source Rust testing modelini ikkita qatlamga ajratadi: unit tests va integration tests. Asosiy boundary to'g'ri: unit tests kod bilan bir faylda yashaydi, integration tests esa package root'dagi `tests/` ichida alohida test crate'lar sifatida compile bo'ladi.

Unit test section `#[test]` va `assert_eq!`ni ko'rsatadi. Testning ishlash modeli oddiy: assertion buzilsa panic, runner esa shuni failed deb belgilaydi. `cargo test` barcha testlarni ishga tushiradi; specific testni `cargo test -- --exact ...` bilan filtrlash mumkin.

Source unit test binary'si `target/debug/deps/` ichida alohida executable bo'lishini aytadi. Bu detail beginner uchun foydali, chunki testlar compile-time'da "special magic" emas, alohida harness bilan yig'iladigan executable.

Integration tests bo'limi public API boundary'ni kuchli ko'rsatadi. `tests/*.rs` fayllar crate tashqarisidagi consumer kabi ishlaydi va private itemlarni ko'ra olmaydi. Shuning uchun test qilinadigan code library crate public API'si orqali chiqishi kerak.

`DataSource` trait bilan `DataSourceMock` implementatsiyasi source'dagi eng amaliy testing insight. Rust'da mock ko'pincha framework emas, trait contract'ni test-specific type orqali implement qilish bilan quriladi. Bu especially service/data layer design uchun muhim.

`[dev-dependencies]` qismi test-only library'lar uchun alohida manifest section borligini ko'rsatadi. Bu production dependency graph'ni test tooling bilan iflos qilmaslikka yordam beradi.

`cargo-nextest` qismi standard `cargo test`dan tashqari ecosystem runner'ini tanishtiradi. Key takeaway: nextest ko'proq parallelism control, retry, va reporting beradi; u Rust testing modelini almashtirmaydi, test runner qatlamini kuchaytiradi.

Source ichidagi `main` misolida `let a = 5; a = inc(a);` satri compile bo'lmaydi, chunki binding mutable emas. Wiki buni literal source of truth sifatida emas, testing flow'ni ko'rsatishga xizmat qilgan typo sifatida o'qiydi.

## Key Concepts

- [[testing]]
- [[unit-tests]]
- [[integration-tests]]
- [[test-macros]]
- [[test-filtering]]
- [[dev-dependencies]]
- [[cargo-nextest]]

## Code Examples

```rust
#[test]
fn test_inc_1() {
    assert_eq!(inc(1), 2);
}
```

```rust
struct DataSourceMock;

impl DataSource for DataSourceMock {
    fn find_user_by_id(&self, _id: u64) -> Option<User> {
        Some(User {
            first_name: "John".to_string(),
            last_name: "Doe".to_string(),
        })
    }
}
```

```toml
[dev-dependencies]
```

## Exercises or Practice Ideas

- Bitta small function uchun `#[test]` va `assert_eq!` bilan unit test yozing.
- `tests/` ichida public API'ni ishlatadigan bitta integration test fayli yarating.
- Trait contract asosida oddiy mock implementatsiya yozib, service functionni tekshiring.
- `cargo test` va `cargo nextest run` output farqini solishtiring.

## Questions Raised

- Qachon unit test yetarli, qachon integration test public API darajasida albatta kerak bo'ladi?
- Trait-based mock yondashuvi qaysi joyda qulay, qaysi joyda ortiqcha abstraction bo'ladi?
- `dev-dependencies`ni oddiy dependency qilib yuborish build va maintenance'ga qanday ta'sir qiladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[testing]]
- [[unit-tests]]
- [[integration-tests]]
- [[test-macros]]
- [[test-filtering]]
- [[dev-dependencies]]
- [[cargo-nextest]]

