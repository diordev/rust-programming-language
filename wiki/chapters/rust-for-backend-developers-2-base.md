---
title: "Rust for Backend Developers: 2. Base"
type: chapter
status: active
created: 2026-05-16
updated: 2026-05-17
tags: [rust, backend, chapter, base]
source_count: 30
---

# Rust for Backend Developers: 2. Base

## Learning Goal

`Rust for Backend Developers` kitobining `2. base` sectionini bir foundation layer sifatida tushunish: bindings, primitive types, strings, formatting, expression-oriented control flow, functions, tuples, structs, modules, traits, generics, enums, `Option`, `Result`, anonymous functions va closures, iterators, smart pointers, ownership, borrowing, lifetimes, references, arrays, vectors, slices, derive semantics, destructuring, pattern matching, declarative macros, va raw pointers.

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
- Anonymous functions bobi behavior'ni value sifatida ko'taradi: non-capturing callable [[function-pointers|function pointer]] bo'lishi mumkin, capturing callable esa [[closures]] bo'ladi; shu yerda [[higher-order-functions|higher-order functions]], [[fn-traits|Fn traits]], returned closure, va `impl Fn`/`Box<dyn Fn>` chegaralari ko'rinadi.
- Generics bobi abstractionni syntax-level convenience emas, compile-time specialization modeli sifatida ko'rsatadi: generic struct/function/method, [[turbofish]], generic traits vs [[associated-types|associated types]], [[trait-bounds|trait bounds]], [[where-clauses|where clauses]], conditional impl, va [[const-generics|const generics]] shu yerda bitta systemga yig'iladi.
- Enums bobi Rustdagi ADT modelini front-stage'ga olib chiqadi: C-like variants, payloadli variants, methods, approximate discriminant mental modeli, va [[if-let|if let]] bilan single-variant branch shu yerda birga ko'rinadi.
- `Option` bobi absence'ni sentinel value yoki null bilan emas, [[option|Option]] bilan ifodalashni normaga aylantiradi: `Some`/`None`, [[unwrap]], `unwrap_or`, [[match]], [[if-let|if let]], va `map`/`flatten`/`and_then` combinatorlari shu qatlamda keladi.
- `Result` bobi recoverable error modelini shu foundation'ga qo'shadi: [[result|Result<T, E>]], custom error enum, [[std-error-trait|std::error::Error]], combinatorlar, [[question-mark-operator|question mark operator]], va ignored `Result` uchun [[discarded-binding]] signali Rustning explicit error flow'ini mustahkamlaydi.
- Iterator bobi traversal modelini syntax orqasidan chiqaradi: [[iterators]], [[into-iterator|IntoIterator]], `.iter()`/`.into_iter()`, [[range]], lazy [[iterator-adapters|iterator adapters]], va [[consuming-adapters|consuming adapters]] orqali `for` loop ownership bilan bog'lanadi.
- Smart pointer bobi ownership modelini heap va runtime rule qatlami bilan kengaytiradi: [[smart-pointers]], [[box-t|Box<T>]], [[deref-trait|Deref]], [[rc-t|Rc<T>]], [[cell-t|Cell<T>]], [[refcell-t|RefCell<T>]], [[interior-mutability|interior mutability]], va [[arc-t|Arc<T>]] bir-biridan qaysi constraintni yechishi bilan ajratiladi.
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
- [[iterators]]
- [[into-iterator|IntoIterator]]
- [[iterator-adapters|iterator adapters]]
- [[consuming-adapters|consuming adapters]]
- [[collect]]
- [[break]]
- [[continue]]
- [[functions]]
- [[function-pointers|function pointers]]
- [[higher-order-functions|higher-order functions]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-methods|generic methods]]
- [[turbofish]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[generic-enums|generic enums]]
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
- [[where-clauses|where clauses]]
- [[associated-types|associated types]]
- [[self-type|Self type]]
- [[smart-pointers]]
- [[box-t|Box<T>]]
- [[recursive-types]]
- [[deref-trait|Deref trait]]
- [[deref-mut-trait|DerefMut trait]]
- [[rc-t|Rc<T>]]
- [[cell-t|Cell<T>]]
- [[refcell-t|RefCell<T>]]
- [[interior-mutability|interior mutability]]
- [[arc-t|Arc<T>]]
- [[sized-trait|Sized trait]]
- [[dyn-compatibility|dyn compatibility]]
- [[unsafe-trait|unsafe trait]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[const-generics|const generics]]
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
- [[if-let|if let]]
- [[or-pattern|or-pattern (`|`)]]
- [[match-guard|match guard]]
- [[at-binding|@ binding]]
- [[slice-patterns|slice patterns]]
- [[ref-pattern|ref pattern]]
- [[option|Option]]
- [[unwrap]]
- [[result|Result]]
- [[error-handling]]
- [[recoverable-errors|recoverable errors]]
- [[std-error-trait|std::error::Error]]
- [[question-mark-operator|question mark operator]]
- [[error-propagation|error propagation]]

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

```rust
fn transform<F>(a: i32, f: F) -> i32
where
    F: Fn(i32) -> i32,
{
    f(a)
}

let step = 5;
let add_step = move |x| x + step;
println!("{}", transform(2, add_step));
```

