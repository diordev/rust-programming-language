---
title: "Recursive Types"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, types, recursion, memory]
source_count: 2
---

# Recursive Types

## Short Definition

Recursive type o'z definitioni ichida o'ziga qayta ishora qiladigan type. Rust bunday type uchun compile time'da finite size topa olishi kerak.

## Why It Matters

Linked list, tree, expression AST kabi data structures recursive shaklga ega. Rustda bunday typelarni to'g'ridan-to'g'ri value ichida value qilib yozib bo'lmaydi; odatda [[box-t|Box<T>]], `Rc<T>`, yoki reference kabi indirection kerak. Agar recursive tuzilma shared ownership ham talab qilsa, `Box<T>` o'rniga ko'pincha `Rc<T>` yoki `Rc<RefCell<T>>` ishlatiladi.

## Mental Model

Bu infinite size:

```rust
enum List {
    Cons(i32, List),
    Nil,
}
```

`Cons` ichida yana `List`, uning ichida yana `List` bo'lishi mumkin. Compiler stack layoutni hisoblay olmaydi.

Bu finite size:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

`Box<List>` pointer size'iga ega; keyingi `List` heapda joylashadi.

## Syntax and Examples

Cons list:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}
```

## Common Mistakes

- Recursive enumni to'g'ridan-to'g'ri o'zini saqlaydigan variant bilan yozish.
- `Vec<T>` ko'proq mos bo'lgan oddiy list vazifasida cons list ishlatish.
- Indirectionning maqsadini faqat "heapga qo'yish" deb tushunish; asosiy sabab compilerga known size berish.

## Related Concepts

- [[box-t|Box<T>]]
- [[smart-pointers]]
- [[enums]]
- [[stack-and-heap|stack and heap]]
- [[e0072-recursive-type-has-infinite-size|E0072 recursive type has infinite size]]

## Sources

- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
