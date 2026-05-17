---
title: "reference cycle"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, memory-leak, reference-cycle]
source_count: 1
---

# reference cycle

## Short Definition

Bir-biriga `Rc<T>` orqali ishora qiluvchi qiymatlar zanjiri — reference countlar hech qachon 0 ga tushmaydi va qiymatlar hech qachon tozalanmaydi.

## Why It Matters

Rust memory safety ni kafolatlaydi, lekin memory leak ni **kafolat qilmaydi**. Reference cycle — memory safe, lekin resurssiz qoldiradigan mantiqiy xato. Kompilyator uni aniqlamaydi.

## Mental Model

A → B → A: A ni tozalamoqchi bo'lsak, B uni ushlab turibdi. B ni tozalamoqchi bo'lsak, A uni ushlab turibdi. Hech qachon birinchi bo'lib tozalanmaydi.

## Syntax and Examples

Cycle yaratish:

```rust
use std::cell::RefCell;
use std::rc::Rc;

#[derive(Debug)]
enum List {
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}

fn main() {
    let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil))));
    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));

    // a → b sikli yaratish
    if let Cons(_, tail) = a.as_ref() {
        *tail.borrow_mut() = Rc::clone(&b);
    }

    // main tugaganda: a count = 1, b count = 1 — hech qachon 0 emas
    // ikkala qiymat tozalanmaydi → memory leak
}
```

## Oldini olish

1. **`Weak<T>` ishlatish** — non-ownership munosabatlarida `Rc<T>` o'rniga `Weak<T>`.
2. **Data strukturani qayta ko'rib chiqish** — ba'zi munosabatlar ownership bo'lmasligi kerak.

Qoida: ota → bola `Rc<T>` (owns); bola → ota `Weak<T>` (non-owns).

## Common Mistakes

- `Rc<T>` + `RefCell<T>` kombinatsiyasida cycle yaratib qo'yish — kompilyator ogohlantirmaydi.
- `println!` bilan cycle ni chop etishga urinish — stack overflow.

## Related Concepts

- [[rc-t|Rc<T>]] — cycle ning asosi
- [[weak-t|Weak<T>]] — yechim
- [[memory-leak]] — oqibat
- [[refcell-t|RefCell<T>]] — cycle yaratish imkonini beruvchi interior mutability
- [[interior-mutability]]

## Sources

- [[15-6-reference-cycles-can-leak-memory]]
