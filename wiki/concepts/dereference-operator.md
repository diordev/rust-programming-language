---
title: "Dereference Operator"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, references, deref, smart-pointers]
source_count: 5
---

# Dereference Operator

## Short Definition

Dereference operator `*` reference yoki `Deref` implement qilgan smart pointer ko'rsatayotgan value'ga murojaat qilish uchun ishlatiladi.

## Why It Matters

Mutable reference orqali value'ni o'zgartirishda reference'ning o'zini emas, u ko'rsatayotgan value'ni mutate qilish kerak. Vector mutable iterationida va [[hash-map|HashMap]] word count patternida bu darhol ko'rinadi.

Chapter 15.2da shu operator [[box-t|Box<T>]] va custom `MyBox<T>` bilan ham ishlashi ko'rsatiladi. Buning mexanizmi [[deref-trait|Deref trait]].

## Mental Model

`i` agar `&mut i32` bo'lsa, `*i` uning ichidagi `i32` value'dir.

`y` agar `Box<i32>` yoki `Deref<Target = i32>` implement qilgan wrapper bo'lsa, `*y` ichki `i32` value'ga yetadi.

Appendix B foydali clarification beradi: `*` bitta ma'noli belgi emas. U multiplication, dereference, va raw pointer type syntax rollarini context bo'yicha oladi.

Raw pointer holatida `*ptr` dereference ham bor, lekin reference'dan farqli ravishda bu faqat `unsafe` ichida ruxsat etiladi.

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

Smart pointer:

```rust
let y = Box::new(5);
assert_eq!(5, *y);
```

Raw pointer:

```rust
let value = 5;
let ptr = &value as *const i32;
unsafe {
    println!("{}", *ptr);
}
```

## Common Mistakes

- Mutable reference'ga to'g'ridan-to'g'ri `+=` ishlatish.
- `or_insert` value'ni emas, `&mut V` mutable reference qaytarishini unutish.
- Dereference operator va [[deref-coercions|deref coercions]]ni bir xil narsa deb o'ylash.
- Custom wrapper type avtomatik dereference bo'ladi deb o'ylash; buning uchun [[deref-trait|Deref trait]] kerak.

## Related Concepts

- [[reference]]
- [[mutable-reference|mutable reference]]
- [[borrowing]]
- [[deref-coercions|deref coercions]]
- [[deref-trait|Deref trait]]
- [[box-t|Box<T>]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[raw-pointer|raw pointer]]
- [[rust-operators-and-symbols]]

## Sources

- [[8-1-storing-lists-of-values-with-vectors]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[15-2-treating-smart-pointers-like-regular-references]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
