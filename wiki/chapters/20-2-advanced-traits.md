---
title: "20.2. Advanced Traits"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, generics, advanced]
source_count: 1
---

# 20.2. Advanced Traits

## Learning Goal

Trait'larni chuqurroq tushunish: [[associated-types|associated types]], [[default-generic-parameters|default generic parameters]], method [[fully-qualified-syntax|disambiguation]], [[supertraits]], va [[newtype-pattern]]. Qachon associated type, qachon generic; qachon supertrait, qachon trait bound — bu farqlarni o'zlashtirish.

## Main Ideas

- **Associated type** — bitta tip uchun bitta implementatsiya, caller annotatsiyasiz ishlatadi.
- **Generic param** — bitta tip uchun ko'p implementatsiya mumkin, lekin caller annotatsiya kerak.
- **Default generic parameter** — `<Rhs = Self>` — operator overloading va backward-compatible kengaytirish.
- **Method disambiguation** — `Trait::method(&val)` (self bilan) yoki `<Type as Trait>::fn()` (associated fn).
- **Supertrait** — bir trait body'da boshqa trait method'larini ishlatish uchun.
- **Newtype pattern** — orphan rule chetlab o'tish, type safety, API yashirish.

## Bobning Tarkibi

| Bo'lim | Mavzu |
|--------|-------|
| Associated Types | `type Item;` placeholder, `Iterator` misoli |
| Default Generic Params | `<Rhs = Self>`, operator overloading, `Add` |
| Disambiguation | Method (`Trait::fly(&val)`) va associated fn (`<Type as Trait>::fn()`) |
| Supertraits | `trait OutlinePrint: Display` |
| Newtype Pattern | `struct Wrapper(Vec<String>)`, orphan rule chetlab o'tish |

## Asosiy Misollar

### Associated Type — `Iterator`

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Counter {
    type Item = u32;
    fn next(&mut self) -> Option<Self::Item> { /* ... */ }
}
```

`counter.next()` avtomatik `Option<u32>` qaytaradi.

### Default Generic — `Add`

```rust
trait Add<Rhs = Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}

// Default Self
impl Add for Point { /* Point + Point */ }

// Custom Rhs
impl Add<Meters> for Millimeters { /* Millimeters + Meters */ }
```

### Method Disambiguation

```rust
trait Pilot { fn fly(&self); }
trait Wizard { fn fly(&self); }

let person = Human;
person.fly();           // type'ga to'g'ridan-to'g'ri impl
Pilot::fly(&person);    // Pilot impl
Wizard::fly(&person);   // Wizard impl
```

### Fully Qualified — `<Type as Trait>::fn()`

```rust
trait Animal { fn baby_name() -> String; }   // self yo'q

<Dog as Animal>::baby_name()    // "puppy"
```

### Supertrait

```rust
trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let s = self.to_string();   // Display'dan
        // ...
    }
}
```

### Newtype

```rust
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}
```

## Concepts

- [[associated-types|associated types]]
- [[default-generic-parameters|default generic parameters]]
- [[operator-overloading|operator overloading]]
- [[fully-qualified-syntax|fully qualified syntax]]
- [[supertraits]]
- [[newtype-pattern|newtype pattern]]
- [[orphan-rule|orphan rule]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[generics]]
- [[iterators]]
- [[display-formatting|Display formatting]]
- [[tuple-structs|tuple structs]]
- [[deref-trait|Deref trait]]

## Examples

- [[point-add-overload|Point Add overload]] — Listing 20-15
- [[millimeters-meters-add|Millimeters + Meters]] — Listing 20-16
- [[human-pilot-wizard|Method disambiguation]] — Listing 20-17..20-19
- [[animal-dog-baby-name|Fully qualified syntax]] — Listing 20-20..20-22
- [[outline-print-supertrait|Supertrait OutlinePrint]] — Listing 20-23
- [[wrapper-newtype-display|Newtype Wrapper]] — Listing 20-24

## Errors

- [[e0790-cannot-call-associated-function|E0790]] — `Trait::fn()` self'siz associated fn uchun
- [[e0277-trait-bound-not-satisfied|E0277]] — supertrait talabi bajarilmagan

## Exercises

1. **Custom iterator:** `Fibonacci` struct, `Iterator` trait `Item = u64;` bilan. `take(10).collect()`.
2. **Operator overload:** `Vec3 { x, y, z }` uchun `Add`, `Sub`, `Mul<f64>`, `Neg`. Dot product alohida.
3. **Method disambiguation:** Ikki trait (`Worker`, `Volunteer`) — bir xil nomli `report()`. `Person` ikkalasini va to'g'ridan-to'g'ri ham implement.
4. **Fully qualified:** `<Vec<i32> as Default>::default()` yozing.
5. **Supertrait:** `Json` trait, `Debug + Clone` supertrait. Default `to_json()` metod.
6. **Newtype:** `Email(String)` — `new()` validatsiya qiladi (`@` borligi), `Display` impl.

## Review Questions

1. Generic parameter va associated type orasidagi asosiy farq nima?
2. `Iterator` trait nega associated type ishlatadi, generic emas?
3. `Add<Rhs = Self>` da `Self` o'rniga `Self: Sized` deb yozilmaganmi? Nima uchun?
4. `person.fly()` qaysi `fly` ni chaqiradi (3 ta bor)?
5. Nega `Animal::baby_name()` ishlamaydi, lekin `Pilot::fly(&person)` ishlaydi?
6. `<Dog as Animal>::baby_name()` — bu sintaksis qachon zarur?
7. Supertrait va trait bound farqi nima?
8. Newtype'ga ulgurji `Vec<T>` metodlari avtomatik keladimi?
9. Newtype + `Deref` ning afzalligi va kamchiligi nima?
10. Orphan rule nimani man qiladi va newtype undan qanday chetlab o'tadi?

## Related Pages

- [[wiki/sources/20-2-advanced-traits|Source: 20.2]]
- [[wiki/chapters/20-1-unsafe-rust|Chapter 20.1: Unsafe Rust]]
- [[wiki/chapters/10-generic-types-traits-and-lifetimes|Chapter 10: Generics, Traits, and Lifetimes]] — bazaviy bilim
- [[10-2-defining-shared-behavior-with-traits|Source: 10.2 Traits asosi]]
