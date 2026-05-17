---
title: "E0614 type cannot be dereferenced"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, compiler-error, deref, smart-pointers]
source_count: 1
---

# E0614 type cannot be dereferenced

## Symptom

`*value` yozilganda compiler bu type dereference qilinmasligini aytadi.

## Cause

`*` operator oddiy reference'lar bilan ishlaydi. Custom wrapper type reference kabi ishlashi uchun [[deref-trait|Deref trait]] implement qilishi kerak.

Failing:

```rust
struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

fn main() {
    let y = MyBox::new(5);
    assert_eq!(5, *y);
}
```

## Fix Pattern

`std::ops::Deref` implement qiling va inner value'ga reference qaytaring.

## Minimal Example

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

fn main() {
    let y = MyBox::new(5);
    assert_eq!(5, *y);
}
```

## Related Concepts

- [[deref-trait|Deref trait]]
- [[dereference-operator]]
- [[smart-pointers]]
- [[trait-implementations|trait implementations]]
- [[mybox-deref]]

## Sources

- [[15-2-treating-smart-pointers-like-regular-references]]

