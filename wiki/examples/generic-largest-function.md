---
title: "Generic Largest Function"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, generics, functions]
source_count: 1
---

# Generic Largest Function

## Summary

`largest_i32` va `largest_char` bir xil loop logicini ishlatadi. Generic function bu duplicationni `fn largest<T>(list: &[T]) -> &T` signature orqali kamaytiradi, lekin `>` ishlatilgani uchun `T` [[partial-ord|PartialOrd]] bilan cheklanishi kerak.

## Code

Failing generic version:

```rust
fn largest<T>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Fixed version with trait bound:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];
    let result = largest(&number_list);
    assert_eq!(*result, 100);

    let char_list = vec!['y', 'm', 'a', 'q'];
    let result = largest(&char_list);
    assert_eq!(*result, 'y');
}
```

## Explanation

- `largest<T>` `T`ni function signature'da e'lon qiladi.
- `list: &[T]` borrowed slice; function list ownershipini olmaydi.
- `-> &T` return value input slice ichidagi elementga reference.
- `T: std::cmp::PartialOrd` `>` operation uchun kerak.
- Empty slice hali alohida muammo: `list[0]` panic qiladi. Bu example generic syntaxga qaratilgan.

## Related Concepts

- [[generic-functions|generic functions]]
- [[generic-type-parameters|generic type parameters]]
- [[partial-ord|PartialOrd]]
- [[traits]]
- [[slices]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]

## Sources

- [[10-1-generic-data-types-the-rust-programming-language]]
