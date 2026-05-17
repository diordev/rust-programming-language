---
title: "Функции - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, functions]
source_count: 1
---

# Функции - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/19. Функции.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/functions.html

## Detailed Summary

Bu source Rust function modelini syntaxdan boshlab control-flow va type theorygacha kengaytiradi. Function signatures typed: parameterlar type annotation talab qiladi, return type `->` bilan ko'rsatiladi, final expression esa default return value bo'ladi.

`return` early exit uchun ishlatiladi. Source `safe_divide` misolida branch ichida erta chiqishni ko'rsatadi. Shu bilan birga final expressionga tayanadigan style Rustda default ekanini mustahkamlaydi.

Qiziqroq qism: function ichida inner function e'lon qilish mumkin. Source Fibonacci helper misoli bilan buni ko'rsatadi. Bu nested helper external API bo'lmagan, lekin reuse kerak bo'lgan logic uchun ishlatilishi mumkin.

Source `return` va never type `!` o'rtasidagi bog'lanishni ham ochadi. `return` current functiondan boshqaruvni olib chiqadi, shuning uchun u expression kontekstida `!` sifatida ko'riladi va boshqa branch type'lari bilan "yelim" bo'lib ishlaydi.

`const fn` compile-time evaluation imkonini beradi. Example `TAU = double(PI)` bilan beriladi. Oxirgi bo'lim function ichidagi `static mut`ni ko'rsatadi va bu unsafe boundary ekanini data race xavfi bilan izohlaydi. Amaliy jihatdan source o'zi ham to'g'ri xulosa qiladi: backend applicationlarda bunday pattern odatda ishlatilmaydi.

## Key Concepts

- [[functions]]
- [[statements-and-expressions|statements and expressions]]
- [[return-values|return values]]
- [[never-type|never type (!)]]
- [[static-items]]
- [[unsafe-rust|unsafe Rust]]
- [[data-race|data race]]

## Code Examples

```rust
fn sum(a: i32, b: i32) -> i32 {
    a + b
}
```

```rust
fn safe_divide(a: f32, b: f32) -> f32 {
    if b == 0.0 {
        return 0.0;
    }
    a / b
}
```

```rust
const TAU: f32 = double(3.14);

const fn double(num: f32) -> f32 {
    num * 2.0
}
```

```rust
fn sum_with_previous(x: i32) -> i32 {
    static mut PREV: i32 = 0;
    unsafe {
        let result = PREV + x;
        PREV = x;
        result
    }
}
```

## Exercises or Practice Ideas

- Final expression va explicit `return` ishlatadigan ikki function yozing.
- Inner helper function bilan bo'laklangan bitta amaliy function yozing.
- `const fn` bilan compile-time hisoblangan constant yarating.

## Questions Raised

- Inner function va alohida private function orasida qachon qaysi biri to'g'ri?
- `return`ning `!` bilan bog'lanishini yangi o'quvchiga qayergacha ochish kerak?
- `static mut` misoli ta'limiy foydali, lekin real code uchun zararli odatga aylanib ketmaydimi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-functions]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[functions]]
- [[never-type|never type (!)]]
- [[static-items]]
