---
title: "15.4. Rc<T>, the Reference-Counted Smart Pointer"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, reference-counting, ownership]
source_count: 1
---

# 15.4. Rc<T>, the Reference-Counted Smart Pointer

## Source

`raw/books/the_rust_programming_language/15.4. Rc<T>, the Reference Counted Smart Pointer.md`
URL: https://doc.rust-lang.org/stable/book/ch15-04-rc.html

## Detailed Summary

Rustda ko'pincha ownership aniq bo'ladi — bitta egasi bo'ladi. Lekin ba'zi holatlarda, masalan, graph data strukturalarda, bir qiymatning bir nechta egasi bo'lishi kerak. `Rc<T>` (Reference Counting) bu imkonni beradi.

### Rc<T> nima va qachon ishlatiladi

`Rc<T>` qiymatga nechta referens borligini hisoblaydi. Referenslar soni nolga tushganda qiymat tozalanadi. Heap dagi ma'lumotni dasturning bir nechta qismida o'qish kerak bo'lganda ishlatiladi, lekin compile time da qaysi qism oxirida tugatishini bilish qiyin bo'lsa.

**Muhim cheklov:** `Rc<T>` faqat **single-threaded** holatda ishlaydi. Ko'p oqimli uchun Chapter 16 da boshqa yechim ko'rsatiladi.

### Box<T> bilan bo'lmagan narsa

```rust
// Bu xato — a move bo'ladi, c uchun ishlatib bo'lmaydi
let a = Cons(5, Box::new(Cons(10, Box::new(Nil))));
let b = Cons(3, Box::new(a));  // a moved into b
let c = Cons(4, Box::new(a));  // E0382: use of moved value
```

### Rc<T> bilan shared ownership

```rust
use std::rc::Rc;

enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));  // ref count: 2
    let c = Cons(4, Rc::clone(&a));  // ref count: 3
}
// c drop: count → 2; b drop: count → 1; a drop: count → 0 → cleanup
```

`Rc::clone(&a)` — bu deep copy **emas**, faqat reference countni oshiradi. Shuning uchun Rust konventsiyasi bo'yicha `a.clone()` o'rniga `Rc::clone(&a)` yoziladi: performance muammolarini izlaganda deep copy clonelarni qidirish oson bo'lsin.

### Reference count kuzatish

```rust
println!("count after creating a = {}", Rc::strong_count(&a)); // 1
let b = Cons(3, Rc::clone(&a));
println!("count after creating b = {}", Rc::strong_count(&a)); // 2
{
    let c = Cons(4, Rc::clone(&a));
    println!("count after creating c = {}", Rc::strong_count(&a)); // 3
}
println!("count after c goes out of scope = {}", Rc::strong_count(&a)); // 2
```

`Rc::strong_count` — joriy kuchli referenslar soni. `weak_count` ham mavjud (15.6 da ko'riladi).

`Drop` trait `Rc<T>` uchun ham ishlaydi: scope tugaganda count avtomatik kamayadi, shuning uchun `rc_decrement()` kabi funksiya chaqirmaydi.

### Faqat immutable access

`Rc<T>` faqat immutable (o'zgarmas) referenslar beradi. Agar mutable ham kerak bo'lsa, `RefCell<T>` bilan kombinatsiya qilinadi (15.5).

## Key Concepts

- [[rc-t|Rc<T>]] — reference counted smart pointer; multiple ownership
- [[reference-counting]] — qancha referens borligini hisoblash mexanizmi
- `Rc::clone` — reference count oshiradi (deep copy emas)
- `Rc::strong_count` — joriy kuchli referenslar soni
- Single-threaded cheklov

## Code Examples

Listing 15-17: `Box<T>` bilan shared ownership muvaffaqiyatsizligi → E0382
Listing 15-18: `Rc<T>` bilan shared ownership
Listing 15-19: Reference count o'zgarishlarini kuzatish

## Exercises or Practice Ideas

1. Ikki ro'yxat bitta uchinchi ro'yxatni `Rc<T>` orqali ulashuvchi cons list yozing.
2. `Rc::strong_count`ni turli nuqtalarda chop etib, count qanday o'zgarishini kuzating.
3. `Rc::clone(&a)` o'rniga `a.clone()` ishlatib ko'ring — natija bir xilmi?
4. `Rc<T>`ni multithreaded kontekstda ishlatishga urinib, compile xatosini ko'ring.

## Questions Raised

- `weak_count` nima uchun kerak? (15.6 — reference cycles oldini olish)
- Ko'p oqimli uchun qanday alternativ bor? (16-bob — `Arc<T>`)

## Links To Update

- [[rc-t]] — yangi concept sahifasi yaratish
- [[wiki/chapters/15-smart-pointers]] — 15.4 bo'limini qo'shish
- [[index]] — yangi source qo'shish
