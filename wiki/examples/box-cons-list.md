---
title: "Box Cons List"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, box, recursive-types, enums]
source_count: 1
---

# Box Cons List

## Context

Bu example [[recursive-types|recursive type]]ni [[box-t|Box<T>]] yordamida finite size qilishni ko'rsatadi.

## Code

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

## Explanation

`Cons(i32, List)` yozilsa, `List` type'i o'z ichida yana `List` saqlaydi va compiler finite size topa olmaydi. `Box<List>` esa pointer size'iga ega; keyingi `List` heapda turadi.

Natija: logical list recursive, physical layout esa finite.

## Common Variation

Generic cons list ham yozish mumkin:

```rust
enum List<T> {
    Cons(T, Box<List<T>>),
    Nil,
}
```

Bu Chapter 15.1da soddalik uchun `i32` bilan ko'rsatilgan patternni generic qiladi.

## Related Pages

- [[15-1-using-box-t-to-point-to-data-on-the-heap]]
- [[box-t]]
- [[recursive-types]]
- [[enums]]
- [[e0072-recursive-type-has-infinite-size]]

