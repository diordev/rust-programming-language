---
title: "Паттерн матчинг - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, match]
source_count: 1
---

# Паттерн матчинг - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/30. Паттерн матчинг.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/pattern-matching.html

## Detailed Summary

Bu source `match`ni oddiy branching emas, shape-aware control flow sifatida beradi. Kirishdagi "switch-case + destructuring hybrid" ta'rifi beginner uchun foydali, lekin durable model yanada aniqroq: [[match]] value'ni patternlar bilan solishtiradi, birinchi mos armni bajaradi, va butun expression sifatida value qaytaradi.

Source avval constant pattern, `_`, named catch-all, `|`, va range patternlarni beradi. Bu qismning asosiy barqaror nuqtasi exhaustive matchingdir: `match` branchlari barcha ehtimoliy holatlarni yopishi kerak. `_` default branch qulay, lekin uni erta ishlatish compilerning yangi case'lar bo'yicha foydali signalini yo'qotishi mumkin.

`@` binding bo'limi pattern syntaxdagi muhim qulaylikni ko'rsatadi: "range'ga tushdimi?" va "o'sha qiymatning o'zi nima edi?" degan ikki savolni bitta armda hal qilish. Bu `match`ni faqat test operatori emas, binding vositasi ham ekanini yana bir bor ko'rsatadi.

`match`ning expression sifatida ishlashi ham yaxshi ko'rsatilgan. `absolute = match a { ..0 => -a, _ => a }` misoli control flow bilan value production Rustda bitta model ekanini mustahkamlaydi. Bu `if`ning expression bo'lishi bilan bir oilaga kiradi.

String matching bo'limi alohida muhim, chunki u pattern syntaxdagi nozik xatoni ko'rsatadi. `match name { anonymous => ... }` tashqi `anonymous` qiymatini solishtirmaydi; u yangi binding yaratadi. Shu sabab string content'ni solishtirish uchun literal (`name.as_str()`) yoki [[match-guard|match guard]] ishlatiladi. Wiki uchun saqlanadigan xulosa: bare identifier pattern ko'pincha comparison emas, binding.

Slice pattern bo'limi source ichidagi eng foydali bridge'lardan biri: oddiy destructuringda ishlamagan shape matching `match`da qaytib keladi. `[]`, `[a, b, c, ..]`, `_` arm'lari bilan `&[T]`ni branch qilish mumkin, chunki mismatch recovery keyingi armga o'tadi. Shu yerda `match` plain bindingdan kuchliroq pattern context ekanini ko'ramiz.

Struct pattern, [[match-guard|guard]], va [[ref-pattern|ref/ref mut]] bo'limlari esa destructuringni branch semantics bilan birlashtiradi. `Person { name, age: 1..18 }` field value'ni ham shape'ni ham tekshiradi. Guard arm ichida qo'shimcha boolean filter beradi. `ref` esa match by value qilinayotgan joyda fieldni move qilmay, reference bilan ushlab qolishga yordam beradi. Practical nuance shuki, `match &mut person { ... }` ko'pincha o'qish uchun sodda; `ref mut` esa pattern ichida aynan reference binding kerak bo'lganda foydali.

## Key Concepts

- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[or-pattern|or-pattern (`|`)]]
- [[at-binding|@ binding]]
- [[match-guard|match guard]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]
- [[pattern-destructuring|pattern destructuring]]
- [[string-slice|String slice]]

## Code Examples

```rust
match 22 + 22 {
    x @ 10..100 => println!("{x} is in range [10,99]"),
    x => println!("{x} is outside the range"),
}
```

```rust
let name = String::from("Robert Smith");
let is_anonymous = match name.as_str() {
    "Anonymous" | "John Doe" => true,
    _ => false,
};
```

```rust
let v = vec![1, 2, 3, 4, 5];
let sum = match v.as_slice() {
    [] => 0,
    [a, b, c, ..] => a + b + c,
    _ => -1,
};
```

## Exercises or Practice Ideas

- Numeric range, `@` binding, va catch-all arm ishlatadigan `match` yozing.
- `String` inputni `as_str()` orqali bir nechta command literalga match qiling.
- `&[u8]` uchun `[]`, `[one]`, `[first, second, ..]` ko'rinishidagi protocol parser skeleton yozing.
- `match &mut value` va `match value { ref mut ... }` variantlarini bir xil struct bilan solishtiring.

## Questions Raised

- Qachon `match`ni `if let` yoki `let...else`ga qisqartirish kerak, qachon to'liq saqlash kerak?
- Guard arm ko'payganda kod qayerdan boshlab haddan tashqari imperative ko'rinadi?
- `ref` patternni ishlatish readability'ni oshiradimi yoki `&value` orqali borrow qilish aniqroqmi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-pattern-matching]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]
