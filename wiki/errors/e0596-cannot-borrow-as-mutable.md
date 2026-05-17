---
title: "E0596 cannot borrow as mutable"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, borrowing]
source_count: 1
---

# E0596 cannot borrow as mutable

## Symptom

Immutable reference orqali borrowed data'ni mutate qilishga uringanda compiler `error[E0596]: cannot borrow ... as mutable` chiqaradi.

## Cause

References default immutable. `&String` orqali `push_str` kabi mutating method chaqirib bo'lmaydi.

## Fix Pattern

Bindingni mutable qiling, call site'da `&mut`, function signatureda `&mut String` ishlating:

```rust
let mut s = String::from("hello");
change(&mut s);

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

## Minimal Example

Failing:

```rust
fn change(some_string: &String) {
    some_string.push_str(", world");
}
```

Fixed:

```rust
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

## Related Concepts

- [[4-2-references-and-borrowing]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
