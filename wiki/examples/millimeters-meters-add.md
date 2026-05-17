---
title: "Millimeters + Meters — custom Rhs"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, operator-overloading, units]
source_count: 1
---

# Millimeters + Meters — Custom `Rhs`

## Listing 20-16 — Default generic parameter o'zgartirish

```rust
use std::ops::Add;

struct Millimeters(u32);
struct Meters(u32);

impl Add<Meters> for Millimeters {
    type Output = Millimeters;

    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}

fn main() {
    let mm = Millimeters(50);
    let m = Meters(2);

    let total = mm + m;        // Millimeters(2050)
    println!("{}", total.0);
}
```

## Tahlil

- `Add<Meters>` — default `Rhs = Self` o'rniga aniq `Meters` belgilanadi.
- `Output = Millimeters` — qaytish tipi (Meters bilan adashmaslik uchun bitta birlikka olib kelinadi).
- `self.0 + (other.0 * 1000)` — `Meters` `Millimeters` ga aylantirish.
- Newtype: `Millimeters(u32)` va `Meters(u32)` — ikkalasi `u32`, lekin **bir-biriga moslashmaydi**. Type safety:

```rust
let bad = mm + Millimeters(100);   // Add<Millimeters> impl yo'q — E0277
```

Faqat `Add<Meters>` mavjud, demak `Millimeters + Meters` ishlaydi, `Millimeters + Millimeters` ishlamaydi (qo'shimcha impl kerak).

## Default Generic Parameter — Ikki Maqsad

1. **Trait'ni kengaytirish backward-compatible.**
   `Add<Rhs = Self>` qo'shilganda eski `impl Add for Foo` kodi buzilmadi.

2. **Ko'p hollarda default ishlaydi, kerakda customize.**
   `Point + Point` — odatiy. `Millimeters + Meters` — maxsus holat.

## Asymmetric Operatsiya

`Millimeters + Meters` ishlaydi, lekin `Meters + Millimeters` — alohida `impl Add<Millimeters> for Meters` kerak. `+` Rust'da kommutativ emas — har yo'nalish alohida implementatsiya.

## Related

- [[default-generic-parameters|default generic parameters]] — `<Rhs = Self>`
- [[operator-overloading|operator overloading]]
- [[newtype-pattern|newtype pattern]] — `Millimeters(u32)`, `Meters(u32)`
- [[type-checking|type checking]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
