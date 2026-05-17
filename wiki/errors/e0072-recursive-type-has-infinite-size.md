---
title: "E0072 recursive type has infinite size"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, compiler-error, recursive-types, box]
source_count: 1
---

# E0072 recursive type has infinite size

## Symptom

Compiler recursive type direct self-reference sabab infinite sizega ega deb xabar beradi.

## Cause

Type definition o'zini to'g'ridan-to'g'ri saqlasa, Rust compile time'da bu type uchun qancha stack space kerakligini hisoblay olmaydi.

Failing:

```rust
enum List {
    Cons(i32, List),
    Nil,
}
```

`Cons` ichida yana `List`, uning ichida yana `List` bo'lishi mumkin. Bu infinite nestingga olib keladi.

## Fix Pattern

Indirection qo'shing: odatda [[box-t|Box<T>]], ba'zan `Rc<T>` yoki reference.

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

`Box<List>` fixed-size pointer bo'lgani uchun compiler `List` layoutini hisoblay oladi.

## Minimal Example

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Nil))));
}
```

## Related Concepts

- [[recursive-types]]
- [[box-t|Box<T>]]
- [[smart-pointers]]
- [[stack-and-heap|stack and heap]]
- [[enums]]

## Sources

- [[15-1-using-box-t-to-point-to-data-on-the-heap]]

