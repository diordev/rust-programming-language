---
title: "15.6. Reference Cycles Can Leak Memory"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, weak, memory-leak, reference-cycle]
source_count: 1
---

# 15.6. Reference Cycles Can Leak Memory

## Source

`raw/books/the_rust_programming_language/15.6. Reference Cycles Can Leak Memory.md`
URL: https://doc.rust-lang.org/stable/book/ch15-06-reference-cycles.html

## Detailed Summary

Rust memory safety ni kafolatlaydi, lekin memory leak ni to'liq taqiqlamaydi. `Rc<T>` va `RefCell<T>` kombinatsiyasi reference cycle yaratishi mumkin — bu memory safe, ammo memory leak hisoblanadi.

### Reference cycle qanday yuzaga keladi

```rust
enum List {
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}
```

`a` → `b` → `a` sikli:

```
a initial rc count = 1
a rc count after b creation = 2
b rc count after changing a = 2
a rc count after changing a = 2
```

`main` tugaganda: `b` drop → b count: 2→1 (0 emas!); `a` drop → a count: 2→1 (0 emas!). Ikkalasi tozalanmaydi — **memory leak**.

Stack overflow misoli: `println!("a next item = {:?}", a.tail())` — a → b → a → ... → stack overflow.

### Weak<T> — yechim

`Rc<T>` strong reference beradi: cleanup uchun strong_count 0 bo'lishi kerak.
`Weak<T>` weak reference beradi: weak_count cleanup ga ta'sir qilmaydi.

```rust
use std::rc::{Rc, Weak};

let rc_val = Rc::new(5);
let weak_val: Weak<i32> = Rc::downgrade(&rc_val); // weak_count++, strong_count o'zgarmaydi

// Weak<T> ni ishlatish uchun avval upgrade
match weak_val.upgrade() {
    Some(val) => println!("{}", val), // hali tirik
    None      => println!("dropped"), // allaqachon tozalangan
}
```

`upgrade()` → `Option<Rc<T>>`: qiymat drop bo'lgan bo'lsa `None`, bo'lmasa `Some`.

### Tree misoli: ownership va non-ownership

Ota bolani **owns** qilishi kerak; bola otani **owns** qilmasligi kerak. Shuning uchun:

```rust
struct Node {
    value: i32,
    children: RefCell<Vec<Rc<Node>>>,   // strong — ota bolani owns qiladi
    parent:   RefCell<Weak<Node>>,      // weak — bola otaga owns qilmaydi
}
```

`Rc::downgrade(&branch)` orqali `leaf.parent` ga `Weak<Node>` o'rnatiladi:

```rust
*leaf.parent.borrow_mut() = Rc::downgrade(&branch);
```

`branch` scope dan chiqqanda: strong_count 0 → Node drop qilinadi. `leaf.parent.upgrade()` endi `None` qaytaradi. Cycle yo'q, memory leak yo'q.

### strong_count va weak_count o'zgarishi (Listing 15-29)

| Holat | leaf strong | leaf weak | branch strong | branch weak |
|---|---|---|---|---|
| leaf yaratilganda | 1 | 0 | — | — |
| branch yaratilganda | 2 | 0 | 1 | 1 |
| branch scope dan chiqqanda | 1 | 0 | (dropped) | — |

`branch` strong_count 0 ga tushadi → drop. `weak_count` 1 bo'lgan bo'lsa ham, cleanup bo'ladi.

### Chapter 15 yakuniy xulosa

| Type | Ownership | Size | Borrow checking | Mutable |
|---|---|---|---|---|
| `Box<T>` | bitta | known at compile | compile time | ha |
| `Rc<T>` | ko'p | — | compile time | yo'q |
| `RefCell<T>` | bitta | — | runtime | ha |
| `Weak<T>` | yo'q | — | — | — |

`Deref` va `Drop` — smart pointer imkoniyatlarini ta'minlovchi asosiy traitlar.

## Key Concepts

- [[memory-leak]] — Rust da mumkin; memory safe, lekin resurs sarf qiladi
- [[reference-cycle]] — `Rc<T>` + `RefCell<T>` orqali yuzaga kelishi mumkin
- [[weak-t|Weak<T>]] — non-ownership referens; cycle ni oldini oladi
- `Rc::downgrade` — `Weak<T>` yaratadi (weak_count++)
- `Weak::upgrade` → `Option<Rc<T>>` — xavfsiz dereferencing
- Ownership dizayni: ota → bola `Rc`, bola → ota `Weak`

## Code Examples

Listing 15-25: `RefCell<Rc<List>>` bilan cons list ta'rifi
Listing 15-26: a → b → a reference cycle va memory leak
Listing 15-27: Farzand nodesi bilgan tree (`parent` yo'q)
Listing 15-28: `Weak<Node>` bilan parent referens
Listing 15-29: strong_count va weak_count o'zgarishini kuzatish

## Exercises or Practice Ideas

1. Listing 15-26 kodni ishlatib, `println!("a next item = {:?}", a.tail())` ni uncommit qiling — stack overflow kuzating.
2. Tree nodeini `Weak<Node>` parent bilan yozib, `branch` scope dan chiqqanda `leaf.parent.upgrade()` `None` qaytarishini tasdiqlang.
3. `Rc::strong_count` va `Rc::weak_count` ni turli nuqtalarda chop etib, Listing 15-29 jadvalini takrorlang.
4. Ota-bola munosabatida `Rc` o'rniga `Weak` ishlatmasangiz nima bo'lishini tushuntiring.

## Questions Raised

- `Arc<T>` va `Arc::downgrade` — multithreaded ekvivalenti (16-bob)
- `Cell<T>` — `RefCell<T>` ning Copy-only varianti qanday ishlaydi?
- Rustonomicon: custom smart pointer yozish

## Links To Update

- [[rc-t]] — Weak<T> va cycle xavfi qo'shish
- [[wiki/chapters/15-smart-pointers]] — 15.6 bo'limini qo'shish
- [[index]] — yangi source
