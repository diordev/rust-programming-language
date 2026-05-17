---
title: "20.2. Advanced Traits"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, traits, generics, advanced]
source_count: 1
---

# 20.2. Advanced Traits

## Source

- File: `raw/books/the_rust_programming_language/20.2. Advanced Traits.md`
- URL: <https://doc.rust-lang.org/stable/book/ch20-02-advanced-traits.html>
- Book: *The Rust Programming Language*

## Detailed Summary

10-bobning traits asosini eslatib, bu bo'lim **chuqurroq** xususiyatlarni ochadi: associated types, default generic parameters, method disambiguation, supertraits, va newtype pattern.

### Associated Types

[[associated-types|Associated type]] — trait ichida `type Name;` bilan e'lon qilingan **type placeholder**. Method signaturalarda ishlatiladi; har implementor concrete type beradi.

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

`Counter` uchun:

```rust
impl Iterator for Counter {
    type Item = u32;
    fn next(&mut self) -> Option<Self::Item> { /* ... */ }
}
```

**Generic parameter'dan farqi:** generic bo'lsa, `Iterator<u32> for Counter` va `Iterator<String> for Counter` ikkalasi mumkin → caller annotatsiya berishi kerak. Associated type bilan **bitta tip uchun bitta implementatsiya** → `counter.next()` avtomatik `Option<u32>`.

Associated type — trait kontraktining qismi; har implementor placeholder o'rniga aniq tip berishi shart.

### Default Generic Parameters va Operator Overloading

[[default-generic-parameters|Default generic parameter]]: `<PlaceholderType=ConcreteType>` — caller tip bermasa, default ishlatiladi.

[[operator-overloading|Operator overloading]]: `+`, `-`, `*` va boshqa operatorlar uchun custom xatti-harakat — `std::ops` ichidagi trait'larni implement qilish orqali. Rust **mavjud** operatorlarni overload qilishga ruxsat beradi; yangi operator yaratish mumkin emas.

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point { x: i32, y: i32 }

impl Add for Point {
    type Output = Point;
    fn add(self, other: Point) -> Point {
        Point { x: self.x + other.x, y: self.y + other.y }
    }
}
```

`Add` traiting haqiqiy ta'rifi:

```rust
trait Add<Rhs = Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}
```

`Rhs = Self` — default. `Rhs` ni o'zgartirish — turli tiplar orasida qo'shish:

```rust
struct Millimeters(u32);
struct Meters(u32);

impl Add<Meters> for Millimeters {
    type Output = Millimeters;
    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + other.0 * 1000)
    }
}
```

Default generic parameter ikki maqsad uchun ishlatiladi:
1. **Mavjud kodni buzmasdan tip kengaytirish** (backward compatible).
2. **Maxsus holatlarda customization** (aksariyat default'ni xohlaydi).

### Disambiguating Identically Named Methods

Bir tip bir nechta trait'ni implement qilsa va trait'lar bir xil nomli metodga ega bo'lsa — kompilyatorga qaysi birini chaqirayotganingizni aytish kerak.

```rust
trait Pilot { fn fly(&self); }
trait Wizard { fn fly(&self); }
struct Human;

impl Pilot  for Human { fn fly(&self) { println!("Captain"); } }
impl Wizard for Human { fn fly(&self) { println!("Up!"); } }
impl Human { fn fly(&self) { println!("*waving*"); } }

fn main() {
    let person = Human;
    person.fly();           // *waving*    — type'ga to'g'ridan-to'g'ri impl
    Pilot::fly(&person);    // Captain     — trait nomi bilan
    Wizard::fly(&person);   // Up!
}
```

`&person` argument — kompilyator `&Human` tipi orqali implementatsiyani topa oladi.

### Fully Qualified Syntax — Associated Function'lar Uchun

`self` olmaydigan associated function'larda `Trait::function()` ishlamaydi (kompilyator `Self`'ni topa olmaydi):

```rust
trait Animal { fn baby_name() -> String; }
struct Dog;
impl Dog { fn baby_name() -> String { String::from("Spot") } }
impl Animal for Dog { fn baby_name() -> String { String::from("puppy") } }

fn main() {
    println!("{}", Dog::baby_name());                // "Spot"
    // println!("{}", Animal::baby_name());           // E0790
    println!("{}", <Dog as Animal>::baby_name());    // "puppy"
}
```

[[fully-qualified-syntax|Fully qualified syntax]]: `<Type as Trait>::function(args)` — "Type'ni Trait sifatida ko'rib funksiyani chaqir."

### Supertraits

[[supertraits]]: bir trait'ning ishlashi uchun boshqa trait'ning ham implement qilinishi shart bo'lsa, ikkinchisini *supertrait* deb atayman.

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();   // Display'dan keladi
        let len = output.len();
        println!("{}", "*".repeat(len + 4));
        println!("*{}*", " ".repeat(len + 2));
        println!("* {output} *");
        println!("*{}*", " ".repeat(len + 2));
        println!("{}", "*".repeat(len + 4));
    }
}
```

