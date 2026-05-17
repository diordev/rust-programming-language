---
title: "Pair Conditional Methods"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, traits, generics, methods]
source_count: 1
---

# Pair Conditional Methods

## Summary

`Pair<T>` example'i method availability trait boundsga bog'lanishini ko'rsatadi. `new` har qanday `T` uchun bor, `cmp_display` esa faqat `T: Display + PartialOrd` bo'lsa bor.

## Code

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

## Explanation

- `impl<T> Pair<T>` all `Pair<T>` instantiations uchun method beradi.
- `impl<T: Display + PartialOrd> Pair<T>` faqat `T` ikkala traitni implement qilganda method beradi.
- `Display` `{}` formatting uchun, `PartialOrd` `>=` comparison uchun kerak.
- Bu pattern generic typeda faqat valid operationsni expose qiladi.

## Related Concepts

- [[conditional-method-implementations|conditional method implementations]]
- [[generic-methods|generic methods]]
- [[trait-bounds|trait bounds]]
- [[display-formatting|Display]]
- [[partial-ord|PartialOrd]]

## Sources

- [[10-2-defining-shared-behavior-with-traits]]
