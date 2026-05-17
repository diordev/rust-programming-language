---
title: "memory leak"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, memory, smart-pointers, reference-cycle]
source_count: 1
---

# memory leak

## Short Definition

Ajratilgan xotiraning hech qachon qaytarilmasligi — dastur ishlayveradi, lekin xotira bo'shab ketadi. Rust memory leak ni **kafolat qilmaydi**, faqat memory unsafety ni kafolatlaydi.

## Why It Matters

Rust ning asosiy va'dasi: use-after-free, double-free, dangling pointer kabi memory **unsafety** holatlarini bartaraf etish. Ammo memory **leak** — resurssizlik, logiqa xatosi — boshqa kategoriya va Rust bu bilan kafolatlanmagan.

## Mental Model

Xona ijaraga olinadi, lekin hech qachon bo'shatilmaydi. Uy egasi ham, ko'cha ham bu xonani qaytarib ololmaydi — u "tirik" ko'rinishda, lekin foydasiz band.

## Qanday yuzaga keladi Rust da

Asosan `Rc<T>` + `RefCell<T>` reference cycle orqali:

```rust
// a va b bir-biriga ishora qiladi
// strong_count hech qachon 0 ga tushmaydi
// ikkalasi tozalanmaydi
```

Boshqa holatlar: `Box::leak`, `std::mem::forget`, `unsafe` blok ichida noto'g'ri pointer boshqaruvi.

## Oldini olish

- Reference cycle larda `Weak<T>` ishlatish.
- Data struktura dizaynida ownership munosabatlarini aniq belgilash.
- Avtomatik testlar va code review.

## Related Concepts

- [[reference-cycle]] — Rust dagi asosiy sabab
- [[weak-t|Weak<T>]] — cycle ni uzish vositasi
- [[rc-t|Rc<T>]] — cycle ning qatnashchisi
- [[drop|Drop trait]] — to'g'ri ishlaganda memory leak yo'q
- [[ownership]] — Rust ning asosiy kafolati

## Sources

- [[15-6-reference-cycles-can-leak-memory]]
