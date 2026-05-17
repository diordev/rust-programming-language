---
title: "Traits"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, traits]
source_count: 10
---

# Traits

## Short Definition

`trait` Rustda shared behavior contractini ifodalaydi: type qaysi methods yoki capabilitiesni taqdim etishini belgilaydi.

## Why It Matters

Traits [[generics]] bilan birga ishlaganda code reuse, abstraction, va compile-time checking beradi. [[copy-trait|Copy trait]] ownership mavzusidagi birinchi amaliy misol. [[debug-trait|Debug trait]] esa custom structni developer-facing formatda chiqarish uchun derive qilinadi.

## Mental Model

Trait "bu type mana shu behaviorni bajaradi" degan shartga o'xshaydi. Function generic type qabul qilsa, trait bound orqali undan qanday behavior kutishini aytadi.

Traitning o'zi behavior contract, `impl` esa shu contractni konkret type uchun bajarishdir.

Backend beginner source shu story'ni yanada amaliy frame qiladi: traitning asosiy foydasi polymorphism. Rust bunda ikki boshqa model beradi: [[static-dispatch|static dispatch]] (`impl Trait`, trait bounds, [[monomorphization]]) va [[dynamic-dispatch|dynamic dispatch]] (`dyn Trait`, [[trait-object|trait object]], vtable).

Chapter 9.2da [[from-trait|From trait]] `?` operator error type conversionida tilga olinadi, [[box-dyn-error|Box<dyn Error>]] esa trait object mavzusiga oldindan signal beradi.

Chapter 10 traitsni generic behavior constraints sifatida frame qiladi: generic type har qanday type emas, ma'lum behaviorga ega type bo'lishi kerak bo'lsa, trait bound ishlatiladi.

Chapter 10.1da `largest<T>` example'i `>` operation sabab [[partial-ord|PartialOrd]] trait bound talab qiladi. Compiler [[e0369-binary-operation-cannot-be-applied|E0369]] orqali `T`ni `std::cmp::PartialOrd` bilan restrict qilishni taklif qiladi.

Chapter 10.2 traitsni full shared behavior system sifatida ko'rsatadi: [[trait-definitions|trait definitions]], [[trait-implementations|trait implementations]], [[default-trait-implementations|default implementations]], [[trait-bounds|trait bounds]], [[impl-trait|impl Trait]], [[where-clauses|where clauses]], [[orphan-rule|orphan rule]], [[conditional-method-implementations|conditional methods]], va [[blanket-implementations|blanket implementations]].

Backend beginner generics source traits mavzusiga yana bir qatlam qo'shadi: traitning o'zi ham generic bo'lishi mumkin. Lekin generic trait va [[associated-types|associated types]] orasida dizayn farqi bor. Generic trait bir concrete type uchun bir nechta impl variantiga yo'l ochishi mumkin, associated type esa implementor tanlagan bitta bog'langan type bilan contractni qotiradi.

```rust
trait Summary {
    fn summarize(&self) -> String;
}

struct NewsArticle {
    headline: String,
    author: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{} by {}", self.headline, self.author)
    }
}
```

Bu yerda `NewsArticle` `Summary` traitini implement qilmoqda. Demak `NewsArticle` uchun `summarize()` behavior mavjud.

## Syntax and Examples

```rust
trait Summary {
    fn summarize(&self) -> String;
}
```

Trait bound bilan generic function:

```rust
fn notify(item: &impl Summary) {
    println!("{}", item.summarize());
}
```

Yoki equivalent `where` syntax:

```rust
fn notify<T>(item: &T)
where
    T: Summary,
{
    println!("{}", item.summarize());
}
```

Comparison behavior talab qiladigan generic function:

```rust
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Derived trait example:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

Default implementation:

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}

impl Summary for NewsArticle {}
```

Trait as parameter:

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

Return type with `impl Trait`:

```rust
fn returns_summarizable() -> impl Summary {
    SocialPost {
        username: String::from("horse_ebooks"),
        content: String::from("of course, as you probably already know, people"),
        reply: false,
        repost: false,
    }
}
```

Dynamic dispatch bilan:

```rust
fn print_introduction(v: &dyn CanIntroduce) {
    println!("{}", v.introduce());
}
```

Generic trait:

```rust
trait ValueProducer<T> {
    fn produce() -> T;
}
```

## Common Mistakes

- Traitni inheritance deb tushunish.
- Trait bound bo'lmasa generic function type haqida yetarli ma'lumotga ega bo'ladi deb o'ylash.
- Custom type traitsni avtomatik implement qiladi deb o'ylash.
- `impl Trait for Type` bilan `impl Type`ni chalkashtirish.
- `derive` bilan qo'lda yozilgan implementation bir xil bo'ladi deb o'ylash.
- External traitni external type uchun implement qilish mumkin deb o'ylash.
- Trait method call uchun trait scope'da bo'lishi kerak bo'lgan holatlarni unutish.
- Return-position `impl Trait` har branchda turli concrete type qaytarishi mumkin deb o'ylash.
- Default methodni override qilgan method ichidan default implementationni chaqirish mumkin deb o'ylash.
- Bare `dyn Trait` oddiy value sifatida ishlaydi deb o'ylash; amalda odatda `&dyn Trait` yoki `Box<dyn Trait>` kerak.
- Generic trait va associated type bir xil API tradeoff beradi deb o'ylash.

## Related Concepts

- [[generics]]
- [[impl-block|impl block]]
- [[copy-trait|Copy trait]]
- [[debug-trait|Debug trait]]
- [[derive-attribute|derive attribute]]
- [[display-formatting|Display formatting]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[from-trait|From trait]]
- [[box-dyn-error|Box<dyn Error>]]
- [[question-mark-operator|question mark operator]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[partial-ord|PartialOrd]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[default-trait-implementations|default trait implementations]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[static-dispatch|static dispatch]]
- [[trait-object|trait object]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[polymorphism]]
- [[where-clauses|where clauses]]
- [[orphan-rule|orphan rule]]
- [[blanket-implementations|blanket implementations]]
- [[conditional-method-implementations|conditional method implementations]]
- [[summary-trait-example|Summary trait example]]
- [[notify-trait-parameters|notify trait parameters]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]
- [[associated-types|associated types]] — `type Item;` placeholder
- [[const-generics|const generics]]
- [[default-generic-parameters|default generic parameters]] — `<Rhs = Self>`
- [[operator-overloading|operator overloading]] — `std::ops::Add` va boshqalar
- [[fully-qualified-syntax|fully qualified syntax]] — `<Type as Trait>::fn()`
- [[supertraits]] — trait body'da boshqa trait method'lari
- [[newtype-pattern|newtype pattern]] — orphan rule chetlab o'tish
- [[dyn-compatibility|dyn compatibility]]
- [[unsafe-trait|unsafe trait]] — `unsafe impl Send`

## Sources

- [[wiki/sources/0-2-introduction]]
- [[4-1-what-is-ownership]]
- [[5-2-an-example-program-using-structs]]
- [[9-2-recoverable-errors-with-result]]
- [[wiki/sources/10-generic-types-traits-and-lifetimes]]
- [[10-1-generic-data-types]]
- [[10-2-defining-shared-behavior-with-traits]]
- [[wiki/sources/20-2-advanced-traits|20.2 Advanced Traits]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-generics]]
