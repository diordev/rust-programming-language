---
title: "Return Values"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, functions]
source_count: 1
---

# Return Values

## Short Definition

Return value function callerga qaytaradigan value.

## Why It Matters

Rustda final expression semicolonsiz yozilsa function return value bo'lishi mumkin.

## Mental Model

Block oxiridagi expression value beradi; semicolon qo'yilsa u statementga aylanadi va `()` qaytaradi.

## Syntax and Examples

```rust
fn five() -> i32 {
    5
}
```

## Common Mistakes

- Return type'ni `->` bilan yozishni unutish.
- Final expressiondan keyin semicolon qo'yish.

## Related Concepts

- [[functions]]
- [[statements-and-expressions|statements and expressions]]
- [[unit-type|unit type]]

## Sources

- [[3-3-functions]]
