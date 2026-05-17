---
title: "Weak<T>"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, weak, reference-cycle]
source_count: 1
---

# Weak<T>

## Short Definition

`Weak<T>` — `Rc<T>` ning non-ownership zayif referensi. `weak_count`ni oshiradi, lekin cleanup ga ta'sir qilmaydi: strong_count 0 ga tushsa, `Weak<T>` referenslar bo'lsa ham qiymat tozalanadi.

## Why It Matters

`Rc<T>` bilan reference cycle qurishda strong_count hech qachon 0 ga tushmaydi — **memory leak**. `Weak<T>` "bu qiymatga murojaat qilaman, lekin uni ushlab turmayman" munosabatini ifodalaydi va cycle ni uzadi.

Qoida: **ownership** munosabati → `Rc<T>`; **non-ownership** (masalan, bola → ota) → `Weak<T>`.

## Mental Model

Kutubxona kartochkasi: kitob mavjud bo'lsa o'qib bo'ladi, yo'q bo'lsa xabar beradi. Kartochkaning o'zi kitobni kutubxonada ushlab turmaydi.

## Syntax and Examples

```rust
use std::rc::{Rc, Weak};

let strong = Rc::new(String::from("hello"));

// Weak<T> yaratish
let weak: Weak<String> = Rc::downgrade(&strong);

println!("strong = {}", Rc::strong_count(&strong)); // 1
println!("weak   = {}", Rc::weak_count(&strong));   // 1

// Weak<T> dan foydalanish — upgrade orqali
match weak.upgrade() {
    Some(val) => println!("hali tirik: {}", val),
    None      => println!("allaqachon tozalangan"),
}

drop(strong); // strong_count → 0 → cleanup

println!("{:?}", weak.upgrade()); // None
```

Tree da parent referens:

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

struct Node {
    value: i32,
    children: RefCell<Vec<Rc<Node>>>,  // strong — ownership
    parent:   RefCell<Weak<Node>>,     // weak — non-ownership
}

let leaf = Rc::new(Node {
    value: 3,
    parent: RefCell::new(Weak::new()),
    children: RefCell::new(vec![]),
});

let branch = Rc::new(Node {
    value: 5,
    parent: RefCell::new(Weak::new()),
    children: RefCell::new(vec![Rc::clone(&leaf)]),
});

// leaf ga parent qo'shish — cycle yo'q
*leaf.parent.borrow_mut() = Rc::downgrade(&branch);
```

## Common Mistakes

- `Weak<T>` ni `upgrade()` qilmasdan to'g'ridan-to'g'ri ishlatishga urinish — `Option<Rc<T>>` qaytaradi, avval tekshirilishi kerak.
- `Weak::new()` — mavjud `Rc`ga bog'liq emas; `upgrade()` har doim `None`.
- `weak_count` 0 bo'lishi shartligini o'ylash — yo'q, faqat `strong_count` 0 bo'lsa yetarli.

## Related Concepts

- [[rc-t|Rc<T>]] — strong counterpart
- [[reference-cycle]] — `Weak<T>` hal qiladigan muammo
- [[memory-leak]] — cycle sabab yuzaga keladigan oqibat
- [[drop|Drop trait]] — strong_count 0 da ishlaydi
- [[interior-mutability]] — `RefCell<Weak<T>>` kombinatsiyasi

## Sources

- [[15-6-reference-cycles-can-leak-memory]]
