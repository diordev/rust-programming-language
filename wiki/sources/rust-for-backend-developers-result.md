---
title: "Result - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, result]
source_count: 1
---

# Result - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/35. Result.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/result.html

## Detailed Summary

Bu source `Result<T, E>`ni `Option`ning error-aware qarindoshi sifatida beradi: `Option` value yoki absence saqlasa, `Result` success value yoki error value saqlaydi. Muhim durable takeaway: recoverable error Rustda yashirin exception emas, explicit return value.

`square_root(num) -> Result<f32, String>` misoli source'ning eng sodda modeli. Bu yerda `unwrap`, `unwrap_or`, `match`, va `if let` bilan extraction ko'rsatiladi. Lekin bu bobning markazi extraction emas, error representation. String error boshlang'ich nuqta sifatida ishlaydi, keyin source darhol maxsus enum error type'ga o'tadi.

`NameParseError` misoli muhim: errorni string bilan emas, alohida enum bilan modellashtirish programmatic handlingni yaxshilaydi. `EmptyString` va `NoMiddleName` variantlari recoverable branchesni aniq nomlaydi. Bu Rustda "error ham domain data" degan signalni mustahkamlaydi.

Keyingi bo'lim [[std-error-trait|std::error::Error]] haqida. Muhim nuance: `Result<T, E>` har qanday `E` bilan ishlay oladi, lekin standard library `Error` traiti error typelarni bir xil contract ostiga yig'adi. Source qo'lda `Display` va `Error` impl yozadi, lekin caveatni ham ochiq aytadi: real ecosystemda bu ish ko'pincha helper crate yoki macro bilan yengillashtiriladi. Wiki'da aynan shu caveat saqlanishi kerak.

`read_text_file` misoli `Result` compositionni amaliy qiladi. Dastlab manual `match` + `return Err(e)` bilan propagation ko'rsatiladi, keyin `and_then` va `map` combinatorlari bilan qisqartiriladi, oxirida esa `?` operatori bilan linear happy-path qayta tiklanadi. Bu juda muhim progression: combinatorlar mumkin, lekin `?` ko'pincha readability yutadi.

`?` operator bo'limidagi caveat qat'iy saqlanishi kerak: u errorni handle qilmaydi, faqat propagate qiladi. `Ok(v)` bo'lsa value olinadi, `Err(e)` bo'lsa current function `Err(e)` bilan tugaydi. Demak compatible return type shart. Source buni aniq yozadi va wiki'da ham bu noaniqlik qoldirilmaydi.

Oxirgi bo'lim ignored `Result` haqida. Bu ham muhim signal: compiler `Result`ni shunchaki tashlab yuborishni shubhali deb hisoblaydi. Agar e'tiborsiz qoldirish ataylab bo'lsa, `let _ = function_that_may_fail();` bilan explicit discard qilish kerak. Bu `discarded binding` materialini bevosita `Result`ga ulaydi.

## Key Concepts

- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[std-error-trait|std::error::Error]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[discarded-binding]]
- [[unwrap]]
- [[closures]]

## Code Examples

```rust
fn square_root(num: f32) -> Result<f32, String> {
    if num < 0.0 {
        Err("Cannot calculate for negative number".to_string())
    } else {
        Ok(num.sqrt())
    }
}
```

```rust
#[derive(Debug)]
enum NameParseError {
    EmptyString,
    NoMiddleName,
}
```

```rust
fn read_text_file(file_name: &str) -> Result<String, std::io::Error> {
    let mut file = std::fs::File::open(file_name)?;
    let mut contents = String::new();
    use std::io::Read;
    file.read_to_string(&mut contents)?;
    Ok(contents)
}
```

```rust
let _ = function_that_may_fail();
```

## Exercises or Practice Ideas

- String error qaytaradigan bitta functionni custom enum error qaytaradigan variantga refactor qiling.
- Bir xil `Result` flow'ni manual `match`, combinatorlar, va `?` bilan uch xil yozib o'qilishini solishtiring.
- `let _ = ...` ishlatmasdan ignored `Result` warningini chaqirib, keyin explicit discard bilan uni ongli yopib ko'ring.
- `Display` va `std::error::Error` implement qiladigan kichik custom error type yozing.

## Questions Raised

- Qachon string error yetadi, qachon alohida enum error type kerak?
- `and_then` va `?` orasida qachon readability tradeoff paydo bo'ladi?
- Errorni local handle qilish kerakmi yoki callerga propagate qilish kerakmi, buni qaysi context belgilaydi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-result]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[std-error-trait|std::error::Error]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]
- [[discarded-binding]]
- [[unwrap]]
