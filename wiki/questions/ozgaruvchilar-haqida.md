---
title: "Rustda o'zgaruvchilar haqida"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, variables]
source_count: 1
---

# Rustda o'zgaruvchilar haqida

## Answer

Rustda variable aslida "binding" hisoblanadi: nom (`let`) ma'lum bir value'ga ulanadi.

```rust
let x = 5;
```

Rustda variables default immutable. Bu `x` ga keyin qayta assignment qilinmaydi degani:

```rust
let x = 5;
// x = 6; // error[E0384]
```

Agar value keyin o'zgarishi kerak bo'lsa, `mut` yoziladi:

```rust
let mut x = 5;
x = 6;
```

`mut` faqat compilerga ruxsat emas, code readerga ham "bu value o'zgaradi" degan signal. Shu sababli Rustda `mut`ni faqat haqiqatan kerak joyda ishlatish yaxshi.

Variable type'i ko'pincha [[type-inference|type inference]] orqali aniqlanadi:

```rust
let count = 42; // odatda i32 deb infer qilinadi
```

Ambiguous joylarda [[type-annotations|type annotation]] yoziladi:

```rust
let guess: u32 = "42".parse().expect("number");
```

[[shadowing]] esa bir xil nom bilan yangi binding yaratishdir. Bu mutation emas:

```rust
let spaces = "   ";
let spaces = spaces.len();
```

Bu yerda birinchi `spaces` type'i `&str`, ikkinchisi esa `usize`. `mut` bilan type'ni bunday o'zgartirib bo'lmaydi.

`const` bilan e'lon qilingan [[constants|constant]] har doim immutable bo'ladi, type annotation talab qiladi va compile-time expression bo'lishi kerak:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Qisqa qoida:

- oddiy, o'zgarmaydigan value uchun `let`
- keyin o'zgaradigan value uchun `let mut`
- compile-time constant uchun `const`
- transformationdan keyin yangi type yoki yangi ma'no kerak bo'lsa `shadowing`

Rustning asosiy foydasi shunda: mutability joyi aniq ko'rinadi. Bu reasoningni osonlashtiradi, buglarni kamaytiradi va keyingi mavzular - [[ownership]], [[borrowing]], [[lifetimes]] - uchun mustahkam asos beradi.

## Evidence

- [[variables-and-mutability|variables and mutability]]: Rust variables default immutable; `mut` o'zgarish intentini explicit qiladi.
- [[immutability]]: immutable binding qayta assignmentdan himoyalaydi.
- [[shadowing]]: `let` bilan yangi binding yaratadi va type o'zgarishiga imkon beradi.
- [[constants]]: `const` immutable va explicit type annotation talab qiladi.
- [[e0384-cannot-assign-twice|E0384 cannot assign twice]]: immutable variablega qayta assignment qilinganda chiqadigan compiler error.
- [[3-1-variables-and-mutability-the-rust-programming-language]]: Rust Book 3.1 bo'limi variables, mutability, constants va shadowingni tushuntiradi.

## Follow-up

- `mut` va [[shadowing]] orasidagi amaliy tanlovni alohida misollar bilan ko'rish.
- Variablesning [[ownership]] va [[borrowing]] bilan bog'lanishini keyingi bosqichda o'rganish.

## Related Pages

- [[variables-and-mutability]]
- [[immutability]]
- [[shadowing]]
- [[constants]]
- [[type-inference]]
- [[type-annotations]]
- [[e0384-cannot-assign-twice]]
