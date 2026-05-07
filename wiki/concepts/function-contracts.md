---
title: "Function Contracts"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, api-design, error-handling]
source_count: 1
---

# Function Contracts

## Short Definition

Function contract - function behaviori guaranteed bo'lishi uchun caller bajarishi kerak bo'lgan input va state talablari.

## Why It Matters

Contract violation odatda caller-side bug. Agar continuation insecure yoki harmful bo'lsa, panic qilish mantiqli bo'lishi mumkin. Public API'da panic conditions documentationda yozilishi kerak.

## Mental Model

Contract "agar mana shu shartlar bajarilsa, function mana shunday ishlaydi" degani. Shart buzilsa, function normal result qaytarishga majbur emas.

## Syntax and Examples

```rust
impl Guess {
    /// Creates a Guess.
    ///
    /// Panics if value is not between 1 and 100.
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {value}.");
        }

        Guess { value }
    }
}
```

## Common Mistakes

- Contract violationni expected user error deb handle qilish.
- Public function panic qilishi mumkin bo'lgan shartlarni yashirish.
- Type system orqali contractni encode qilish mumkin bo'lgan joyda plain primitive type ishlatish.

## Related Concepts

- [[api-design|API design]]
- [[panic-vs-result|panic! vs Result]]
- [[bad-state|bad state]]
- [[invariants]]
- [[documentation-comments|documentation comments]]
- [[public-api|public API]]

## Sources

- [[9-3-to-panic-or-not-to-panic-the-rust-programming-language]]
