---
title: "Генерики - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, generics]
source_count: 1
---

# Генерики - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/32. Генерики.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/generics.html

## Detailed Summary

Bu source `generics`ni "bir xil behaviorni type'dan ajratib yozish" deb boshlaydi, lekin amaliy markazi shunchaki `<T>` syntax emas. Asosiy signal: Rust'dagi generic item concrete type emas, balki compile-time template. `Holder<T>` misoli orqali source avval generic struct, keyin generic function, keyin `impl<T>` block mental modelini ketma-ket yig'adi. Saqlanadigan practical nuance: struct ichidagi `T`, functiondagi `T`, va `impl<T>`dagi `T` bir xil harf bo'lsa ham, scope bo'yicha alohida declaration; bu yerda harf convention, semantika emas.

`Holder<T>` uchun `get` va `set` methodlari generic method mental modelini tozalaydi. `impl<T> Holder<T>` ichidagi birinchi `T` impl block scope'ini ochadi, ikkinchi `T` esa aynan qaysi generic struct specialization bilan bog'lanayotganini ko'rsatadi. Bu beginner uchun syntax shovqini bo'lib ko'rinadi, lekin keyingi conditional impl va concrete-only impl'lar uchun tayanch shu yerda.

Source'ning markaziy nazariy qismi [[monomorphization]]. Muhim durable takeaway: Rust generics runtime'da erib ketadigan metadata emas, compile time'da concrete variantlarga aylantiriladigan template modelidir. `Holder<i32>` va `Holder<bool>` misoli shuni ochadi. Java/C# generics bilan comparison bor, lekin wiki'da bu ehtiyotkor yozilishi kerak: source type erasure'ni beginner contrast sifatida ishlatadi, umumiy "hamma boshqa tillar shunday" degan xulosa chiqarmaydi.

[[turbofish]] bo'limi syntax trivia emas, inference yetmaydigan call site muammosining yechimi sifatida foydali. `make_empty_vec<T>() -> Vec<T>` misolida compiler argumentdan `T`ni topa olmaydi; shuning uchun yoki variable annotation, yoki `make_empty_vec::<i32>()` kerak bo'ladi. Durable signal: expression context'dagi generic specialization bilan type context'dagi `Vec<i32>` boshqa parser vazifasini bajaradi.

Source keyin generic traits va [[associated-types|associated types]]ni taqqoslaydi. Bu yerda ikkita farq saqlanishi kerak. Birinchisi: generic traitda concrete type argument ko'pincha caller/usage context orqali tanlanadi, associated type'da esa implementor `impl Trait for Type` ichida bir marta belgilaydi. Ikkinchisi: generic trait bir xil concrete type uchun bir nechta impl olish imkonini beradi (`ValueProducer<i32>` va `ValueProducer<String>`), associated type bilan bu model ishlamaydi. Source buni `CanBeAccessed<T>` va `type ElementType` misollari orqali ochadi.

[[trait-bounds|Trait bounds]] bo'limi generic code "har qanday `T`" emasligini aniq qiladi. `duplicate<T: Copy>` misoli behavior constraintni compile time contractga aylantiradi. Bu source yana muhim bir chegarani ko'rsatadi: oddiy parameter holatida `T: Trait` va `impl Trait` deyarli bitta static dispatch modeliga olib boradi, lekin expressiveness bir xil emas. Ayniqsa `FnMut() -> R` kabi nested trait bound bilan ishlaganda generic parameters kuchliroq.

[[where-clauses|where clauses]] source'da faqat style masalasi sifatida emas, nested generics o'qilishini boshqarish vositasi sifatida ishlaydi. `print_produced<F, R>` misoli shuni ko'rsatadi: `F: FnMut() -> R, R: Display` constrainti inline yozilganda signature og'irlashadi, `where` bilan esa function shape bir qarashda ko'rinadi.

Source `impl Trait` limitini ham amaliy ko'rsatadi. `fn print_produced(mut f: impl FnMut() -> impl Display)` ishlamaydi, chunki `impl Trait` `Fn` trait bounds ichidagi return type'da ruxsat etilmaydi. Shuning uchun bu joyda generic return parameter (`R: Display`) kerak. Bu [[impl-trait|impl Trait]]ni "har joyda generic replacement" deb tushunish xato ekanini ko'rsatadi.

