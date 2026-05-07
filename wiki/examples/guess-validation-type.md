---
title: "Guess Validation Type"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, example, error-handling, validation]
source_count: 1
---

# Guess Validation Type

## Summary

`Guess` custom type guessing game'dagi "guess 1 dan 100 gacha bo'lishi kerak" invariantini constructor va private field orqali saqlaydi.

## Code

```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {value}.");
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```

## Explanation

- `Guess::new` faqat valid range'ni qabul qiladi.
- Invalid range [[function-contracts|contract]] violation bo'lgani uchun example `panic!` qiladi.
- `value` field private, shuning uchun outside code validationni chetlab o'tolmaydi.
- `value(&self)` getter safe read access beradi.
- Function `Guess` parameter qabul qilsa, uning body'ida range check qayta yozilmaydi.

## Related Concepts

- [[custom-validation-types|custom validation types]]
- [[invariants]]
- [[bad-state|bad state]]
- [[panic-vs-result|panic! vs Result]]
- [[guessing-game|Guessing Game]]

## Sources

- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
