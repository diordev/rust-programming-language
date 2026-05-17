---
title: "E0790 cannot call associated function on trait"
type: error
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, error, traits]
source_count: 1
---

# E0790 — Cannot Call Associated Function on Trait

## Symptom

```text
error[E0790]: cannot call associated function on trait without specifying
              the corresponding `impl` type
  --> src/main.rs:20:43
   |
 2 |     fn baby_name() -> String;
   |     ------------------------- `Animal::baby_name` defined here
...
20 |     println!("{}", Animal::baby_name());
   |                    ^^^^^^^^^^^^^^^^^^^ cannot call associated function of trait
   |
help: use the fully-qualified path to the only available implementation
   |
20 |     println!("{}", <Dog as Animal>::baby_name());
   |                    +++++++       +
```

## Cause

`self` parameter olmaydigan trait associated function'ni `Trait::function()` shaklida chaqirish. Kompilyator `Self` qaysi tip ekanini topa olmaydi — chunki trait ko'p tip uchun implement qilingan bo'lishi mumkin.

## Fix Pattern

[[fully-qualified-syntax|Fully qualified syntax]] ishlatish:

```rust
<Type as Trait>::function(args)
```

## Minimal Example

```rust
trait Animal {
    fn baby_name() -> String;
}

struct Dog;

impl Animal for Dog {
    fn baby_name() -> String { String::from("puppy") }
}

fn main() {
    // println!("{}", Animal::baby_name());           // E0790
    println!("{}", <Dog as Animal>::baby_name());     // OK
}
```

## Method'lar Uchun Bo'lmaydi

`fn method(&self)` qabul qilsa — `Trait::method(&value)` yetarli, `&value` orqali tip aniqlanadi:

```rust
trait Greet { fn hello(&self); }
impl Greet for String { fn hello(&self) { println!("hi from {self}"); } }

let s = String::from("Ada");
Greet::hello(&s);   // OK — &s tipi orqali topiladi
```

E0790 faqat `self`'siz associated function'larda chiqadi.

## Related

- [[fully-qualified-syntax|fully qualified syntax]]
- [[associated-functions|associated functions]]
- [[traits]]
- [[trait-implementations|trait implementations]]
- [[animal-dog-baby-name|Animal/Dog::baby_name example]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
