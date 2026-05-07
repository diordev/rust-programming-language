---
title: "Dereference Operator"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, references]
source_count: 2
---

# Dereference Operator

## Short Definition

Dereference operator `*` reference ko'rsatayotgan value'ga murojaat qilish uchun ishlatiladi.

## Why It Matters

Mutable reference orqali value'ni o'zgartirishda reference'ning o'zini emas, u ko'rsatayotgan value'ni mutate qilish kerak. Vector mutable iterationida va [[hash-map|HashMap]] word count patternida bu darhol ko'rinadi.

## Mental Model

`i` agar `&mut i32` bo'lsa, `*i` uning ichidagi `i32` value'dir.

## Syntax and Examples

```rust
let mut v = vec![100, 32, 57];

for i in &mut v {
    *i += 50;
}
```

Hash map word count:

```rust
let count = map.entry(word).or_insert(0);
*count += 1;
```

## Common Mistakes

- Mutable reference'ga to'g'ridan-to'g'ri `+=` ishlatish.
- `or_insert` value'ni emas, `&mut V` mutable reference qaytarishini unutish.
- Dereference operator va [[deref-coercions|deref coercions]]ni bir xil narsa deb o'ylash.

## Related Concepts

- [[reference]]
- [[mutable-reference|mutable reference]]
- [[borrowing]]
- [[deref-coercions|deref coercions]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
