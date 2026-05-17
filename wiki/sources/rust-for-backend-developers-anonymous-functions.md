---
title: "Анонимные функции - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, closures]
source_count: 1
---

# Анонимные функции - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/31. Анонимные функции.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/anonymous-functions.html

## Detailed Summary

Bu source Rustdagi "anonymous function" mavzusini ikki qatlamga ajratadi: capture qilmaydigan funksional literal va outer scope'dan qiymat ushlaydigan [[closures]]. Durable nuqta shuki, source boshidagi "anonymous function" terminology soddalashtirilgan; amalda capture yo'q bo'lsa `fn(...) -> ...` function pointerga koerce bo'ladigan callable haqida gapiryapmiz, capture bo'lsa esa compiler closure object generatsiya qiladi.

Kirish qismidagi `|args| body` syntax, type inference, va single-expression closure syntax Rustning daily ergonomics qismini beradi. Bu yerda saqlanadigan practical signal: ko'p joyda argument/return type annotation kerak bo'lmaydi, lekin birinchi usage concrete type'ni qotiradi. Pure anonymous function `let inc: fn(i32) -> i32 = |x| x + 1;` shaklida plain [[function-pointers|function pointer]] sifatida ishlashi mumkin.

`higher-order function` bo'limi source'ning foydali markazlaridan biri. `transform(a, f)` misoli functionni argument sifatida qabul qiladigan API'ni ko'rsatadi, `create_inc() -> fn(i32) -> i32` esa non-capturing callable'ni return qilish mumkinligini ko'rsatadi. Bu yerda muhim chegara darhol chiqadi: plain `fn` faqat environment capture qilmaydigan callable uchun ishlaydi.

Keyingi bo'lim aynan shu chegarani ochadi: `step`ni ishlatadigan `make_inc_with_step` endi `fn` qaytara olmaydi, chunki bu oddiy function pointer emas, [[closures]] bo'ladi. Source to'g'ri buriladi: return type `impl Fn(i32) -> i32`ga o'tadi va `move` qo'shiladi. Durable model: closure bu "code + captured environment" kombinatsiyasi; `move` outer value ownershipini closure ichiga o'tkazishni explicit qiladi. `move` har doim kerak emas, lekin closure outer scope'dan uzoqroq yashasa yoki value'ni by-value ichkariga olib kirish kerak bo'lsa kerak bo'ladi.

`Fn`, `FnMut`, `FnOnce` bo'limi bu source'dagi eng nozik qism. Muhim caveat: bu uch trait thread-safety kategoriyasi emas. Ular closure'ni qanday chaqirish mumkinligini bildiradi: faqat o'qishmi, mutatsiyami, yoki consume qilib yuborishmi. Source buni usage-based model bilan deyarli to'g'ri beradi: trait closure qanday capture qilgani bilan emas, captured value'dan closure body qanday foydalangani bilan aniqlanadi. Shuning uchun concurrency uchun hali ham alohida [[send-trait|Send]], [[sync-trait|Sync]], yoki `'static` kabi boundlar kerak bo'lishi mumkin.

`FnMut` misollari source'da foydali: capture mutable reference bo'lishi shart emas, by-value capture qilingan state ham closure ichida mutatsiya qilinsa natija baribir `FnMut` bo'ladi. Bu beginner uchun muhim tuzatish: classification capture form emas, behaviorga bog'liq.

`FnOnce` qismi resource-consuming closure modelini beradi: captured `String`ni ichkarida by-value uzatib yuborilsa, closure ikkinchi marta chaqirilmaydi. Bu yerda ownership va closure semantics to'liq birlashadi.

Oxirgi bo'lim `impl Fn` va concrete closure type muammosini tushuntiradi. Har closure alohida concrete type. Shu sabab bitta function ichida ikki xil closure branchini `impl Fn` bilan qaytarib bo'lmaydi; common interface kerak bo'lsa `Box<dyn Fn(...) -> ...>` ishlatiladi. Bu source'dagi eng muhim durable takeaway'lardan biri, chunki u [[opaque-types|opaque types]] va [[trait-object|trait object]] o'rtasidagi farqni juda amaliy ko'rinishda beradi.

## Key Concepts

- [[closures]]
- [[function-pointers|function pointers]]
- [[higher-order-functions|higher-order functions]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]
- [[impl-trait|impl Trait]]
- [[trait-object|trait object]]
- [[move-semantics|move semantics]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]

## Code Examples

```rust
let inc: fn(i32) -> i32 = |x: i32| x + 1;
```

```rust
fn transform(a: i32, f: fn(i32) -> i32) -> i32 {
    f(a)
}
```

```rust
fn make_inc_with_step(step: i32) -> impl Fn(i32) -> i32 {
    move |x| x + step
}
```

```rust
fn make_inc(is_decrement: bool) -> Box<dyn Fn(i32) -> i32> {
    if is_decrement {
        Box::new(|x| x - 1)
    } else {
        Box::new(|x| x + 1)
    }
}
```

## Exercises or Practice Ideas

- `transform`ga o'xshash bitta higher-order function yozib, named function va closure bilan chaqiring.
- Non-capturing closure'ni `fn` pointer sifatida, capturing closure'ni esa `impl Fn` sifatida qaytaring.
- `FnMut` va `FnOnce` orasidagi farqni ko'rsatadigan ikkita kichik example yozing.
- Ikki branchda turli closure qaytarib, nega `Box<dyn Fn>` kerak bo'lishini tekshiring.

## Questions Raised

- Qachon API `fn(...)` qabul qilishi kerak, qachon `F: Fn(...)` ochiqroq bo'ladi?
- `move`ni qachon explicit yozish kerak, qachon compiler capture mode'ni o'zi hal qilishi yetarli?
- `Fn`/`FnMut`/`FnOnce` classificationni thread-safety emas, callability modeli sifatida qanday eslab qolish qulay?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-anonymous-functions]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[closures]]
- [[function-pointers|function pointers]]
- [[higher-order-functions|higher-order functions]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]
