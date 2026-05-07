---
title: "Largest Function Extraction"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, functions, generics]
source_count: 1
---

# Largest Function Extraction

## Summary

Chapter 10 opening duplicated largest-number logicni `largest(list: &[i32]) -> &i32` functioniga extract qilish orqali abstractionni ko'rsatadi.

## Code

```rust
fn largest(list: &[i32]) -> &i32 {
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
    println!("The largest number is {result}");
    assert_eq!(*result, 100);

    let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&number_list);
    println!("The largest number is {result}");
    assert_eq!(*result, 6000);
}
```

## Explanation

- `list: &[i32]` functionga owned `Vec<i32>` emas, borrowed slice beradi.
- Return type `&i32` largest elementga borrowed reference.
- Function duplicate loopni bitta joyga jamlaydi.
- Bu generic functionga o'tishdan oldingi mental model: value parameter abstractiondan type parameter abstractionga o'tiladi.

## Related Concepts

- [[function-extraction|function extraction]]
- [[code-duplication|code duplication]]
- [[functions]]
- [[slices]]
- [[borrowing]]
- [[generics]]

## Sources

- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
