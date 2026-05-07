---
title: "10.2. Defining Shared Behavior with Traits - The Rust Programming Language"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, source, traits]
source_count: 1
source_path: "raw/books/the_rust_programming_language/10.2. Defining Shared Behavior with Traits - The Rust Programming Language.md"
source_url: "https://doc.rust-lang.org/stable/book/ch10-02-traits.html"
---

# 10.2. Defining Shared Behavior with Traits - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/10.2. Defining Shared Behavior with Traits - The Rust Programming Language.md`
- Type: book chapter section
- URL: https://doc.rust-lang.org/stable/book/ch10-02-traits.html
- Date processed: 2026-05-07

## Detailed Summary

Chapter 10.2 [[traits]]ni shared behavior contracti sifatida chuqurlashtiradi. Trait type qaysi methods yoki functionalityga ega ekanini abstract shaklda belgilaydi. [[trait-bounds|Trait bounds]] esa generic type "istalgan type" emas, muayyan behaviorga ega type bo'lishi kerakligini compilerga aytadi. Source traits boshqa tillardagi interfacesga o'xshashligini, lekin Rustda o'ziga xos qoidalar borligini eslatadi.

[[trait-definitions|Trait definition]] method signaturesni bir joyga group qiladi. `pub trait Summary { fn summarize(&self) -> String; }` `Summary` traitini e'lon qiladi va `summarize` method signature'ini contract sifatida belgilaydi. Method body yo'q bo'lsa, semicolon bilan tugaydi; traitni implement qilgan har type shu methodni aynan shu signature bilan berishi kerak. Trait ichida bir nechta method signatures bo'lishi mumkin.

[[trait-implementations|Trait implementation]] regular `impl` blockga o'xshaydi, lekin `impl TraitName for TypeName` shaklida yoziladi. Aggregator example'da `NewsArticle` va `SocialPost` typelari `Summary` traitini implement qiladi. `NewsArticle::summarize` headline, author, locationdan summary yasaydi; `SocialPost::summarize` username va contentni birlashtiradi. Trait methodlari regular methods kabi chaqiriladi, lekin external crate trait methoddan foydalanishi uchun traitni scope'ga ham olib kirishi kerak: `use aggregator::{SocialPost, Summary};`.

Trait implementationda muhim restriction bor: [[orphan-rule|orphan rule]]. Crate faqat trait yoki type'dan kamida bittasi local bo'lsa implementation yoza oladi. Masalan aggregator crate `Display`ni local `SocialPost` uchun implement qila oladi, yoki local `Summary`ni `Vec<T>` uchun implement qila oladi. Lekin standard librarydagi external `Display` traitini standard librarydagi external `Vec<T>` type'i uchun implement qila olmaydi. Bu coherence qoidasini saqlaydi: ikki crate bir xil trait/type juftligi uchun raqobat qiluvchi implementation berib, code'ni buzib qo'ymasligi kerak.

[[default-trait-implementations|Default trait implementations]] trait methodiga body berishga imkon beradi. `Summary` traitida `summarize` default `(Read more...)` qaytarsa, `impl Summary for NewsArticle {}` empty block bilan default behaviorni oladi. `SocialPost` esa o'z `summarize` methodini yozib defaultni override qiladi. Default methodlar shu traitdagi boshqa required methodlarni chaqirishi ham mumkin: `summarize` default body `summarize_author`ni chaqiradi, implementor esa faqat `summarize_author`ni beradi. Lekin overriding method ichidan shu methodning default implementationini chaqirish mumkin emas.

Traits function parameters sifatida ishlatilganda [[impl-trait|impl Trait]] syntax qulay: `pub fn notify(item: &impl Summary)` har qanday `Summary` implement qilgan type'ni qabul qiladi. Function body `item.summarize()`ni chaqira oladi, chunki `Summary` contracti buni kafolatlaydi. `String` yoki `i32` kabi `Summary` implement qilmagan type bilan chaqirish compile bo'lmaydi.

`impl Trait` oddiy holatlar uchun shorthand, lekin to'liq generic [[trait-bounds|trait bound]] syntax `pub fn notify<T: Summary>(item: &T)` bo'ladi. Farq ikki parameterda ko'rinadi: `fn notify(item1: &impl Summary, item2: &impl Summary)` ikki parameter turli concrete type bo'lishiga ruxsat beradi; `fn notify<T: Summary>(item1: &T, item2: &T)` esa ikkalasi bir xil concrete type bo'lishini talab qiladi.

