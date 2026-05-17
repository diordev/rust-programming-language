---
title: "Fully Qualified Syntax"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, syntax]
source_count: 1
---

# Fully Qualified Syntax

## Short Definition

`<Type as Trait>::function(args)` — bir xil nomli metodlar/funksiyalar orasida disambiguatsiya qilish uchun aniq sintaksis. "Type'ni Trait sifatida ko'rib, function'ni chaqir."

## Why It Matters

Bir tip bir nechta trait implement qilsa va trait'lar bir xil nomli metodga ega bo'lsa — kompilyatorga qaysi birini chaqirayotganingizni aytish kerak. Method'lar uchun `Trait::method(&val)` ham yetarli, lekin `self`siz associated function'lar uchun **fully qualified** kerak.

## Mental Model

```
person.fly()          →  metod resolution: type → traits (default: type)
Pilot::fly(&person)   →  trait method, self orqali aniq
<Dog as Animal>::baby_name()
                      →  associated fn (no self), trait + type ikkalasini aytib qo'yish
```

## Sintaksis

```rust
<Type as Trait>::function(receiver_if_method, next_arg, ...);
```

- Method bo'lsa: `receiver` — `&self`/`&mut self`/`self`.
- Associated function bo'lsa (no self): faqat argumentlar.

## Syntax and Examples

### Method'lar — `self` borligi yordam beradi

```rust
trait Pilot { fn fly(&self); }
trait Wizard { fn fly(&self); }
struct Human;

impl Pilot for Human {
    fn fly(&self) { println!("Captain speaking"); }
}
impl Wizard for Human {
    fn fly(&self) { println!("Up!"); }
}
impl Human {
    fn fly(&self) { println!("*waving arms*"); }
}

fn main() {
    let person = Human;
    person.fly();           // Human::fly  →  "*waving arms*"
    Pilot::fly(&person);    // "Captain speaking"
    Wizard::fly(&person);   // "Up!"
}
```

`person.fly()` default'da type'da to'g'ridan-to'g'ri belgilangan metodga boradi. Trait metodlari uchun `Trait::fly(&person)` yetarli — kompilyator `&person` tipi orqali implementatsiyani topadi.

### Associated function'lar — fully qualified zarur

```rust
trait Animal {
    fn baby_name() -> String;     // <-- self yo'q
}

struct Dog;

impl Dog {
    fn baby_name() -> String { String::from("Spot") }
}
impl Animal for Dog {
    fn baby_name() -> String { String::from("puppy") }
}

fn main() {
    println!("{}", Dog::baby_name());                     // "Spot" — Dog::
    // println!("{}", Animal::baby_name());               // E0790 — qaysi impl?
    println!("{}", <Dog as Animal>::baby_name());         // "puppy"
}
```

`Animal::baby_name()` — kompilyator `Self` tipini topa olmaydi (no `self` parameter). `<Dog as Animal>::baby_name()` aniq aytadi: "Dog'ni Animal sifatida ko'rib chaqir."

### Qachon ishlatish kerak

Kerak emas → kompilyator topa oladi:
- `value.method()` — type aniq.
- `Trait::method(&value)` — `&value` tip orqali aniq.

Kerak → noaniqlik bor:
- Bir nomli metodlar bir tip uchun ko'p trait'dan keladi.
- Associated function (no `self`) bir nechta tip uchun mumkin.

## Common Mistakes

- **Method uchun `<Type as Trait>` yozish keraksiz.** Sodda holatda `value.method()` yetarli; agar shunday yozsangiz — kod chiroyli emas.
- **`Trait::function()` ni associated fn uchun ishlatish.** `Self` topilmaydi; E0790 xato.
- **Lifetime'larni `as` ichida unutish.** Murakkab generic'larda `<Wrapper<'a, T> as MyTrait>::call()` kerak bo'lishi mumkin.

## Related Concepts

- [[traits]]
- [[trait-implementations|trait implementations]]
- [[associated-functions|associated functions]] — eng ko'p qachon kerak
- [[methods]]
- [[supertraits]]
- [[orphan-rule|orphan rule]]

## Sources

- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
