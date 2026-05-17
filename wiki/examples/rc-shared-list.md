---
title: "Rc<T> bilan shared cons list"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, ownership]
source_count: 1
---

# Rc<T> bilan shared cons list

## Tavsif

Ikki ro'yxat (`b` va `c`) bitta uchinchi ro'yxatni (`a`) `Rc<T>` orqali ulashadi. `Box<T>` bilan bu imkonsiz — `a` move bo'lib ketardi.

## Kod

```rust
use std::rc::Rc;

enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a)); // 1

    let b = Cons(3, Rc::clone(&a));
    println!("count after creating b = {}", Rc::strong_count(&a)); // 2

    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a)); // 3
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a)); // 2
}
```

## Chiqish

```
count after creating a = 1
count after creating b = 2
count after creating c = 3
count after c goes out of scope = 2
```

## Izoh

- `Rc::clone(&a)` deep copy emas — faqat reference count oshiradi.
- `c` scope dan chiqqanda `Drop` avtomatik count ni kamaytiradi.
- `b` va `a` scope oxirida yana kamayib, count 0 ga tushadi va cleanup bo'ladi.

## Related Pages

- [[rc-t]]
- [[15-4-rc-the-reference-counted-smart-pointer]]
- [[box-cons-list]]
