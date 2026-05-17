---
title: "Human + Pilot + Wizard — method disambiguation"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, disambiguation]
source_count: 1
---

# Human + Pilot + Wizard — Method Disambiguation

## Listing 20-17 — Bir xil nomli metodlar

```rust
trait Pilot {
    fn fly(&self);
}

trait Wizard {
    fn fly(&self);
}

struct Human;

impl Pilot for Human {
    fn fly(&self) {
        println!("This is your captain speaking.");
    }
}

impl Wizard for Human {
    fn fly(&self) {
        println!("Up!");
    }
}

impl Human {
    fn fly(&self) {
        println!("*waving arms furiously*");
    }
}
```

`Human` uchun **3 ta `fly`** mavjud:
1. `Human` ga to'g'ridan-to'g'ri implement qilingan
2. `Pilot` trait'i orqali
3. `Wizard` trait'i orqali

## Listing 20-18 — Default chaqiruv

```rust
fn main() {
    let person = Human;
    person.fly();      // *waving arms furiously*
}
```

`person.fly()` default'da **type'da to'g'ridan-to'g'ri belgilangan** metodga boradi (`impl Human`).

## Listing 20-19 — Trait method'larini chaqirish

```rust
fn main() {
    let person = Human;
    Pilot::fly(&person);    // This is your captain speaking.
    Wizard::fly(&person);   // Up!
    person.fly();           // *waving arms furiously*
}
```

`Trait::method(&value)` sintaksisi — trait nomini oldindan ko'rsatib disambiguatsiya. `&person` borligi tufayli kompilyator implementatsiyani topa oladi.

## Nima Uchun `self` Yetarli

`fly` metod `&self` qabul qiladi → kompilyator `&person` tipi (`&Human`) orqali qaysi `impl` ekanligini topa oladi. `Pilot::fly` chaqirig'ida `&person` argument sifatida — kompilyator `Pilot for Human` impl'ini topadi.

Lekin agar `fly` `self` parametri olmasa (associated function bo'lsa) — bu sintaksis ishlamaydi. Shu vaqtda [[fully-qualified-syntax|fully qualified syntax]] kerak (Listing 20-20..22 ga qarang).

## Output

```text
This is your captain speaking.
Up!
*waving arms furiously*
```

## Related

- [[fully-qualified-syntax|fully qualified syntax]]
- [[traits]]
- [[trait-implementations|trait implementations]]
- [[methods]]
- [[animal-dog-baby-name|Animal/Dog associated function disambiguation]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
