---
title: "Vector basics"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, collections, vectors]
source_count: 1
---

# Vector basics

## Goal

[[vector|Vec<T>]] yaratish, update qilish, element o'qish, va iterate qilishning asosiy patternlarini bitta joyda ko'rsatish.

## Code

```rust
fn main() {
    let mut values = vec![1, 2, 3];

    values.push(4);

    match values.get(2) {
        Some(value) => println!("third value: {value}"),
        None => println!("missing value"),
    }

    for value in &values {
        println!("{value}");
    }

    for value in &mut values {
        *value += 10;
    }
}
```

## Explanation

`vec![1, 2, 3]` `Vec<i32>` yaratadi. `values` mutable bo'lgani uchun `push` bilan yangi element qo'shish mumkin. `get(2)` third elementni `Option<&i32>` sifatida qaytaradi, shuning uchun missing index `None` bilan handle qilinadi.

`for value in &values` vectorni o'zgartirmasdan element references bo'ylab yuradi. `for value in &mut values` mutable references beradi; `*value += 10` reference ortidagi actual `i32`ni o'zgartiradi.

## Related Pages

- [[8-1-storing-lists-of-values-with-vectors]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[vector-indexing|vector indexing]]
- [[option|Option]]
- [[for-loop|for loop]]
- [[dereference-operator|dereference operator]]
