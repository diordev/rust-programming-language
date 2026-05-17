---
title: "Higher-Order Functions"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, functions, closures, functional]
source_count: 1
---

# Higher-Order Functions

## Short Definition

Boshqa function yoki closure'ni argument sifatida qabul qiladigan yoki function/closure qaytaradigan function.

## Why It Matters

Callback, adapter, transformer, builder, va configured behavior patternlari shu modelga tayanadi. Rustda bu ko'pincha `fn(...)`, `F: Fn(...)`, `impl Fn(...)`, yoki `Box<dyn Fn(...)>` orqali ifodalanadi.

## Mental Model

Higher-order function "behaviorni data kabi uzatish" imkonini beradi. Rustda bu ikki asosiy ko'rinishda chiqadi:

- argument sifatida callable qabul qilish;
- natija sifatida callable qaytarish.

Oddiy named function, non-capturing anonymous function, va capturing closure bir xil familyada ishlaydi, lekin type modeli bir xil emas.

## Syntax and Examples

Argument sifatida callable:

```rust
fn transform(a: i32, f: fn(i32) -> i32) -> i32 {
    f(a)
}
```

Moslashuvchanroq generic variant:

```rust
fn transform<F>(a: i32, f: F) -> i32
where
    F: Fn(i32) -> i32,
{
    f(a)
}
```

Callable qaytarish:

```rust
fn make_adder(step: i32) -> impl Fn(i32) -> i32 {
    move |x| x + step
}
```

## Common Mistakes

- `fn(...)` va `F: Fn(...)` bir xil moslashuvchanlik beradi deb o'ylash.
- Capturing closure'ni `fn` pointer sifatida qaytarishga urinish.
- Turli closure branchlarini `impl Fn` ostida bitta concrete type deb qabul qilish.

## Related Concepts

- [[functions]]
- [[function-pointers|function pointers]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]

## Sources

- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