Keyingi bo'lim generic method specializationni ikki usulda beradi: concrete-only `impl Holder<i32>` va trait-bound bilan `impl<T: Clone> Holder<T>`. Bu yerda source aslida "hamma method hamma instantiation uchun mavjud emas" degan muhim design signalni beradi. Generic API surface compile-time capability'ga qarab torayishi mumkin.

Oxirgi bo'lim [[const-generics|const generics]]. Muhim durable model: Rust generic parameterlari faqat type emas, compile-time constant ham bo'lishi mumkin. `[T; SIZE]` array type'i bu yerda asosiy motivatsiya. `fn make_array<T: Copy, const SIZE: usize>(init_value: T) -> [T; SIZE]` va `print_times<const QUANTITY: usize>` misollari type-level shape bilan value-level repetitionni birlashtiradi. Source bu yerda `Copy` boundni ham bekorga qo'ymaydi: `[init_value; SIZE]` aynan shu constraintni talab qiladi.

## Key Concepts

- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-methods|generic methods]]
- [[monomorphization]]
- [[turbofish]]
- [[traits]]
- [[trait-bounds|trait bounds]]
- [[where-clauses|where clauses]]
- [[impl-trait|impl Trait]]
- [[associated-types|associated types]]
- [[function-pointers|function pointers]]
- [[fn-traits|Fn traits]]
- [[const-generics|const generics]]

## Code Examples

```rust
struct Holder<T> {
    v: T,
}

impl<T> Holder<T> {
    fn get(&self) -> &T {
        &self.v
    }

    fn set(&mut self, new_v: T) {
        self.v = new_v;
    }
}
```

```rust
fn make_empty_vec<T>() -> Vec<T> {
    Vec::new()
}

let numbers = make_empty_vec::<i32>();
```

```rust
fn print_produced<F, R>(mut f: F)
where
    F: FnMut() -> R,
    R: std::fmt::Display,
{
    println!("{}", f());
}
```

```rust
fn make_array<T: Copy, const SIZE: usize>(init_value: T) -> [T; SIZE] {
    [init_value; SIZE]
}
```

## Exercises or Practice Ideas

- `Holder<T>`ga `replace(self, new_v: T) -> T` yoki `into_inner(self) -> T` kabi yana bitta method qo'shib, ownership qanday siljishini yozing.
- `make_empty_vec<T>` misolini variable annotation bilan va `turbofish` bilan ikki xil yozib, qaysi biri qayerda o'qilish yaxshiroq ekanini solishtiring.
- Bir xil concrete type uchun generic traitning ikki impl'i ishlaydigan, associated type esa ishlamaydigan kichik example yozing.
- `impl Holder<i32>` va `impl<T: Clone> Holder<T>` bilan qaysi method qaysi instantiationda ko'rinishini tekshiring.
- `make_array<T: Copy, const SIZE: usize>`ni `String` bilan chaqirib, nega `Copy` bound yiqilishini compiler orqali ko'ring.

## Questions Raised

- Qachon generic trait ishlatish kerak, qachon associated type bilan contractni bitta bog'langan type'ga tushirish to'g'ri?
- Qachon `impl Trait` yetarli, qachon `T: Trait` yoki alohida `R` generic parametri kerak bo'ladi?
- `monomorphization` runtime costni yo'qotsa ham, compile time yoki binary size tradeoffi qayerda paydo bo'ladi?
- `const generics` array shape bilan cheklanadimi, yoki keyingi advanced API design'da yana qayerlarda kuchli?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-generics]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[generic-functions|generic functions]]
- [[generic-structs|generic structs]]
- [[generic-methods|generic methods]]
- [[monomorphization]]
- [[turbofish]]
- [[trait-bounds|trait bounds]]
- [[where-clauses|where clauses]]
- [[associated-types|associated types]]
- [[impl-trait|impl Trait]]
- [[const-generics|const generics]]