```rust
struct Holder<T> {
    v: T,
}

impl<T: Clone> Holder<T> {
    fn clone_value(&self) -> T {
        self.v.clone()
    }
}

fn make_array<T: Copy, const SIZE: usize>(init_value: T) -> [T; SIZE] {
    [init_value; SIZE]
}
```

```rust
let evens_squared = vec![1, 2, 3, 4, 5]
    .into_iter()
    .filter(|x| x % 2 == 0)
    .map(|x| x * x)
    .collect::<Vec<_>>();
```

```rust
use std::{cell::RefCell, rc::Rc};

let shared = Rc::new(RefCell::new(1));
*shared.borrow_mut() += 10;
println!("{}", shared.borrow());
```

```rust
enum Shape {
    Square { width: f32 },
    Rectangle { width: f32, height: f32 },
}

if let Shape::Square { width } = Shape::Square { width: 4.0 } {
    println!("{width}");
}
```

```rust
let maybe_name: Option<String> = Some("Ali".to_string());
let upper = maybe_name.map(|name| name.to_uppercase());
```

```rust
fn read_text_file(path: &str) -> Result<String, std::io::Error> {
    let mut file = std::fs::File::open(path)?;
    let mut contents = String::new();
    use std::io::Read;
    file.read_to_string(&mut contents)?;
    Ok(contents)
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
- `fn(i32) -> i32` qabul qiladigan va `F: Fn(i32) -> i32` qabul qiladigan ikki API yozib, qaysi biri capturing closure'ni qabul qilishini ko'rsating.
- `FnMut` stateful counter closure va `FnOnce` consuming closure yozib, farqini ko'rsating.
- `Holder<T>` uchun `impl Holder<i32>` va `impl<T: Clone> Holder<T>` yozib, qaysi method qaysi instantiationda mavjud bo'lishini tekshiring.
- `make_empty_vec::<i32>()` va `let xs: Vec<i32> = make_empty_vec();` orasidagi inference farqini yozing.
- Generic traitni associated type bilan qayta yozib, qaysi API signal o'zgarganini tushuntiring.
- `make_array<T: Copy, const SIZE: usize>`ga o'xshash yana bitta `const generics` helper yozing.
- `.iter()`, `&collection`, `.into_iter()`, va `collection` orqali bir xil data ustida item type hamda ownership farqini jadvalga tushiring.
- `Rc<Cell<T>>` va `Rc<RefCell<T>>` bilan bitta shared-state example yozib, qaysi model replacement, qaysi model borrow-based mutation ekanini ajrating.
- C-like enum va payloadli enum bilan bitta domainni ikki xil modellashtirib, qaysi joyda `match` va `if let` o'zgarishini ko'rsating.
- `Option<User>`dan `Option<String>` field extractionni `map`, `flatten`, va `and_then` bilan yozib solishtiring.
- String error qaytaradigan functionni custom enum error + `?` bilan qayta yozing.
- Ignored `Result` warningini chaqirib, keyin `let _ = ...;` bilan ongli bosib ko'ring.

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
- Qachon callable `fn` pointer bo'ladi, qachon closure object bo'ladi?
- Nega `Fn`, `FnMut`, `FnOnce` thread-safety emas, callability/capture behavior modeli?
- Nega ikki xil closure branchini `impl Fn` bilan bitta return type ostida ushlab bo'lmaydi?
- Nega `Holder<T>`ni compile-time template deb o'qish `generic struct`ni oddiy type deb o'qishdan to'g'riroq?
- `turbofish` qachon zarur, qachon variable type annotationning o'zi yetarli?
- Generic trait va associated type orasidagi eng muhim ikkita design farqi nima?
- `const generics` oddiy type parameterdan qaysi jihati bilan sifat jihatdan boshqa?
- `for item in collection` va `for item in &collection` nega ownership jihatdan boshqa natija beradi?
- Iterator laziness qayerda tugaydi va `collect`, `fold`, `reduce`, `find` orasida terminal behavior qanday farqlanadi?
- `Box<T>` uchun "zero-cost abstraction" deganda nimani tushunish kerak, nimani emas?
- `Cell<T>` bilan `RefCell<T>` orasidagi eng muhim amaliy farq nima?
- Nega enum memory layoutini faqat discriminant + payload sxemasi bilan to'liq tasdiqlangan representation deb qabul qilish xato?
- `Option` nega empty string, `-1`, yoki null pointerdan kuchliroq signal beradi?
- `unwrap` nega `unsafe` emas, lekin baribir productionda ehtiyotkor ishlatilishi kerak?
- `?` operator nega error handling emas, error propagation instrumenti?
- Ignored `Result` uchun `let _ = ...;` nega oddiy warning suppression emas, semantic signal?

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
- [[wiki/sources/rust-for-backend-developers-anonymous-functions]]
- [[wiki/sources/rust-for-backend-developers-generics]]
- [[wiki/sources/rust-for-backend-developers-enums]]
- [[wiki/sources/rust-for-backend-developers-option]]
- [[wiki/sources/rust-for-backend-developers-result]]
- [[wiki/sources/rust-for-backend-developers-iterators]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
