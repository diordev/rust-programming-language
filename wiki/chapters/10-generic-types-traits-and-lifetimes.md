---
title: "10. Generic Types, Traits, and Lifetimes"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, rust-book, chapter, generics, traits, lifetimes]
source_count: 3
---

# 10. Generic Types, Traits, and Lifetimes

## Learning Goal

Rustda duplicationni kamaytirish va abstraction yaratish uchun [[generics]], [[traits]], va [[lifetimes]] qanday ishlashini tushunish: avval duplicated code'ni [[function-extraction|function extraction]] bilan kamaytirish, keyin shu mental modelni generic types, trait constraints, va lifetime relationshipsga o'tkazish.

## Main Ideas

- Generics concrete type yoki property o'rnida abstract placeholder beradi.
- Generic functions concrete `i32` yoki `String` o'rniga type parameter bilan ishlaydi.
- Oldingi chapterlarda generic standard library typelar allaqachon ishlatilgan: [[option|Option<T>]], [[vector|Vec<T>]], [[hash-map|HashMap<K, V>]], va [[result|Result<T, E>]].
- Chapter 10 o'zimiz generic functions, structs, enums, va methods define qilishni ko'rsatadi.
- [[traits]] generic type qaysi behaviorga ega bo'lishi kerakligini constraint sifatida belgilaydi.
- [[lifetimes]] references relationshipni compilerga aytadigan generics turi sifatida frame qilinadi.
- [[code-duplication|Code duplication]] tedious va error-prone; logic o'zgarsa har copy joyini update qilish kerak.
- Duplicate code'ni functionga extract qilish abstraction yaratadi.
- `largest(list: &[i32]) -> &i32` borrowed slice qabul qilib, largest elementga reference qaytaradi.
- Function extraction steps: duplicate code'ni topish, inputs/outputsni signature'ga chiqarish, duplicate sitesni function callga almashtirish.
- Function body concrete values o'rniga abstract parameter ustida ishlaganidek, generics concrete types o'rniga abstract type parameters ustida ishlaydi.
- 10.1 generic syntaxni function, struct, enum, va method definitions uchun ochadi.
- [[generic-functions|Generic functions]] type parameterni function nomidan keyin `<>` ichida e'lon qiladi: `fn largest<T>(list: &[T]) -> &T`.
- Generic function body ishlatadigan behavior signature'da ko'rinishi kerak; `>` operation uchun `T: std::cmp::PartialOrd` constraint kerak.
- [[generic-structs|Generic structs]] bir yoki bir nechta type parameter bilan field typelarini umumlashtiradi: `Point<T>` bir xil field type, `Point<T, U>` turli field typelar.
- [[generic-enums|Generic enums]] `Option<T>` va `Result<T, E>` kabi reusable data shape beradi.
- [[generic-methods|Generic methods]] `impl<T> Point<T>`, concrete `impl Point<f32>`, va method-level generics (`fn mixup<X2, Y2>`) orqali yoziladi.
- [[monomorphization]] generic code'ni compile time'da concrete type versionsga aylantiradi, shuning uchun generics runtime cost keltirmaydi.
- 10.2 [[traits]]ni shared behavior contracti sifatida chuqurlashtiradi.
- [[trait-definitions|Trait definitions]] method signaturesni contract qiladi; [[trait-implementations|trait implementations]] `impl Trait for Type` orqali concrete behavior beradi.
- [[orphan-rule|Orphan rule]] trait yoki type'dan kamida bittasi local bo'lmasa implementation yozishni taqiqlaydi.
- [[default-trait-implementations|Default trait implementations]] common behaviorni trait ichida beradi va implementor override qilishi mumkin.
- [[impl-trait|impl Trait]] parameter yoki return type'da trait implement qilgan type'ni concise ifodalaydi.
- [[trait-bounds|Trait bounds]] `T: Summary`, `T: Summary + Display`, va [[where-clauses|where clauses]] orqali generic type capabilitiesni cheklaydi.
- Return-position `impl Trait` bitta concrete return type talab qiladi.
- [[conditional-method-implementations|Conditional method implementations]] method availability'ni type parameter trait bounds bilan bog'laydi.
- [[blanket-implementations|Blanket implementations]] traitni boundga mos barcha typelar uchun implement qiladi, masalan `Display` -> `ToString`.

## Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[partial-ord|PartialOrd]]
- [[monomorphization]]
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
- [[lifetimes]]
- [[code-duplication|code duplication]]
- [[function-extraction|function extraction]]
- [[functions]]
- [[slices]]
- [[abstraction]]
- [[zero-cost-abstractions|zero-cost abstractions]]
- [[option|Option<T>]]
- [[vector|Vec<T>]]
- [[hash-map|HashMap<K, V>]]
- [[result|Result<T, E>]]
- [[e0369-binary-operation-cannot-be-applied|E0369 binary operation cannot be applied]]
- [[e0308-mismatched-types|E0308 mismatched types]]

## Examples

Extracted largest function:

```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

Using it with two lists:

```rust
let number_list = vec![34, 50, 25, 100, 65];
let result = largest(&number_list);
assert_eq!(*result, 100);

let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];
let result = largest(&number_list);
assert_eq!(*result, 6000);
```

Generic type examples already seen:

```rust
Option<T>
Vec<T>
HashMap<K, V>
Result<T, E>
```

Generic largest function with a trait bound:

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

Generic `Point` structs:

```rust
struct Point<T> {
    x: T,
    y: T,
}

struct MixedPoint<T, U> {
    x: T,
    y: U,
}
```

Generic methods:

```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

Trait definition and implementation:

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

impl Summary for SocialPost {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

Trait as parameter:

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
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

## Exercises

- Listing 10-2 style duplicated largest code yozing, keyin uni `largest` functioniga extract qiling.
- `largest` signature'idagi `&[i32]` va `&i32` ownership/borrowing ma'nosini tushuntiring.
- `largest` functionini empty slice bilan chaqirish nima muammo tug'dirishini oldindan tahlil qiling.
- `Option<T>`, `Vec<T>`, `HashMap<K,V>`, `Result<T,E>` uchun generic parameterlar ro'yxatini yozing.
- `largest_i32` va `largest_char` functions orasidagi duplicationni generic function bilan qanday kamaytirish mumkinligini taxmin qiling.
- `largest<T>`ni `T: std::cmp::PartialOrd` bilan tuzating va nima uchun kerakligini tushuntiring.
- `Point<T>` va `Point<T, U>` orasidagi type inference farqini misollar bilan yozing.
- `impl<T> Point<T>` va `impl Point<f32>` method availability farqini tekshiring.
- `Option<i32>` va `Option<f64>` monomorphization mental modelini chizing.
- `Summary` traitini define qilib, ikki custom type uchun implement qiling.
- `notify(item: &impl Summary)` va `notify<T: Summary>(item: &T)` orasidagi difference'ni ikki parameter bilan tekshiring.
- `T: Summary + Display` bound va equivalent `where` clause yozing.
- Orphan rule bo'yicha allowed/not allowed trait implementation examples tuzing.
- `Pair<T>` uchun `cmp_display` methodini faqat `T: Display + PartialOrd` bo'lganda mavjud qiling.

## Review Questions

- Generics duplicationni qanday kamaytiradi?
- Function extraction va generic abstraction orasidagi o'xshashlik nima?
- Traitlar generic typelarni qanday cheklaydi?
- Lifetimes nega genericsning bir turi sifatida qaraladi?
- `largest(list: &[i32]) -> &i32` nima uchun `Vec<i32>`ni ownership bilan olmaydi?
- `fn largest<T>(list: &[T]) -> &T` nima uchun `>` ishlatilganda compile bo'lmaydi?
- `Point<T>`da `x` va `y` nima uchun bir xil type bo'lishi kerak?
- `impl<T> Point<T>`dagi `T` nima uchun `impl`dan keyin e'lon qilinadi?
- Monomorphization generics performance'ini qanday tushuntiradi?
- Trait definition va trait implementation orasidagi farq nima?
- Orphan rule qanday conflictni oldini oladi?
- `&impl Summary` bilan `T: Summary` qachon bir xil, qachon farqli?
- Return-position `impl Trait` nima uchun har branchda turli concrete type qaytarishga ruxsat bermaydi?
- Blanket implementation nimani umumlashtiradi?

## Related Pages

- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-1-generic-data-types-the-rust-programming-language]]
- [[10-2-defining-shared-behavior-with-traits-the-rust-programming-language]]
- [[generics]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-enums|generic enums]]
- [[generic-methods|generic methods]]
- [[monomorphization]]
- [[partial-ord|PartialOrd]]
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
- [[lifetimes]]
- [[largest-function-extraction|largest function extraction]]
- [[generic-largest-function|generic largest function]]
- [[generic-point|generic Point]]
- [[summary-trait-example|Summary trait example]]
- [[notify-trait-parameters|notify trait parameters]]
- [[pair-conditional-methods|Pair conditional methods]]
