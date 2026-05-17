---
title: "Custom Validation Types"
type: pattern
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, pattern, validation, type-system]
source_count: 1
---

# Custom Validation Types

## Pattern

Primitive value ustidagi muhim validation rule'ni custom type ichiga joylang. Fieldni private qiling, public constructor validation qilsin, va public methods faqat valid state bilan ishlasin.

## Why It Matters

Bu pattern range, format, yoki domain-specific invariantni codebase bo'ylab qayta-qayta tekshirish o'rniga type signature ichida ifodalaydi. Function `i32` emas `Guess` qabul qilsa, caller valid value yaratganini compiler/API surface orqali ko'rasiz.

## Example

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

`value` field private bo'lgani uchun outside code `Guess { value: 200 }` qila olmaydi. Outside code `Guess::new` orqali o'tishi kerak.

## Tradeoffs

- Constructor panic qilishi mumkin bo'lsa, panic condition documentationda yozilishi kerak.
- Agar invalid input expected user data bo'lsa, constructor `Result<Guess, E>` qaytarishi yaxshiroq bo'lishi mumkin.
- Custom type ko'proq code talab qiladi, lekin invariant muhim bo'lsa repeated checksni kamaytiradi.

## Related Pages

- [[guess-validation-type|Guess validation type]]
- [[invariants]]
- [[function-contracts|function contracts]]
- [[bad-state|bad state]]
- [[privacy]]
- [[api-design|API design]]
- [[9-3-to-panic-or-not-to-panic]]
