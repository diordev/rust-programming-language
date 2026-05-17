---
title: "Rust for Backend Developers: 2. Base"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-17
tags: [rust, backend, chapter, base]
source_count: 23
---

# Rust for Backend Developers: 2. Base

## Learning Goal

`Rust for Backend Developers` kitobining `2. base` sectionini bir foundation layer sifatida tushunish: bindings, primitive types, strings, formatting, expression-oriented control flow, functions, tuples, structs, modules, traits, derive semantics, destructuring, pattern matching, ownership, borrowing, lifetimes, references, arrays, vectors, slices, declarative macros, va raw pointers.

## Main Ideas

- Rust syntaxining bazasi immutability-first binding modelidan boshlanadi: `let`, `mut`, `const`, `static`, raw identifiers, va discarded bindings.
- Primitive types qatlami scalar types, `()`, `!`, va explicit `as` casting orqali qat'iy type systemni ko'rsatadi.
- Strings bobi `&str` va `String` boundarysini, UTF-8 text modeli va `format!`/append workflows bilan birga ochadi.
- `println!` formatting modeli `Display` va `Debug` orasidagi boundaryni juda erta ko'rsatadi.
- `if` va loop materiallari Rust control flow'ni expression-oriented ko'rinishda beradi: `if` value qaytaradi, `loop` esa `break value` bilan natija beradi.
- Scope faqat lifetime emas; u block expression value'si va semicolon semanticsini ham belgilaydi.
- Functions bobi typed signatures, early return, `const fn`, nested helper, va `!` bilan bog'langan control transferni birga ko'rsatadi.
- Tuples arraydan farqli ravishda heterogeneous grouping beradi va slice parameter bilan multiple return values patternini mustahkamlaydi.
- Structs bobi domain data'ni named fields bilan yig'adi: field init shorthand, struct update syntax, `impl`, methods, tuple structs, unit-like structs, va reference field bo'lsa lifetime annotation.
- Modules bobi syntax darajasidagi foundation'ni code organizationga ulaydi: `mod`, `pub`, `use`, file layout, va `crate` compile modelining beginner mental modeli shu yerda keladi.
- Traits bobi behavior abstractionni qo'shadi: `impl Trait` orqali static dispatch, `dyn Trait` orqali trait object + dynamic dispatch, orphan rule, default impl, supertraits, `Self`, va `unsafe trait`.
- Auto-derive bobi trait ecosystemni everyday data modelga tushiradi: `#[derive(...)]` [[attribute]] sifatida mechanical impl generatsiya qiladi, [[partial-eq|PartialEq]], [[clone]], va [[copy-trait|Copy trait]] esa equality/copy semanticsni aniq ajratadi.
- Destructuring bobi pattern syntaxni control flow'dan oldin tayyorlaydi: tuple, array, struct, va function parametrlari value shape'ini ochib beradi; fixed-size [[array-destructuring|array destructuring]] bilan runtime-size slice o'rtasidagi chegara shu yerda ko'rinadi.
- Pattern matching bobi esa `match`ni syntax emas, semantics darajasida yakunlaydi: range, [[at-binding|@ binding]], [[match-guard|match guard]], [[slice-patterns|slice patterns]], struct patterns, va [[ref-pattern|ref pattern]] orqali branch + binding bir joyga tushadi.
- Ownership bobi Rustning markaziy compile-time resource modelini front-stage'ga olib chiqadi: move, deterministic cleanup, borrowing, va referential safety bitta chiziqqa tushadi.
- Lifetimes bobi relation-level annotationlarni qo'shadi: reference umrini cho'zmaydi, lekin qaysi borrow qaysi natija bilan bog'langanini compilerga ko'rsatadi.
- Declarative macro bobi compile-time code expansionni ochadi: `macro_rules!`, fragment specifierlar, repetition, va `vec![]` nega function emasligi.
- Pointer bobi safe reference modelidan tashqariga chiqadi: `*const T`, `*mut T`, `&raw`, dereference, `unsafe`, va raw pointerlar bilan invariantlar endi dasturchi zimmasiga o'tishi.
- References, arrays, vectors, va slices birga o'qilganda contiguous memory + borrowing + mutability mental modeli shakllanadi.
- `Vec<T>` va slice materiallari keyingi ownership/borrowing chapterlarini ancha yengillashtiradi.

