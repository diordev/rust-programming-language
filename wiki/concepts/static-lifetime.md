---
title: "Static Lifetime"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-16
tags: [rust, lifetimes, static]
source_count: 3
---

# Static Lifetime

## Short Definition

`'static` — Rust dasturining butun ishlash davomida valid bo'ladigan reference lifetime. Barcha string literallar `'static` lifetimega ega.

## Why It Matters

`'static` Rustdagi eng uzun lifetime. String literallar dastur binary'siga to'g'ridan-to'g'ri kiritiladi va shu sababli dastur ishlab turgan vaqt davomida har doim mavjud bo'ladi.

## Mental Model

`'static` reference butun dastur davomida yashaydigan ma'lumotga ishora qiladi. String literallar uchun bu tabiiy — ular binary ichida saqlanadi. Boshqa typelar uchun `'static` odatda global yoki `Box::leak` bilan static memory'ga joylashtirilgan ma'lumot degani.

**Ogohlantirish:** Compiler `'static` taklif qilganda bu ko'pincha muammoning belgisi. Haqiqiy muammo — dangling reference yoki lifetime mismatch. Avval muammoning ildizini topib hal qiling; `'static` bilan "yopib ketish" kod dizaynini murakkablashtiradi.

## Syntax and Examples

```rust
// String literalning tipi &'static str
let s: &'static str = "I have a static lifetime.";

// Explicit annotatsiya yozmasdan ham ishlaydi — inferred
let greeting = "Hello, world!"; // &'static str
```

String literallar dastur binary'sida saqlanadi va har doim mavjud bo'ladi, shuning uchun ularga reference har doim valid.

Bu beginner backend source'da ham to'g'ridan-to'g'ri beriladi:

```rust
const TEXT: &'static str = "some string";
```

```rust
pub fn execute<F>(&self, f: F)
where
    F: FnOnce() + Send + 'static,
{
    // worker thread qachon bajarishini bilmaymiz
}
```

Bu joydagi `'static` "closure abadiy yashaydi" degani emas. Ma'nosi: closure ichidagi borrow'lar dasturdan qisqaroq umrli reference'larga bog'lanmagan bo'lishi kerak. Owned `String`, `Vec<T>`, `TcpStream` kabi qiymatlar `move` bilan closure ichiga o'tsa bu bound'ni qondira oladi.

## Common Mistakes

- Compiler `'static` taklif qilganda darhol qo'llash — ko'pincha bu to'g'ri yechim emas.
- `'static` bilan hamma lifetime muammolarini "hal qilishga" urinish — bu asl muammoni yashiradi.
- `'static` faqat string literallarga tegishli deb o'ylash — aslida istalgan turdagi ma'lumot `'static` bo'lishi mumkin (global static, `Box::leak` orqali).
- `thread::spawn` yoki thread pool'dagi `'static`ni "memory leak qil" degan maslahat deb tushunish — ko'pincha owned data'ni move qilish kifoya.

## Related Concepts

- [[lifetimes]]
- [[lifetime-elision]]
- [[string-slice]]
- [[reference]]
- [[thread-pool]]
- [[send-trait|Send trait]]

## Sources

- [[10-3-validating-references-with-lifetimes]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-lifetimes]]
