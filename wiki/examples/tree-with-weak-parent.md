---
title: "Weak<T> bilan parent referensli tree"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, rc, weak, tree]
source_count: 1
---

# Weak<T> bilan parent referensli tree

## Tavsif

Bola node ota nodega `Weak<T>` orqali ishora qiladi — otalikni da'vo qilmaydi. Ota esa bolani `Rc<T>` orqali owns qiladi. Cycle yo'q, memory leak yo'q.

## Kod

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

#[derive(Debug)]
struct Node {
    value:    i32,
    parent:   RefCell<Weak<Node>>,          // non-ownership
    children: RefCell<Vec<Rc<Node>>>,       // ownership
}

fn main() {
    let leaf = Rc::new(Node {
        value:    3,
        parent:   RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });

    let branch = Rc::new(Node {
        value:    5,
        parent:   RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });

    // leaf ga parent qo'shish
    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

    println!(
        "branch: strong={}, weak={}",
        Rc::strong_count(&branch),  // 1
        Rc::weak_count(&branch),    // 1 (leaf.parent dan)
    );
    println!(
        "leaf: strong={}, weak={}",
        Rc::strong_count(&leaf),    // 2 (leaf + branch.children)
        Rc::weak_count(&leaf),      // 0
    );

    // leaf.parent mavjudligini tekshirish
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    // Some(Node { value: 5, ... })
}
```

## Count o'zgarishi (branch scope bilan)

```rust
{
    let branch = /* ... */;
    // branch strong=1, weak=1
    // leaf   strong=2, weak=0
} // branch drop → strong_count 0 → Node tozalanadi

println!("{:?}", leaf.parent.borrow().upgrade()); // None
// leaf strong=1, weak=0
```

## Izoh

- `children: RefCell<Vec<Rc<Node>>>` — ota bolani owns qiladi.
- `parent: RefCell<Weak<Node>>` — bola otaga ishora qiladi, owns qilmaydi.
- `branch` strong_count 0 ga tushganda drop bo'ladi — `weak_count` 1 bo'lsa ham.
- Keyin `leaf.parent.upgrade()` → `None`.

## Related Pages

- [[weak-t]]
- [[rc-t]]
- [[reference-cycle]]
- [[15-6-reference-cycles-can-leak-memory]]
