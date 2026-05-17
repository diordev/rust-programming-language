---
title: "MyBox Deref Example"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, deref, smart-pointers, traits]
source_count: 1
---

# MyBox Deref Example

## Context

Bu example custom smart pointerga o'xshash wrapper type uchun [[deref-trait|Deref trait]] implement qilishni ko'rsatadi.

## Code

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

fn hello(name: &str) {
    println!("Hello, {name}!");
}

fn main() {
    let x = 5;
    let y = MyBox::new(x);

    assert_eq!(5, *y);

    let m = MyBox::new(String::from("Rust"));
    hello(&m);
}
```

## Explanation

`Deref` implement qilinmasa, `*y` ishlamaydi va [[e0614-type-cannot-be-dereferenced|E0614]] chiqadi. `deref` methodi inner value'ga reference qaytargani uchun compiler `*y`ni `*(y.deref())` kabi ishlata oladi.

`hello(&m)` esa [[deref-coercions|deref coercion]] sabab ishlaydi: `&MyBox<String>` -> `&String` -> `&str`.

## Related Pages

- [[15-2-treating-smart-pointers-like-regular-references]]
- [[deref-trait]]
- [[deref-coercions]]
- [[dereference-operator]]
- [[smart-pointers]]

