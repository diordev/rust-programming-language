---
title: "while Loop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, control-flow]
source_count: 2
---

# while Loop

## Short Definition

`while` loop condition `true` bo'lib turgan paytda blockni qayta-qayta bajaradi.

## Why It Matters

Conditionga bog'liq repetition kerak bo'lganda `while` ishlatiladi, lekin collection iteration uchun odatda [[for-loop|for loop]] afzal.

## Mental Model

Har iteration boshida condition tekshiriladi; `false` bo'lsa loop tugaydi.

Rustda alohida `do-while` syntax yo'q. Shuning uchun "kamida bir marta bajarilsin" semantikasi kerak bo'lsa, odatda `loop` + `break` yoki boshqa qayta yozish tanlanadi.

## Syntax and Examples

```rust
let mut n = 3;
while n != 0 {
    n -= 1;
}
```

## Common Mistakes

- Condition hech qachon `false` bo'lmaydigan loop yozish.
- Collection indexini manual boshqarib bounds riskini oshirish.

## Related Concepts

- [[loop]]
- [[for-loop|for loop]]
- [[boolean-type|bool]]

## Sources

- [[3-5-control-flow]]
- [[wiki/sources/rust-for-backend-developers-loops]]
