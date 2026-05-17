---
title: "PartialOrd"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, traits, comparisons]
source_count: 4
---

# PartialOrd

## Short Definition

`PartialOrd` qiymatlarni partial ordering bilan taqqoslashga ruxsat beradi; uning markaziy methodi `partial_cmp` bo'lib, `Option<Ordering>` qaytaradi.

## Why It Matters

Hamma qiymatlar ham har doim taqqoslanavermaydi. `PartialOrd` shu noaniqlikni type systemga ko'taradi, ayniqsa float `NaN` holatida.

## Mental Model

`PartialEq` "tengmi?" degan savolni beradi. `PartialOrd` esa "tartib bormi?" degan savolni qo'shadi. Javob har doim bo'lmasligi mumkin, shu sabab return type `Option<Ordering>`.

`None` qaytsa, bu taqqoslab bo'lmaydigan juftlik.

## Syntax and Examples

```rust
println!("{:?}", 4.0.partial_cmp(&5.0));           // Some(Less)
println!("{:?}", f32::NAN.partial_cmp(&f32::NAN)); // None
```

```rust
pub trait PartialOrd<Rhs = Self>: PartialEq<Rhs>
where
    Rhs: ?Sized,
{
    fn partial_cmp(&self, other: &Rhs) -> Option<Ordering>;
}
```

## Common Mistakes

- `PartialOrd` total orderingni kafolatlaydi deb o'ylash.
- `partial_cmp` `Ordering` qaytaradi deb o'ylash.
- `Ord` implement qilinadigan type uchun `PartialOrd`ni alohida boshqa logic bilan yozib yuborish.

## Related Concepts

- [[ord-trait|Ord]]
- [[ordering]]
- [[partial-eq]]
- [[traits]]

## Sources

- [[10-1-generic-data-types]]
- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/22-3-c-derivable-traits|22.3]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
