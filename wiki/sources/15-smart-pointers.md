---
title: "15. Smart Pointers"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, ownership, memory]
source_count: 1
---

# 15. Smart Pointers

## Source

- Manba: The Rust Programming Language, 15-bob
- URL: https://doc.rust-lang.org/stable/book/ch15-00-smart-pointers.html
- Raw fayl: `raw/books/the_rust_programming_language/15. Smart Pointers.md`

## Detailed Summary

15-bob [[smart-pointers|smart pointers]] mavzusini ochadi. Oddiy pointer memory address saqlaydi; Rustdagi eng ko'p uchraydigan pointer turi [[reference|reference]] bo'lib, `&` orqali qiymatni borrow qiladi va qo'shimcha behavior bermaydi.

Smart pointer esa pointer kabi ishlaydigan data structure: u address bilan birga qo'shimcha metadata yoki behavior olib yurishi mumkin. Rustda muhim farq shuki, reference odatda data'ni faqat borrow qiladi, ko'p smart pointerlar esa ko'rsatayotgan data'ga [[ownership]] ham qiladi.

Smart pointerlar odatda [[structs|struct]] sifatida implement qilinadi va ikki trait bilan bogliq:

- [[deref-trait|Deref trait]] smart pointerni reference kabi ishlatishga yordam beradi.
- [[drop|Drop trait]] value scope'dan chiqqanda cleanup code'ni custom qilishga imkon beradi.

Bob standard librarydagi asosiy smart pointerlarni ko'rib chiqishga tayyorlaydi:

- [[box-t|Box<T>]] - value'ni heapda saqlaydi.
- `Rc<T>` - reference counting orqali multiple ownership beradi.
- `RefCell<T>` orqali olinadigan `Ref<T>` va `RefMut<T>` - borrowing qoidalarini compile time emas, runtime'da enforce qiladi.

Bob keyingi bo'limlarda interior mutability va reference cycles memory leakga olib kelishi mumkinligini ham ko'rib chiqishini aytadi.

## Key Concepts

- [[smart-pointers]]
- [[reference]]
- [[ownership]]
- [[box-t|Box<T>]]
- [[deref-trait|Deref trait]]
- [[drop|Drop trait]]
- [[memory-safety|memory safety]]

## Code Examples

15.0 faqat overview beradi; konkret code misollar 15.1 va 15.2 source sahifalarida.

## Exercises or Practice Ideas

- Oddiy reference va smart pointer farqini bitta jadvalda yozing: ownership, borrowing, cleanup, extra behavior.
- `String`, `Vec<T>`, `Box<T>` uchun "stackda nima turadi, heapda nima turadi?" degan mental model chizing.

## Questions Raised

- Qachon `Box<T>` kerak, qachon oddiy reference yetarli?
- `Deref` va `Drop` traitlari smart pointer behaviorini qanday ochadi?
- `Rc<T>`, `RefCell<T>`, va interior mutability qachon kerak bo'ladi?

## Links To Update

- [[wiki/chapters/15-smart-pointers]]
- [[smart-pointers]]
- [[box-t]]
- [[deref-trait]]
- [[drop]]
