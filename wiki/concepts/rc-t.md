---
title: "Rc<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, smart-pointers, rc, reference-counting, ownership]
source_count: 4
---

# Rc<T>

## Short Definition

`Rc<T>` — Reference Counted smart pointer. Bir qiymatning bir nechta o'quvchi egasi bo'lishi kerak bo'lganda ishlatiladi.

## Why It Matters

Oddiy ownership qoidalari bitta egani ko'zda tutadi. Graph, daraxt va boshqa tuzilmalarda bir node bir nechta qismga tegishli bo'lishi mumkin. `Rc<T>` shu muammoni hal qiladi.

**Cheklov:** faqat **single-threaded**. `Rc<T>` na `Send`, na `Sync`. Ko'p oqimli uchun `Arc<T>` ishlatiladi.

## Mental Model

TV xonasi: birinchi kishi kirsa TV yonadi, oxirgi kishi chiqsa o'chadi.

## Syntax and Examples

```rust
use std::rc::Rc;

let a = Rc::new(String::from("hello"));
let b = Rc::clone(&a);
let c = Rc::clone(&a);
```

## Weak<T>

`Rc<T>` + `RefCell<T>` kombinatsiyasida reference cycle paydo bo'lishi mumkin. Yechim: non-ownership munosabatlarida `Weak<T>` ishlatish.

## Common Mistakes

- `Rc<T>` mutable access beradi deb o'ylash.
- Multithreaded kodda `Rc<T>` ishlatishga urinish.
- `Rc::clone` chuqur nusxa qiladi deb o'ylash.

## Related Concepts

- [[reference-counting]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[weak-t|Weak<T>]]
- [[reference-cycle]]
- [[memory-leak]]
- [[drop|Drop trait]]
- [[smart-pointers]]
- [[ownership]]
- [[box-t|Box<T>]]
- [[threads]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]

## Sources

- [[15-4-rc-the-reference-counted-smart-pointer]]
- [[15-6-reference-cycles-can-leak-memory]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