`OutlinePrint`'ni `Point`'ga implement qilish uchun avval `Display`'ni implementatsiya qilish kerak — aks holda [[e0277-trait-bound-not-satisfied|E0277]].

### Newtype Pattern — Orphan Rule'dan O'tib O'tish

[[orphan-rule|Orphan rule]]: trait yoki tip — kamida bittasi mahalliy bo'lishi shart. `Display for Vec<T>` to'g'ridan-to'g'ri yozib bo'lmaydi (ikkalasi tashqi).

Yechim — [[newtype-pattern]]:

```rust
use std::fmt;

struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

fn main() {
    let w = Wrapper(vec![String::from("hello"), String::from("world")]);
    println!("w = {w}");   // w = [hello, world]
}
```

`Wrapper` — yangi mahalliy tip. `self.0` orqali ichidagi `Vec<String>` ga kiriladi. Compile time'da `Wrapper` qatlami yo'qoladi (zero overhead).

Cheklov: inner tip metodlari avtomatik kelmaydi. Yechimlar:
- Qo'lda delegate (`pub fn push(&mut self, s: String) { self.0.push(s); }`).
- [[deref-trait|Deref]] implement (lekin type safety pasayadi).

Newtype faqat orphan rule'dan o'tish emas — type safety, API yashirish, va domain modeling uchun ham:
- `Millimeters(u32)` ↔ `Meters(u32)` — aralashtirib bo'lmaydi.
- `UserId(u64)` — caller `+`, `*` ishlata olmaydi (yashirin).

## Key Concepts

- [[associated-types|associated types]]
- [[default-generic-parameters|default generic parameters]]
- [[operator-overloading|operator overloading]]
- [[fully-qualified-syntax|fully qualified syntax]]
- [[supertraits]]
- [[newtype-pattern|newtype pattern]]
- [[orphan-rule|orphan rule]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[trait-implementations|trait implementations]]
- [[generics]]
- [[iterators]] — `Iterator::Item` associated type
- [[display-formatting|Display formatting]]
- [[tuple-structs|tuple structs]] — newtype asosi
- [[deref-trait|Deref trait]]

## Code Examples

- [[point-add-overload|Point + Point operator overload]] — Listing 20-15
- [[millimeters-meters-add|Millimeters + Meters custom Rhs]] — Listing 20-16
- [[human-pilot-wizard|Human/Pilot/Wizard method disambiguation]] — Listing 20-17..20-19
- [[animal-dog-baby-name|Animal/Dog associated fn fully qualified syntax]] — Listing 20-20..20-22
- [[outline-print-supertrait|OutlinePrint supertrait]] — Listing 20-23
- [[wrapper-newtype-display|Wrapper newtype Display for Vec<String>]] — Listing 20-24

## Exercises or Practice Ideas

1. **Custom iterator:** `Fibonacci` strukturasi yarating, `Iterator` trait'ini `type Item = u64;` bilan implement qiling. `Fibonacci::new().take(10).collect::<Vec<_>>()` `[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]` qaytarishi kerak.
2. **Operator overload:** `Vec3 { x, y, z: f64 }` strukturasi uchun `Add`, `Sub`, `Mul<f64>`, `Neg` implementatsiyalang. Dot product alohida `dot()` metod.
3. **Method disambiguation:** Ikki trait yarating (`Worker`, `Volunteer`) — har birida `report(&self) -> String` metodi. `Person` strukturasi ikkalasini implement qilsin va to'g'ridan-to'g'ri `Person::report` ham bo'lsin. Uch xil chaqiriqni yozing.
4. **Fully qualified:** `Default` trait'ini `Vec<i32>` uchun customise qiling — `<Vec<i32> as Default>::default()` qanday yoziladi?
5. **Supertrait:** `Json` trait yarating — `to_json(&self) -> String` metod bilan; `Debug + Clone` supertraits qo'shing va default implementatsiyada ulardan foydalaning.
6. **Newtype:** `Email(String)` newtype yarating — `new(s: String)` validatsiya qilsin (`@` borligini), `as_str()` orqali `&str` qaytarsin. `Display`'ni implement qiling.

## Questions Raised

- Generic associated types (GAT) qachon kerak (`type Item<'a>;`)?
- `Add<Rhs>` operatori `Rhs = &Self` uchun avtomatik kelmaydi — nima uchun?
- Newtype Deref bilan ishlatish qachon noto'g'ri?
- Fully qualified syntax associated const'lar uchun ham ishlaydimi?
- Supertrait bilan trait bound farqi kompilyator nuqtai-nazaridan qanday?

## Links To Update

- [[index|Rust Wiki Index]] — yangi sourcelar va concept'lar
- [[overview]] — 20.2 synthesis
- [[wiki/chapters/20-2-advanced-traits|Chapter 20.2]]
- [[traits]] — associated types, default generic params havolalari
- [[orphan-rule|orphan rule]] — newtype pattern havolasi
- [[iterators]] — associated type kontekstida
- [[generics]] — default parameters
