---
title: "Rectangle Area Example"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, structs]
source_count: 2
---

# Rectangle Area Example

## Goal

Rectangle area hisoblash example'i related data'ni alohida variablesdan [[tuple]]ga, keyin [[structs|struct]]ga, keyin [[methods|method]]ga refactor qilishni ko'rsatadi.

## Version 1: Separate Variables

```rust
fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

Bu ishlaydi, lekin `width` va `height` bitta rectangle'ga tegishli ekani type orqali bog'lanmagan.

## Version 2: Tuple

```rust
fn area(dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}
```

Tuple data'ni group qiladi, lekin meaning indexlarga bog'lanadi.

## Version 3: Struct

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

Struct field names data meaningni explicit qiladi. `&Rectangle` ownershipni olmaydi, faqat borrow qiladi.

## Version 4: Method

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

Method behaviorni `Rectangle` type bilan birga tashkil qiladi.

## Related Concepts

- [[structs]]
- [[struct-fields|struct fields]]
- [[methods]]
- [[impl-block|impl block]]
- [[borrowing]]
- [[debug-trait|Debug trait]]

## Sources

- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
- [[5-3-methods-the-rust-programming-language]]
