---
title: "Invariants"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, type-system, error-handling]
source_count: 1
---

# Invariants

## Short Definition

Invariant - code to'g'ri ishlashi uchun doim rost bo'lishi kerak bo'lgan condition.

## Why It Matters

Rustda invariantlarni type system va privacy orqali encode qilish mumkin. Shunda functionlar har safar runtime check yozmasdan, type qoidalariga tayanadi.

## Mental Model

Invariantni "bu value hech qachon mana shu qoidadan chiqmasligi kerak" deb o'qing. Masalan `Guess` value'i 1..=100 oralig'ida bo'lishi invariant.

## Syntax and Examples

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
}
```

Private field outside code invariantni chetlab o'tmasligini ta'minlaydi.

## Common Mistakes

- Invariantni faqat commentda yozib, type/API orqali enforce qilmaslik.
- Fieldni public qilib, constructor validationni chetlab o'tishga ruxsat berish.
- Expected user input failure bilan invariant violationni aralashtirish.

## Related Concepts

- [[custom-validation-types|custom validation types]]
- [[function-contracts|function contracts]]
- [[bad-state|bad state]]
- [[privacy]]
- [[type-checking|type checking]]

## Sources

- [[9-3-to-panic-or-not-to-panic]]
