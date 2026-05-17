---
title: "Animal/Dog::baby_name — fully qualified syntax"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, disambiguation, associated-functions]
source_count: 1
---

# Animal/Dog::baby_name — Fully Qualified Syntax

## Listing 20-20 — Trait + type'da bir xil nomli associated function

```rust
trait Animal {
    fn baby_name() -> String;     // <-- self parameter yo'q
}

struct Dog;

impl Dog {
    fn baby_name() -> String {
        String::from("Spot")
    }
}

impl Animal for Dog {
    fn baby_name() -> String {
        String::from("puppy")
    }
}
```

`Dog::baby_name()` ikki manbadan keladi:
1. `impl Dog` — direct associated fn (`"Spot"`)
2. `impl Animal for Dog` — trait impl (`"puppy"`)

## Listing 20-20 — `Dog::` default

```rust
fn main() {
    println!("{}", Dog::baby_name());   // "Spot"
}
```

`Dog::baby_name()` to'g'ridan-to'g'ri impl'ga boradi — yashirin trait emas.

## Listing 20-21 — `Animal::baby_name()` ishlamaydi

```rust
fn main() {
    println!("{}", Animal::baby_name());   // E0790
}
```

```text
error[E0790]: cannot call associated function on trait without
              specifying the corresponding `impl` type
```

Sabab: `baby_name` `self` parametri olmaydi. Kompilyator `Self` qaysi tip ekanini topa olmaydi (chunki `Animal` ko'p tip uchun implement qilingan bo'lishi mumkin).

## Listing 20-22 — Fully Qualified Syntax

```rust
fn main() {
    println!("{}", <Dog as Animal>::baby_name());   // "puppy"
}
```

`<Dog as Animal>::baby_name()` — "Dog'ni Animal sifatida ko'rib, `baby_name` ni chaqir."

## Sintaksis

```rust
<Type as Trait>::function(arg1, arg2, ...);
```

Method bo'lsa — birinchi argument `&self`/`&mut self`/`self`:

```rust
<Dog as Animal>::greet(&dog, "hello");
```

## Asosiy Farq

| Method (`&self`) | Associated fn (no `self`) |
|------------------|----------------------------|
| `Trait::method(&val)` yetarli | Fully qualified majburiy |
| `&val` orqali tip aniqlanadi | Tip annotatsiyasi kerak |

## Related

- [[fully-qualified-syntax|fully qualified syntax]]
- [[associated-functions|associated functions]]
- [[traits]]
- [[human-pilot-wizard|Human/Pilot/Wizard method disambiguation]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