Multiple trait bounds `+` bilan yoziladi: `&(impl Summary + Display)` yoki `T: Summary + Display`. Bu function body'ga ham `summarize`, ham `{}` display formatting ishlatish huquqini beradi. Bounds ko'payib signature o'qilishi qiyinlashsa, [[where-clauses|where clauses]] ishlatiladi: `fn some_function<T, U>(...) -> i32 where T: Display + Clone, U: Clone + Debug`.

Return positionda ham `impl Trait` ishlatish mumkin: `fn returns_summarizable() -> impl Summary` concrete return type nomini yashiradi, lekin callerga return value `Summary` behavioriga ega ekanini bildiradi. Bu closures va iterators kabi uzun yoki compiler-only typelar bilan ayniqsa foydali. Cheklov: `impl Trait` return positionda bitta concrete type qaytarishi kerak. Bir branch `NewsArticle`, boshqa branch `SocialPost` qaytarsa, `-> impl Summary` ishlamaydi; bunday behavior keyinroq trait objects bilan ko'riladi.

[[conditional-method-implementations|Conditional method implementations]] generic type uchun methodni faqat type parameter ma'lum traitsni implement qilganda berishga imkon beradi. `Pair<T>` har doim `new` methodiga ega, lekin `cmp_display` faqat `T: Display + PartialOrd` bo'lganda mavjud. Bu method body `>=` comparison va `{}` display formattingdan foydalanadi.

[[blanket-implementations|Blanket implementations]] traitni trait boundga mos keladigan barcha typelar uchun implement qiladi. Standard library `Display` implement qilgan har qanday `T` uchun `ToString`ni implement qiladi: `impl<T: Display> ToString for T`. Shu sabab `3.to_string()` ishlaydi. Bunday implementations trait documentationdagi "Implementors" sectionida ko'rinadi.

Section yakunida traits va trait bounds Rust generic code'ni ikki tomondan kuchaytirishi ta'kidlanadi: duplication kamayadi, lekin compiler generic type kerakli behaviorga ega ekanini compile time'da tekshiradi. Dynamically typed languagesda method yo'qligi runtime error bo'lishi mumkin; Rustda bu xatolar compile time'da ushlanadi va runtime behavior checks yozish shart bo'lmaydi.

## Key Concepts

- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[default-trait-implementations|default trait implementations]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[where-clauses|where clauses]]
- [[orphan-rule|orphan rule]]
- [[blanket-implementations|blanket implementations]]
- [[conditional-method-implementations|conditional method implementations]]
- [[generics]]
- [[generic-functions|generic functions]]
- [[generic-methods|generic methods]]
- [[display-formatting|Display]]
- [[partial-ord|PartialOrd]]
- [[public-api|public API]]
- [[impl-block|impl block]]

## Code Examples

Defining a trait:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Implementing a trait:

```rust
impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
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

Trait bound syntax:

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

Multiple bounds and `where`:

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
    unimplemented!()
}
```

Conditional method implementation:

```rust
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

## Exercises or Practice Ideas

- `Summary` traitini bitta required method bilan define qiling va ikki custom struct uchun implement qiling.
- `Summary`ga default `summarize` implementation qo'shing, keyin bitta type defaultni ishlatsin, boshqasi override qilsin.
- `notify(item: &impl Summary)` va `notify<T: Summary>(item: &T)` orasidagi farqni ikki parameterli functionda tekshiring.
- `Summary + Display` bound ishlatadigan function yozing va qaysi methods/body operations ruxsat bo'lishini belgilang.
- `where` clause bilan uzun signature'ni qayta yozing.
- `Pair<T>` uchun `cmp_display` methodini faqat `T: Display + PartialOrd` bo'lganda mavjud qiling.
- Orphan rule bo'yicha qaysi impl'lar allowed yoki not allowed ekanini jadvalga ajrating.

## Questions Raised

- Return position `impl Trait` nima uchun bitta concrete type bilan cheklangan?
- Trait objects Chapter 18da `impl Trait` return limitationini qanday hal qiladi?
- Orphan rule public API design va extension traits patterniga qanday ta'sir qiladi?
- Blanket implementations overlap qilsa Rust qanday conflictni ushlaydi?

## Links To Update

- [[10-generic-types-traits-and-lifetimes|10. Generic Types, Traits, and Lifetimes]]
- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[default-trait-implementations|default trait implementations]]
- [[trait-bounds|trait bounds]]
- [[impl-trait|impl Trait]]
- [[where-clauses|where clauses]]
- [[orphan-rule|orphan rule]]
- [[blanket-implementations|blanket implementations]]
- [[conditional-method-implementations|conditional method implementations]]
- [[summary-trait-example|Summary trait example]]
- [[notify-trait-parameters|notify trait parameters]]
- [[pair-conditional-methods|Pair conditional methods]]
