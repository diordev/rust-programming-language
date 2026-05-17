---
title: "4.1. What Is Ownership? - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, ownership]
source_count: 1
source_path: "raw/books/the_rust_programming_language/4.1. What is Ownership?.md"
source_url: "https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html"
---

# 4.1. What Is Ownership? - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/4.1. What is Ownership?.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch04-01-what-is-ownership.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[ownership]]ni Rust memory management modeli sifatida tushuntiradi. Boshqa languages memoryni garbage collector yoki manual allocate/free orqali boshqarishi mumkin. Rust uchinchi yo'lni tanlaydi: memory ownership qoidalari orqali boshqariladi va compiler bu qoidalarni compile time'da tekshiradi. Ownership runtime performance'ni sekinlashtirmaydi, chunki qoidalar compile time'da enforce qilinadi.

[[stack-and-heap|Stack and heap]] ownershipni tushunish uchun mental model beradi. Stack LIFO tartibda ishlaydi, fixed-size data saqlaydi, push/pop tez. Heap runtime'da allocator orqali joy ajratadi, pointer qaytaradi, va access odatda sekinroq, chunki pointer follow qilish kerak. Ownershipning asosiy vazifasi heap data'ni kim ishlatayotganini kuzatish, duplicate heap data'ni kamaytirish, va unused heap data'ni tozalashdir.

Ownership rules:

- Har bir Rust value'ning owner'i bor.
- Bir vaqtning o'zida faqat bitta owner bo'lishi mumkin.
- Owner scope'dan chiqqanda value dropped bo'ladi.

Scope variable valid bo'lgan code range. Variable declarationdan keyin valid bo'ladi va current scope tugaganda invalid bo'ladi.

`String` ownershipni tushuntirish uchun ishlatiladi, chunki u heapda allocated, growable text saqlaydi. String literal compile time'da known va immutable, shuning uchun executable ichida hardcoded bo'ladi. `String::from("hello")` esa runtime'da heap allocation qiladi va mutable/growable textni saqlay oladi.

Heap allocation ikki muammoni talab qiladi: memoryni runtime'da so'rash va ishlatib bo'lgach allocatorga qaytarish. Rustda `String` owner'i scope'dan chiqqanda `drop` avtomatik chaqiriladi va heap memory freed bo'ladi. Bu C++dagi RAII patterniga o'xshaydi.

Assignment paytida `String` kabi heap-backed value move bo'ladi:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

Bu yerda stackdagi pointer/length/capacity metadata copy qilinadi, lekin heap data copy qilinmaydi. Double free bug oldini olish uchun Rust `s1`ni invalid deb hisoblaydi. `s1`ni keyin ishlatish `E0382 borrow of moved value` xatosini beradi.

Rust automatic deep copy qilmaydi. Agar heap data'ni haqiqatan copy qilish kerak bo'lsa, explicit `clone()` ishlatiladi:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

`clone()` ko'rinsa, bu operation potentially expensive bo'lishi mumkinligi aniq ko'rinadi.

Stack-only, fixed-size types uchun `Copy` trait ishlaydi. Integers, `bool`, floating-point types, `char`, va faqat `Copy` elementlardan iborat tuples assignmentda move qilinmaydi; ular trivially copied bo'ladi. `Drop` implement qilgan type `Copy` bo'la olmaydi.

Functionga value uzatish assignmentga o'xshaydi: `String` functionga berilsa ownership function parameteriga move bo'ladi; `i32` kabi `Copy` type functionga berilsa copy qilinadi va callerda hali ishlatiladi. Return value ham ownership transfer qiladi. Ownershipni functionga berib yana qaytarish tuple orqali mumkin, lekin bu tedious; section oxirida Rustda value'dan ownershipni transfer qilmasdan foydalanish uchun [[reference]]lar borligi aytiladi.

## Key Concepts

- [[ownership]]
- [[stack-and-heap|stack and heap]]
- [[scope]]
- [[string-type|String]]
- [[string-literal|string literal]]
- [[drop]]
- [[move-semantics|move semantics]]
- [[clone]]
- [[copy-trait|Copy trait]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
- [[reference]]

## Code Examples

Ownership rules in move form:

```rust
let s1 = String::from("hello");
let s2 = s1;

// println!("{s1}"); // E0382: borrow of moved value
```

Explicit clone:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {s1}, s2 = {s2}");
```

Function ownership transfer:

```rust
fn takes_ownership(some_string: String) {
    println!("{some_string}");
}

fn makes_copy(some_integer: i32) {
    println!("{some_integer}");
}
```

## Exercises or Practice Ideas

- `String`ni assignmentdan keyin eski variable orqali ishlatib `E0382`ni ko'rish.
- `clone()` qo'shib xatoni tuzatish va behaviorni tushuntirish.
- `i32` bilan assignmentdan keyin ikkala variable ham valid qolishini tekshirish.
- Functionga `String` va `i32` uzatib ownership/copy farqini kuzatish.
- Tuple bilan ownershipni qaytarish exampleini yozish.

## Questions Raised

- `String` move bo'lganda aynan qaysi stack fields copy qilinadi?
- Rust nima uchun automatic deep copy qilmaydi?
- `Drop` va `Copy` nima uchun bir type'da birga bo'la olmaydi?
- References ownership transfer ceremony'ni qanday kamaytiradi?

## Links To Update

- [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]]
- [[ownership]]
- [[move-vs-clone]]
- [[string-literal-vs-string]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
