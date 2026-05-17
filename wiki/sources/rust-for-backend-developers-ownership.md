---
title: "Владение - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, ownership]
source_count: 1
---

# Владение - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/21. Владение.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/ownership.html

## Detailed Summary

Bu source ownershipni backend-developer audience uchun Rustning markaziy differentiatori sifatida qo'yadi: garbage collector yo'q, qo'lda free ham yo'q, lekin compiler ownership qoidalari bilan resource cleanup'ni boshqaradi.

Core rule sodda beriladi: primitive bo'lmagan owned value bir vaqtning o'zida bitta ownerga ega bo'ladi. Shu sabab `let s2 = s1;` kabi assignment copy emas, move bo'ladi; eski binding invalid bo'ladi. Source `String` misoli bilan bunu ko'rsatadi va `borrow of moved value` turidagi compile-time xatoni mental modelga bog'laydi.

Cleanup qismi scope bilan ulangan: owner scope'dan chiqqanda compiler destructor/drop chaqirish nuqtasini aniq biladi. Shu yerda source borrow checker'ni ham ownership va referential safety qoidalarini kuzatuvchi mexanizm sifatida nomlaydi.

Source "ownership memory leaklarni kafolatli yo'q qiladi" degan da'voga yaqin yozadi. Wiki bu joyni ehtiyotkor qabul qiladi: ownership deterministic cleanup va use-after-free'dan himoya beradi, lekin safe Rust'da leak umuman imkonsiz degan keskin xulosa to'g'ri emas.

Keyin ownership transfer bo'ladigan barcha asosiy nuqtalar sanaladi: assignment, function argument, return value, closure capture. `greet(name: String) -> String` misoli function call ownershipni qanday ko'chirishini toza ko'rsatadi.

Tuple bilan value'ni qaytarib olish misoli ataylab noqulay, lekin foydali: borrowing nega kerakligini ko'rsatadi. `len_of_string(&s)` bilan reference orqali ownershipni saqlab qolish backend API design uchun ham muhim signal beradi. Source o'zi ham real API'da `&String` o'rniga `&str` yaxshiroq ekanini ochiq aytadi.

Referential safety bo'limi kuchli: vector elementiga immutable reference ushlab turib, butun vector'ga mutable reference olib `push` qilinsa reallocation dangling reference xavfiga olib keladi. Shu orqali Rustning "yo bitta `&mut T`, yo ko'p `&T`" qoidasi asoslantiriladi.

Oxirida primitive typelar move qilinmasligi, copy bo'lishi ko'rsatiladi. `for n in arr` misoli bilan ownership iteration header'da ham ishlashini ko'rsatadi: owned `String` array elementlari loop variable'ga move bo'ladi; `for n in &arr` esa borrowing variant.

## Key Concepts

- [[ownership]]
- [[move-semantics|move semantics]]
- [[drop]]
- [[borrow-checker]]
- [[borrowing]]
- [[reference]]
- [[dangling-reference|dangling reference]]
- [[data-race|data race]]
- [[copy-trait|Copy trait]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]

## Code Examples

```rust
let s1 = String::from("some string");
let s2 = s1;
// s1 invalid, s2 owner
```

```rust
fn greet(name: String) -> String {
    format!("Hello {}!!!", name)
}
```

```rust
fn len_of_string(s: &String) -> usize {
    s.len()
}
```

```rust
let mut vector: Vec<i32> = Vec::with_capacity(3);
vector.push(1);
vector.push(2);
vector.push(3);
let reference: &i32 = &vector[1];
let vec_ref = &mut vector;
vec_ref.push(4);
```

```rust
for n in &arr {
    println!("{n}");
}
```

## Exercises or Practice Ideas

- Bitta functionni avval `String`, keyin `&str` parameter bilan yozib, ownership farqini kuzating.
- `for item in arr` va `for item in &arr` farqini `String` array bilan tekshiring.
- Vector reallocation misolini o'qib, nega `push` immutable borrow turganda xavfli ekanini diagrammasiz tushuntiring.

## Questions Raised

- Qachon `clone()` to'g'ri, qachon borrowing to'g'ri?
- Borrow checker'ni alohida mexanizm deb o'qitish yaxshimi yoki ownershipning amaliy qatlami debmi?
- Ownership deterministic cleanup beradi, lekin leak masalasi qayerda yana qoladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-ownership]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[borrow-checker]]
