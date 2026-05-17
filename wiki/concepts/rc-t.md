---
title: "Rc<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, reference-counting, ownership]
source_count: 2
---

# Rc<T>

## Short Definition

`Rc<T>` — Reference Counted smart pointer. Bir qiymatning bir nechta o'quvchi egasi bo'lishi kerak bo'lganda, compile time da ownership yakunini aniqlab bo'lmaydigan holatlarda ishlatiladi.

## Why It Matters

Oddiy ownership qoidalari bitta egani ko'zda tutadi. Graph, daraxt va boshqa tuzilmalarda bir node bir nechta qismga tegishli bo'lishi mumkin. `Rc<T>` shu muammoni hal qiladi — reference count nolga tushgandagina qiymat tozalanadi.

**Cheklov:** faqat **single-threaded**. Ko'p oqimli uchun `Arc<T>` (Atomically Reference Counted) ishlatiladi — `Rc<T>` bilan bir xil API, lekin reference counting atomik operatsiyalar orqali bajariladi, shuning uchun threadlar orasida xavfsiz. `Arc<T>` `Send + Sync` traitlarini implement qiladi.

## Mental Model

TV xonasi: birinchi kishi kirsa TV yonadi, oxirgi kishi chiqsa o'chadi. Hech kim kirmay turib o'chirish — qolganlar uchun muammo. `Rc<T>` shunday ishlaydi.

## Syntax and Examples

```rust
use std::rc::Rc;

let a = Rc::new(String::from("hello"));
let b = Rc::clone(&a);   // reference count: 2, deep copy emas
let c = Rc::clone(&a);   // reference count: 3

println!("count = {}", Rc::strong_count(&a)); // 3
// b, c scope dan chiqsa count kamayadi; 0 da cleanup
```

Shared cons list:

```rust
use std::rc::Rc;
enum List { Cons(i32, Rc<List>), Nil }
use List::{Cons, Nil};

let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
let b = Cons(3, Rc::clone(&a));
let c = Cons(4, Rc::clone(&a));
// a, b, c hammasi oxirgi ikkita elementni ulashadi
```

`Rc::clone(&a)` — Rust konventsiyasi. `a.clone()` ham ishlaydi, lekin visual farq: `Rc::clone` = cheap (count++), boshqa `clone` = expensive (deep copy). Performance debugging da qulaylik uchun.

## Weak<T> — reference cycle dan himoya

`Rc<T>` + `RefCell<T>` kombinatsiyasida reference cycle paydo bo'lishi mumkin — count hech qachon 0 ga tushmaydi va qiymatlar tozalanmaydi (**memory leak**). Yechim: non-ownership munosabatlarida `Weak<T>` ishlatish.

```rust
use std::rc::{Rc, Weak};

let rc = Rc::new(5);
let weak: Weak<i32> = Rc::downgrade(&rc); // weak_count++, strong_count o'zgarmaydi

// Weak dan foydalanish
if let Some(val) = weak.upgrade() {
    println!("{}", val);
}
```

Qoida: ota → bola `Rc<T>` (owns); bola → ota `Weak<T>` (non-owns).

## Common Mistakes

- `Rc<T>` mutable access beradi deb o'ylash — faqat **immutable**. Mutable kerak bo'lsa `Rc<RefCell<T>>` ishlatiladi.
- Multithreaded kodda `Rc<T>` ishlatishga urinish — compile xatosi; o'rniga `Arc<T>`.
- `Rc::clone` chuqur nusxa qiladi deb o'ylash — yo'q, faqat count oshiradi.
- `Rc<T>` + `RefCell<T>` bilan cycle yaratib qo'yish — kompilyator ogohlantirmaydi.

## Related Concepts

- [[reference-counting]] — mexanizm
- [[refcell-t|RefCell<T>]] — `Rc<RefCell<T>>` kombinatsiyasi uchun
- [[weak-t|Weak<T>]] — non-ownership referens; cycle ni oldini oladi
- [[reference-cycle]] — `Rc<T>` + `RefCell<T>` bilan yuzaga kelishi mumkin
- [[memory-leak]] — cycle ning oqibati
- [[drop|Drop trait]] — scope oxirida count avtomatik kamayadi
- [[smart-pointers]]
- [[ownership]]
- [[box-t|Box<T>]] — bitta owner uchun alternativ
- [[threads]] — ko'p oqimli uchun `Arc<T>` kerak
- [[send-trait|Send trait]] — `Rc<T>` Send emas, `Arc<T>` Send

## Sources

- [[15-4-rc-the-reference-counted-smart-pointer]]
- [[15-6-reference-cycles-can-leak-memory]]
