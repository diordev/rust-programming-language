---
title: "Обработка ошибок - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, source, error-handling, advance]
source_count: 1
---

# Обработка ошибок - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/54. Обработка ошибок.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/error-handling.html

## Detailed Summary

Bu source Rust Book'dagi basic `Result` modelidan keyingi amaliy backend error handling qatlamini ochadi. Fikr markazi to'g'ri: library/domain code va final application code bir xil error shape ishlatmasligi mumkin.

Birinchi bo'lim `thiserror`. Source `ReservationService::reserve` uchun domain-specific `ReserveError` enum yaratadi: `NoSuchProduct` va `NotEnough`. Bu yerda eng foydali signal shuki, custom error enum oddiy stringdan ancha kuchli API beradi: caller variant bo'yicha branch qila oladi, structured payload oladi, va failure semantics signature darajasida ko'rinadi.

Source qo'lda `Display` va `std::error::Error` impl yozib ko'rsatadi, keyin `thiserror` bilan shu boilerplate'ni qisqartiradi. `#[derive(Debug, Error)]` va variant ustidagi `#[error("...")]` annotation'lar bu crate'ning asosiy value proposition'i. Durable takeaway: `thiserror` error typing'ni o'zgartirmaydi; faqat standard ecosystem trait impl'larini ergonomik qiladi.

`thiserror = "1"` dependency misoli source snapshot sifatida saqlanadi. Wiki buni "latest" versiya deb yozmaydi. Version string source kontekstida qoladi.

Reservation service implementation misolida `Mutex<HashMap<u64, u64>>` va `AtomicU64` ishlatiladi. Bu yerda error handling bilan concurrency birlashadi: service lock ichida inventory tekshiradi va `Result<Reservation, ReserveError>` qaytaradi. `AtomicU64` bilan ID generatsiyasi ham domain success branch ichida turadi.

Keyingi bo'lim error wrapping. Source `ReservationService`, `ShipmentService`, va `PurchaseService`ni birlashtiradi. Eng muhim pattern `PurchaseError`:

- `ReservationFailed(#[from] ReserveError)`
- `ShippingFailed(#[from] ShipmentError)`

Bu yerda `#[from]` orqali `From<ReserveError> for PurchaseError` va `From<ShipmentError> for PurchaseError` generate qilinadi. Natija: `?` operator lower-level error'ni qo'lda `map_err` yozmasdan higher-level API error'ga o'raydi.

Wiki caveat: source snippet'da matn xatolari bor. `Nested servation error: (0)` va `Nested shipping error: (0)` ehtimol `Nested reservation error: {0}` va `Nested shipping error: {0}` bo'lishi kerak. Yana `impl From<ReservationError> for PurchaseError` deb yozilgan satr ham ehtimol `ReserveError` bo'lishi kerak. Bular source artifact sifatida qayd qilinadi, canonical syntax sifatida emas.

Wrapping bo'limining asosiy durable g'oyasi kuchli: errorni qayta o'rash bilan siz sababni yo'qotmaysiz, lekin current API uchun semantically to'g'ri error shape olasiz. Bu ayniqsa library/service boundary uchun foydali.

Keyin source `Box<dyn Error>`ga o'tadi. Bu bo'lim amaliy application-layer tradeoffni ko'rsatadi: ba'zi joyda har error turi bo'yicha alohida action yo'q, shuning uchun "log + 500" uslubi kifoya. Shu vaziyatda error typing'ni batafsil davom ettirish ortiqcha bo'lishi mumkin. `Box<dyn Error>` turli concrete error'larni bitta trait object ostida qaytarish imkonini beradi.

Muhim nuance: `?` bu yerda ham ishlaydi, chunki mos `From` conversion chain mavjud. `fail_a()?` va `fail_b()?` `Box<dyn Error>`ga ko'tariladi. Source buni explicit `map_err(|e| Box::new(e) as Box<dyn Error>)`ga tenglashtirib tushuntiradi.

Lekin source shu yerda to'xtamaydi va `downcast_ref` bilan `dyn Error` ichidagi concrete type'ni tekshiradi. Durable takeaway: erased error qaytarsangiz ham ba'zi aniq turlarni special-case qila olasiz, lekin bu default pattern emas; type erasure'dan keyin branching murakkablashadi.