## Concepts

- [[variables-and-mutability|variables and mutability]]
- [[constants]]
- [[static-items]]
- [[raw-identifiers]]
- [[discarded-binding]]
- [[scalar-types|scalar types]]
- [[unit-type|unit type]]
- [[never-type|never type (!)]]
- [[type-casting]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[string-literal]]
- [[utf-8|UTF-8]]
- [[format-macro|format! macro]]
- [[println-macro|println! macro]]
- [[display-formatting|Display formatting]]
- [[debug-formatting|Debug formatting]]
- [[if-expressions|if expressions]]
- [[while-loop|while loop]]
- [[loop]]
- [[for-loop|for loop]]
- [[range]]
- [[break]]
- [[continue]]
- [[functions]]
- [[tuple]]
- [[pattern-destructuring|pattern destructuring]]
- [[structs]]
- [[struct-fields|struct fields]]
- [[struct-instances|struct instances]]
- [[field-init-shorthand]]
- [[struct-update-syntax]]
- [[methods]]
- [[impl-block|impl block]]
- [[associated-functions]]
- [[tuple-structs]]
- [[unit-like-structs]]
- [[module-system|module system]]
- [[module]]
- [[pub-keyword|pub keyword]]
- [[use-declarations|use declarations]]
- [[mod-declarations|mod declarations]]
- [[module-file-layout|module file layout]]
- [[crate]]
- [[crate-root|crate root]]
- [[traits]]
- [[trait-definitions|trait definitions]]
- [[trait-implementations|trait implementations]]
- [[attribute]]
- [[derive-attribute|derive attribute]]
- [[partial-eq|PartialEq]]
- [[clone]]
- [[copy-trait|Copy trait]]
- [[marker-trait|marker trait]]
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
- [[ownership]]
- [[move-semantics|move semantics]]
- [[drop]]
- [[borrow-checker]]
- [[lifetimes]]
- [[static-lifetime]]
- [[dangling-reference|dangling reference]]
- [[macro]]
- [[declarative-macros|declarative macros]]
- [[procedural-macros|procedural macros]]
- [[raw-pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[dereference-operator|dereference operator]]
- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]]
- [[miri|Miri]]
- [[scope]]
- [[statements-and-expressions|statements and expressions]]
- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[array]]
- [[array-destructuring|array destructuring]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[slices]]
- [[match]]
- [[pattern-matching|pattern matching]]
- [[exhaustive-matching|exhaustive matching]]
- [[catch-all-patterns|catch-all patterns]]
- [[or-pattern|or-pattern (`|`)]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]

## Examples

```rust
let value = {
    let x = 1;
    x + 2
};
```

```rust
let mut v = vec![1, 2, 3];
let window: &mut [i32] = &mut v[1..3];
window[0] = 9;
println!("{v:?}");
```

```rust
let label = format!("worker-{}", 7);
let result = loop {
    if label.len() > 0 {
        break label;
    }
};
```

```rust
macro_rules! sum_nums {
    ( $( $rest:expr ),* ) => { 0 $( + $rest )* }
}
```

```rust
struct Person {
    first_name: String,
    last_name: String,
}

impl Person {
    fn full_name(&self) -> String {
        format!("{} {}", self.first_name, self.last_name)
    }
}
```

```rust
mod a { pub fn get_num() -> i32 { 1 } }
mod b { pub fn get_num() -> i32 { 2 } }
```

```rust
trait CanIntroduce {
    fn introduce(&self) -> String;
}

fn print_introduction(v: &impl CanIntroduce) {
    println!("{}", v.introduce());
}
```

```rust
#[derive(Debug, PartialEq, Clone, Copy)]
struct Point2D {
    x: i32,
    y: i32,
}
```

```rust
let packet = [1, 2, 3, 4, 5];
let [head, second, rest @ ..] = packet;

match packet.as_slice() {
    [a, b, ..] if a < b => println!("{a} < {b}"),
    _ => println!("other shape"),
}
```

## Exercises

