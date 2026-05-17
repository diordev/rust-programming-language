---
title: "E0384 cannot assign twice to immutable variable"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, variables]
source_count: 1
---

# E0384 cannot assign twice to immutable variable

## Symptom

Immutable variablega ikkinchi marta assignment qilinganda compiler `error[E0384]: cannot assign twice to immutable variable` chiqaradi.

## Cause

Rust variables default immutable. `let x = 5;` bilan binding yaratilgandan keyin `x = 6;` qilish mumkin emas.

## Fix Pattern

Agar value haqiqatan o'zgarishi kerak bo'lsa, `mut` qo'shing:

```rust
let mut x = 5;
x = 6;
```

Agar transformationdan keyin yangi immutable value kerak bo'lsa, [[shadowing]] ishlating:

```rust
let x = 5;
let x = x + 1;
```

## Minimal Example

Failing:

```rust
fn main() {
    let x = 5;
    x = 6;
}
```

Fixed:

```rust
fn main() {
    let mut x = 5;
    x = 6;
}
```

## Related Concepts

- [[3-1-variables-and-mutability]]
- [[variables-and-mutability|variables and mutability]]
- [[shadowing]]