So'ng `anyhow` bo'limi keladi. Source `anyhow::Error` va `anyhow::Result<T>`ni `Box<dyn Error>` bilan bir xil maqsadning ergonomik versiyasi sifatida ko'rsatadi. Muhim boundary saqlanadi: `anyhow` application-level simplification, `thiserror` esa library/domain API uchun mosroq.

`anyhow` bilan `downcast_ref::<MyError>()`, `root_cause()`, `backtrace()`, va `.context(...)` ko'rsatiladi. Bu yerda manbaning eng foydali qismi `context`: original `io::Error` matni "No such file or directory" bo'lsa, `.context("Cannot read non_existing_file.txt")` caller logini ancha informativ qiladi.

Backtrace bo'limida source `RUST_LIB_BACKTRACE=1` environment variable'ni ko'rsatadi va `anyhow::Error` stack trace capture qila olishini aytadi. Wiki bunu source-specific operational note sifatida saqlaydi; exact formatting va environment behavior toolchain/versionga bog'liq bo'lishi mumkin.

Source yakuniy tavsiyasi aniq va foydali:

- library yozsangiz - detailed concrete error types, odatda `thiserror`
- final application code'da typed recovery kerak bo'lmagan joylarda - `anyhow`

Bu ajratish wiki'da alohida saqlanadi, chunki amaliy Rust codebase'larda aynan shu boundary ko'p chalkashadi.

## Key Concepts

- [[error-handling]]
- [[custom-error-enum]]
- [[std-error-trait|std::error::Error]]
- [[error-wrapping]]
- [[from-trait]]
- [[question-mark-operator|question mark operator]]
- [[box-dyn-error|Box<dyn Error>]]
- [[error-downcasting]]
- [[root-cause]]
- [[error-context]]
- [[backtrace]]

## Code Examples

```rust
#[derive(Debug, thiserror::Error)]
enum ReserveError {
    #[error("No product with ID {id}")]
    NoSuchProduct { id: u64 },
    #[error("Asked {asked}, but available {available}")]
    NotEnough { asked: u64, available: u64 },
}
```

```rust
#[derive(Debug, thiserror::Error)]
enum PurchaseError {
    #[error("Nested reservation error: {0}")]
    ReservationFailed(#[from] ReserveError),
    #[error("Nested shipping error: {0}")]
    ShippingFailed(#[from] ShipmentError),
}
```

```rust
fn fail_something(is_a: bool) -> Result<(), Box<dyn std::error::Error>> {
    if is_a { fail_a()?; } else { fail_b()?; }
    Ok(())
}
```

```rust
use anyhow::Context;

let text = std::fs::read_to_string("non_existing_file.txt")
    .context("Cannot read non_existing_file.txt")?;
```

## Exercises or Practice Ideas

- `ReserveError`ga yana bitta variant qo'shib, `thiserror` bilan message formatini yozing.
- `PurchaseError`ga `#[from]` qo'yilgan va qo'yilmagan ikki variantni solishtiring.
- `Box<dyn Error>` qaytargan functionda `downcast_ref` bilan bitta specific error'ni ushlang.
- `anyhow::Context` bilan bir xil `io::Error`ga ikki xil context berib log outputni solishtiring.

## Questions Raised

- Domain layer concrete enum qaytarishi kerakmi yoki allaqachon erased error qaytarsinmi?
- `Box<dyn Error>` yetarlimi yoki `anyhow` ergonomikasi qiymatlimi?
- Qaysi boundary'da `context` qo'shish foydali, qaysi boundary'da shovqin?
- `downcast_ref` ishlatilayotgan bo'lsa, error type juda erta yo'qotilmayaptimi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-error-handling]]
- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[custom-error-enum]]
- [[error-wrapping]]
- [[error-context]]
- [[error-downcasting]]
- [[root-cause]]
- [[std-error-trait|std::error::Error]]
- [[box-dyn-error|Box<dyn Error>]]
- [[backtrace]]
- [[wiki/crates/thiserror]]
- [[wiki/crates/anyhow]]
