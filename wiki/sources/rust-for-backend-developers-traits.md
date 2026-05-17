---
title: "Трэйты - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, traits]
source_count: 1
---

# Трэйты - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/27. Трэйты.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/traits.html

## Detailed Summary

Bu source `trait`ni type bilan interaction interface'i sifatida beradi: trait qaysi methods mavjud bo'lishi kerakligini belgilaydi, `impl Trait for Type` esa shu contractni konkret type uchun bajaradi. Source interface/abstract class analogiyalarini keltiradi, lekin wiki uchun muhim farq shuki: Rust traits inheritance modelining o'rnini to'liq bosmaydi; ular shared behavior va polymorphism contractini beradi.

Source traitlarning asosiy amaliy maqsadi polymorphism ekanini to'g'ri markazga qo'yadi. U ikki yo'lni ajratadi: static dispatch va dynamic dispatch. Static dispatch tarafida function parameter `&impl Trait` ko'rinishida yoziladi va compiler har concrete type uchun alohida specialized variant yaratadi. Source bunu monomorphization deb nomlaydi va bu yerda asosiy durable nuqta: chaqiriladigan method manzili compile time'da ma'lum bo'ladi.

Dynamic dispatch tarafida parameter `&dyn Trait` bo'ladi. Source bu yerda trait object modelini foydali beginner shaklda tushuntiradi: `&dyn Trait` oddiy reference emas, value pointeri va vtable pointeridan iborat fat pointer. Method chaqiruvi runtime'da vtable orqali topiladi. Wiki uchun saqlanadigan asosiy farq shu: `impl Trait` compile-time specialization, `dyn Trait` runtime indirection.

`impl Trait` va `dyn Trait` return position qismi source'da yaxshi ko'rsatilgan. `impl Trait` return qiladigan function bitta concrete type qaytarishi kerak; branchlar har xil concrete type qaytarsa bu model sinadi. Shunday holatda `Box<dyn Trait>` ishlatiladi. Source `Box`ni heap allocation va owning pointer sifatida qisqacha kiritadi; wiki bu yerdagi sababni aniq saqlaydi: function ichida yaratilgan local value'ga reference qaytarib bo'lmaydi, shuning uchun ownership heapga ko'chirilgan object orqali qaytariladi.

Orphan rule bo'limi traits coherence qoidasi uchun muhim: trait yoki type'dan kamida bittasi current crate'ga tegishli bo'lishi kerak. Local traitni external type uchun, yoki external traitni local type uchun implement qilish mumkin; external trait + external type kombinatsiyasi esa taqiqlanadi. Source keyinroq `newtype pattern` bilan workaround bo'lishini aytadi.

Default method implementations qismi trait contract ichida shared behavior saqlashni ko'rsatadi. Implementor default body'ni ishlatishi ham, override qilishi ham mumkin. Supertrait bo'limi `trait A: B` syntaxisini "trait inheritance" deb tanishtiradi, lekin wiki semantikani aniqroq saqlaydi: bu behavior reuse emas, talablar zanjiri; `A`ni implement qiluvchi type `B`ni ham implement qilishi shart.

Multiple trait bounds qismi `impl A + B` bilan constraint qo'shishni ko'rsatadi. Wiki uchun kerakli note shuki: bu syntax ishlaydi, lekin generic/whereda relationshiplarni ifodalash kengroq va ko'proq idiomatic.

`Self` bo'limi trait definition ichida implementing concrete type nomini yozmasdan qaytish yoki argument type'ini ifodalash uchun kerakligini ko'rsatadi. Bu yerda `Self` va `self` farqini ajratish muhim: biri type, biri value receiver.

Trait object requirements bo'limi source'da soddalashtirilgan ko'rinishda berilgan. Practical takeaway to'g'ri: hamma traitlardan `dyn Trait` yasab bo'lmaydi. `Self`ni qaytaradigan yoki qabul qiladigan methods, generic methods, va receiver'siz static methods trait objectga to'siq bo'lishi mumkin. Lekin source associated typesni ham umumiy taqiq sifatida beradi; wiki buni beginner simplification deb belgilaydi, chunki amalda ba'zi associated-typeli traitlar ham `dyn` bilan ishlashi mumkin, agar kerakli concrete bog'lanishlar aniq ko'rsatilsa.

Oxirgi muhim qism `unsafe trait`. Wiki uchun asosiy barqaror nuqta: `unsafe trait` method body ichida albatta `unsafe` bo'ladi degani emas. Asosiy ma'no shuki, implementor compiler tekshira olmaydigan safety invariantlarini o'z zimmasiga oladi. `Send` va `Sync` shuning klassik misoli.

## Key Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[impl-trait|impl Trait]]
- [[static-dispatch|static dispatch]]
- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[polymorphism]]
- [[monomorphization]]
- [[orphan-rule|orphan rule]]
- [[newtype-pattern|newtype pattern]]
- [[default-trait-implementations|default trait implementations]]
- [[supertraits]]
- [[trait-bounds|trait bounds]]
- [[self-type|Self type]]
- [[box-t|Box<T>]]
- [[sized-trait|Sized trait]]
- [[dyn-compatibility|dyn compatibility]]
- [[unsafe-trait|unsafe trait]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]

## Code Examples

```rust
trait CanIntroduce {
    fn introduce(&self) -> String;
}

impl CanIntroduce for Person {
    fn introduce(&self) -> String {
        format!("Hello, I'm {}", self.name)
    }
}
```

```rust
fn print_introduction(v: &impl CanIntroduce) {
    println!("{}", v.introduce());
}
```

```rust
fn print_introduction(v: &dyn CanIntroduce) {
    println!("{}", v.introduce());
}
```

```rust
fn make_someone(is_person: bool) -> Box<dyn CanIntroduce> {
    if is_person {
        Box::new(Person { name: String::from("John") })
    } else {
        Box::new(Dog { name: String::from("Bark") })
    }
}
```

## Exercises or Practice Ideas

- `trait Describe` yozib, `Person` va `ServiceConfig` uchun implement qiling.
- Bitta functionni `&impl Trait` va `&dyn Trait` bilan ikki variantda yozib, qaysi tradeoff borligini yozib chiqing.
- `impl Trait` return qiluvchi function yozib, keyin ikki xil concrete type qaytarishga urining va nega `Box<dyn Trait>` kerakligini izohlang.

## Questions Raised

- Qachon static dispatch aniq afzal, qachon dynamic dispatch kerak bo'ladi?
- Trait object cheklovlarini yodlashdan ko'ra qanday mental model bilan tushunsa bo'ladi?
- `unsafe trait` uchun "xavf" caller tomonidami, implementor tomonidami?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-traits]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[traits]]
- [[trait-object|trait object]]
- [[unsafe-trait|unsafe trait]]
