---
title: "Code Duplication"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, abstraction, refactoring]
source_count: 1
---

# Code Duplication

## Short Definition

Code duplication - bir xil yoki deyarli bir xil logic bir nechta joyda takrorlanishi.

## Why It Matters

Duplication tedious va error-prone. Logic o'zgarsa, har copy joyini update qilish kerak; bittasi unutilsa, behavior ajralib ketadi.

## Mental Model

Duplicate code "bu yerda bitta concept bor, lekin code uni bir nechta copy sifatida ifodalayapti" degani. Rust Book Chapter 10 duplicationni avval function extraction bilan, keyin generics bilan kamaytirishni ko'rsatadi.

## Syntax and Examples

Ikki list uchun bir xil largest loop yozish duplication:

```rust
let mut largest = &number_list[0];

for number in &number_list {
    if number > largest {
        largest = number;
    }
}
```

Better abstraction:

```rust
fn largest(list: &[i32]) -> &i32 {
    // same logic in one place
}
```

## Common Mistakes

- Copy-paste code ishlagani uchun abstraction kerak emas deb o'ylash.
- Duplicated logicni o'zgartirganda barcha copiesni update qilmaslik.
- Juda erta abstraction qilib, hali aniq bo'lmagan patternni yashirish.

## Related Concepts

- [[function-extraction|function extraction]]
- [[generics]]
- [[abstraction]]
- [[functions]]

## Sources

- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
