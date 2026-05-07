---
title: "Lifetime Elision"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, lifetimes, compiler, elision]
source_count: 1
---

# Lifetime Elision

## Short Definition

Lifetime elision — Rust compileri lifetime annotatsiyalarni avtomatik infer qila oladigan deterministic qoidalar to'plami. Bu qoidalar tufayli ko'p hollarda explicit `'a` yozmasdan ham compile bo'ladi.

## Why It Matters

Rust dastlab har bir reference uchun explicit lifetime talab qilgan (pre-1.0). Vaqt o'tishi bilan Rust jamoasi programmerllar bir xil annotatsiyalarni qayta-qayta yozishini kuzatdi va bu patternlarni compiler ichiga kodladi. Natija: kod qisqaroq va o'qilishi osonroq bo'ldi — lekin qoidalar noaniq holatlarda hali ham explicit annotatsiya talab qiladi.

## Mental Model

Compiler annotatsiyasiz funksiya signaturasiga duch kelganda, uchta qoidani ketma-ket qo'llaydi. Agar qoidalar barcha lifetimesni hal qilsa — annotatsiya kerak emas. Agar biror lifetime aniqlanmay qolsa — compiler error beradi.

Elision qoidalar programmerllar uchun qoida emas — compiler uchun infer algoritmi.

## Uchta Qoida

**Input lifetimes** (parametrlar) uchun:

**Qoida 1:** Har bir reference parameter o'z alohida lifetime parameterini oladi.

```rust
// Yozilgan
fn foo(x: &i32, y: &i32) -> &i32 { ... }

// Compiler ko'radi
fn foo<'a, 'b>(x: &'a i32, y: &'b i32) -> &i32 { ... }
```

**Output lifetimes** (return type) uchun:

**Qoida 2:** Agar faqat bitta input lifetime parameter bo'lsa, u barcha output parametrlarga beriladi.

```rust
// Yozilgan
fn foo(x: &i32) -> &i32 { ... }

// Compiler ko'radi
fn foo<'a>(x: &'a i32) -> &'a i32 { ... }
```

**Qoida 3:** Agar bir nechta input lifetime bo'lsa, lekin ulardan biri `&self` yoki `&mut self` bo'lsa (method), `self`ning lifetime'i barcha output parametrlariga beriladi.

```rust
// Yozilgan (method ichida)
fn announce_and_return_part(&self, announcement: &str) -> &str { ... }

// Qoida 1: &self → 'a, announcement → 'b
// Qoida 3: return → 'a (self lifetime)
fn announce_and_return_part<'a, 'b>(&'a self, announcement: &'b str) -> &'a str { ... }
```

## Qoidalar Qachon Yetmaydi

`longest` funksiyasida:

```rust
fn longest(x: &str, y: &str) -> &str { ... }
```

Qoida 1 → ikkita input lifetime (`'a`, `'b`). Qoida 2 — bir nechta input, qo'llanmaydi. Qoida 3 — method emas, qo'llanmaydi. Return lifetime aniqlanmadi → `error[E0106]: missing lifetime specifier`.

Yechim — explicit annotatsiya:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str { ... }
```

## Syntax and Examples

```rust
// Elision ishlaydi — bitta input, bir xil output lifetime
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' { return &s[0..i]; }
    }
    &s[..]
}

// Elision ishlamaydi — qaysi input qaytarilishini compiler bilmaydi
fn longest(x: &str, y: &str) -> &str { ... } // E0106
```

## Common Mistakes

- Elision ishlaydi deb hamma funksiyada annotatsiya yozmaslik; ikkita reference parameter va return reference bo'lsa explicit annotatsiya kerak.
- Elision qoidalarini bilmasdan `'a` qo'shib, nima uchun ishlayotganini tushunmaslik.

## Related Concepts

- [[lifetimes]]
- [[e0106-missing-lifetime-specifier]]
- [[static-lifetime]]
- [[borrowing]]
- [[reference]]

## Sources

- [[10-3-validating-references-with-lifetimes-the-rust-programming-language]]
