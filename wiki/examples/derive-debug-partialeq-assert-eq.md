---
title: "derive Debug and PartialEq for assert_eq!"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, derive, debug, partialeq, testing]
source_count: 1
---

# derive Debug and PartialEq for assert_eq!

`assert_eq!` custom type bilan ishlashi uchun amalda ko'pincha `Debug` va `PartialEq` birga kerak bo'ladi.

```rust
#[derive(Debug, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

assert_eq!(
    Point { x: 1, y: 2 },
    Point { x: 1, y: 2 }
);
```

## Why It Matters

`PartialEq` tenglikni tekshiradi. `Debug` esa assertion fail bo'lsa ikkala qiymatni chiqarish imkonini beradi.

## Related

- [[derive-attribute]]
- [[debug-trait]]
- [[partial-eq]]
- [[test-macros|test macros (assert!, assert_eq!, assert_ne!)]]
