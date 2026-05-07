---
title: "Stack va heap nima?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, memory]
source_count: 2
---

# Stack va heap nima?

## Answer

`Stack` va `heap` runtime memoryning ikki xil qismi.

`Stack` tez ishlaydi va odatda kichik, fixed-size qiymatlar uchun ishlatiladi. Rustda ko'pincha quyidagilar stackda saqlanadi:

- local variables bo'lib fixed-size bo'lgan qiymatlar
- function parameters
- return address va function call ga tegishli vaqtinchalik ma'lumotlar
- pointer, reference, length, capacity kabi metadata

```rust
let x = 5; // odatda stackda
let y = true; // odatda stackda
```

`heap` esa dynamic size yoki runtime allocation kerak bo'lgan data uchun ishlatiladi. Rustda heapda ko'pincha quyidagilar saqlanadi:

- `String` ning text bytes qismi
- `Vec<T>` ning elementlari
- `Box<T>` ichidagi qiymat
- `HashMap` kabi o'sadigan collectionlar
- size oldindan aniq bo'lmagan yoki katta data

```rust
let s = String::from("hello");
// s ning metadata'si stackda, matn bytes'lari heapda
```

Muhim nuqta: `String`ning o'zi butunlay heapda turmaydi. Uning stackda uchta metadata maydoni bo'ladi: pointer, length, capacity. Actual text esa heapda saqlanadi.

Qisqa farq:

- `stack`: tez, LIFO, fixed-size data
- `heap`: allocation talab qiladi, dinamik data

Rustda `ownership` aynan heap data qachon va kim tomonidan tozalanishini aniqlash uchun muhim. Shuning uchun `String` assignmentda move bo'ladi, `Copy` type'lar esa oddiy copy bo'ladi.

## Evidence

- [[stack-and-heap]]: stack va heapning asosiy modeli.
- [[4-1-what-is-ownership-the-rust-programming-language]]: `String` metadata stackda, text heapda ekanini tushuntiradi.
- [[string-type|String]]: growable text type va heap allocation.
- [[ownership]]: heap data cleanup ownership orqali boshqariladi.

## Follow-up

- `String` va `&str` ni stack/heap nuqtayi nazaridan solishtirish.
- `Vec<T>` qanday qilib heapdan foydalanishini ko'rish.
- `Box<T>` nega heap allocation qiladi.

## Related Pages

- [[stack-and-heap]]
- [[ownership]]
- [[string-type|String]]
- [[move-semantics]]
- [[copy-trait]]