- `const`, `static`, `let`, va `let mut` uchun bitta domain example ishlab chiqing.
- `&str`, `String`, `format!`, va `push_str`ni bitta text-building scenario ichida solishtiring.
- `[u8; 4]`, `Vec<u8>`, va `&[u8]`ni bitta networking scenario ichida qaysi joyda ishlatishni ajrating.
- `if` expression, `loop { break value; }`, va tuple return ishlatadigan kichik utility function yozing.
- Borrowing bilan ownership transferni oldini oladigan kichik API yozing.
- `take_longest<'a>`ga o'xshash signature yozib, qaysi inputga output bog'langanini izohlang.
- `macro_rules!` bilan `ident` va `expr` fragmentli ikkita macro yozing.
- `&raw const` bilan pointer olib, dereference qayerda unsafe bo'lishini ko'rsating.
- `User` struct, `new` associated function, va `rename(&mut self, ...)` method yozing.
- `mod`, `pub`, va `use`ni birga ishlatadigan ikki file'li mini example yozing.
- Default implementationli trait yozib, bitta type uchun override qiling.
- `impl Trait` va `Box<dyn Trait>` bilan bir xil API'ning ikki variantini yozing.
- `Display` va `Debug` formattingni bir xil qiymat uchun ikki xil chiqish bilan ko'rsating.
- `Point2D` yoki `UserId` uchun qaysi traitlarni derive qilish mantiqli ekanini tanlab, nega ekanini yozing.
- Fixed-length array destructuring va `match`dagi slice patternni bir xil data ustida solishtiring.
- `match`da `@` binding, guard, va `ref mut`ni birlashtiradigan kichik parser yoki state updater yozing.

## Review Questions

- Nega `2. base` sectioni variablesdan boshlab slicesgacha uzluksiz bitta modelga aylanadi?
- `&str` va `String` orasidagi boundary ownership bilan qanday bog'langan?
- `if`, `loop`, va function final expression bir xil expression modeliga qanday ulanadi?
- Nega `for item in arr` va `for item in &arr` ownership jihatdan boshqa-boshqa natija beradi?
- `Vec<T>`ning reallocation modeli borrowing qoidalarini qanday izohlaydi?
- `vec![]` nega declarative macro bo'lib, oddiy function emas?
- Pointer yaratish va pointer dereference orasidagi safety boundary qayerda?
- `pub struct` va `pub field` orasidagi API farqi nima?
- `mod` va `use` nega bir-birining o'rniga ishlamaydi?
- `impl Trait` va `dyn Trait` qachon boshqa-boshqa model beradi?
- Nega bare `dyn Trait` parameter yoki return sifatida ishlamaydi?
- Trait object bo'lishiga qaysi method shakllari to'siq bo'ladi?
- Block expression, `()`, va `!` bir-biri bilan qanday bog'langan?
- `derive(PartialEq)` bilan qo'lda `impl PartialEq` orasida qachon semantik farq paydo bo'ladi?
- Nega oddiy destructuringda slice ishlamaydi, lekin `match`da slice pattern ishlaydi?
- `ref pattern` bilan `match &value` orasida qaysi tradeoff bor?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-variables]]
- [[wiki/sources/rust-for-backend-developers-primitive-types]]
- [[wiki/sources/rust-for-backend-developers-console-output]]
- [[wiki/sources/rust-for-backend-developers-scopes]]
- [[wiki/sources/rust-for-backend-developers-references]]
- [[wiki/sources/rust-for-backend-developers-arrays]]
- [[wiki/sources/rust-for-backend-developers-vector]]
- [[wiki/sources/rust-for-backend-developers-slices]]
- [[wiki/sources/rust-for-backend-developers-strings]]
- [[wiki/sources/rust-for-backend-developers-if]]
- [[wiki/sources/rust-for-backend-developers-loops]]
- [[wiki/sources/rust-for-backend-developers-functions]]
- [[wiki/sources/rust-for-backend-developers-tuples]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
- [[wiki/sources/rust-for-backend-developers-lifetimes]]
- [[wiki/sources/rust-for-backend-developers-declarative-macros]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
- [[wiki/sources/rust-for-backend-developers-structs]]
- [[wiki/sources/rust-for-backend-developers-modules]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-auto-derive-traits]]
- [[wiki/sources/rust-for-backend-developers-destructuring]]
- [[wiki/sources/rust-for-backend-developers-pattern-matching]]
