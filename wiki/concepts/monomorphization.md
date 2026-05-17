---
title: "Monomorphization"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-16
tags: [rust, generics, performance]
source_count: 3
---

# Monomorphization

## Short Definition

Monomorphization Rust compiler generic code'ni compile time'da ishlatilgan concrete typelar bo'yicha specialized codega aylantirish jarayoni.

## Why It Matters

Monomorphization sabab Rust [[generics]]i runtime'da sekinlashuv keltirmaydi. Generic abstraction code reuse beradi, compiler esa kerakli concrete versionsni yaratadi.

## Mental Model

Developer `Option<T>` kabi generic definition yozadi. Compiler programda `Option<i32>` va `Option<f64>` ishlatilganini ko'rsa, mental model sifatida alohida `Option_i32` va `Option_f64`ga o'xshash concrete definitions yaratadi. Trait parameterlari uchun `&impl Trait` yoki `T: Trait` ham shu modelda ishlaydi: concrete type ma'lum bo'lgach, compiler specialized variant yaratadi.

Bu hand-written duplicated concrete codega o'xshash runtime behavior beradi. Cost asosan compile time va mumkin bo'lgan binary size tomonda bo'lishi mumkin; source bu sectionda runtime cost yo'qligiga urg'u beradi.

Backend beginner source `Holder<T>` va `make_holder<T>` bilan shu jarayonni yanada ko'rinarli qiladi: `Holder<i32>` va `Holder<bool>` mental modelda alohida structlarga, generic function esa alohida specialized functionlarga aylanadi. Source Java/C# type erasure'ni contrast sifatida tilga oladi; wiki uchun saqlanadigan nuqta "Rust generic info compile time specializationga xizmat qiladi", xolos.

## Syntax and Examples

Source code:

```rust
let integer = Some(5);
let float = Some(5.0);
```

Compiler mental modelda shunga o'xshash specialized definitions yaratadi:

```rust
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}
```

`Holder<T>` style mental model:

```rust
struct Holder_i32 { v: i32 }
struct Holder_bool { v: bool }
```

## Common Mistakes

- Generic code dynamic runtime dispatch bo'ladi deb o'ylash.
- "No runtime cost" compile time yoki binary sizega hech qanday ta'sir yo'q degani deb o'ylash.
- Monomorphizationni `trait object` dispatch bilan aralashtirish.
- Type erasure comparisonini ortiqcha umumlashtirish.

## Related Concepts

- [[generics]]
- [[generic-enums|generic enums]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[compiler]]
- [[option|Option]]
- [[performance]]
- [[static-dispatch|static dispatch]]
- [[turbofish]]

## Sources

- [[10-1-generic-data-types]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-generics]]
