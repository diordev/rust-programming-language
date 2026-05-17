---
title: "Rust for Backend Developers: Generics"
type: chapter
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, generics, chapter]
source_count: 1
---

# Rust for Backend Developers: Generics

## Learning Goal

`generics`ni syntax darajasida emas, compile-time abstraction modeli sifatida tushunish: generic struct/function/method, [[monomorphization]], [[turbofish]], generic traits vs [[associated-types|associated types]], [[trait-bounds|trait bounds]], `where`, conditional impl, va [[const-generics|const generics]]ni bitta chiziqqa tushirish.

## Main Ideas

- `Holder<T>` source'ning asosiy teaching vehicle'i: generic item concrete type emas, template; concrete type compiler specialization paytida paydo bo'ladi.
- `impl<T> Holder<T>` syntax'i ikki xil rol bajaradi: bir tomonda generic parameter scope ochadi, ikkinchi tomonda shu parameter bilan struct specializationni bog'laydi.
- [[monomorphization]] Rust genericlarining performance modeli: compiler `Holder<i32>` va `Holder<bool>`ga o'xshash concrete variantlar yaratadi; bu beginner darajada Java/C# type erasure contrasti bilan ko'rsatiladi, lekin ortiqcha umumlashtirilmaydi.
- [[turbofish]] `::<...>` syntax'i inference yetmaydigan expression context uchun kerak; zero-arg generic function chaqiruvida ayniqsa muhim.
- Generic traits va [[associated-types|associated types]] bir maqsadga yaqin, lekin contract boshqa joyda qotadi: generic trait type argumentni usage tarafida qoldiradi, associated type esa implementor tarafida bitta bog'langan type belgilaydi.
- [[trait-bounds|Trait bounds]] generic code capabilitysini cheklaydi: `T: Copy`, `T: Clone`, yoki `T: Trait1 + Trait2` body ichida qaysi operationlar halol ekanini belgilaydi.
- `T: Trait` va `impl Trait` oddiy parameter holatlarida bitta static dispatch modeliga olib boradi, lekin nested return relationship kerak bo'lsa generic parameter kuchliroq.
- [[where-clauses|where clauses]] ayniqsa `F: FnMut() -> R, R: Display` kabi nested generic constraintlarda signature'ni o'qiladigan qiladi.
- Concrete-only `impl Holder<i32>` va `impl<T: Clone> Holder<T>` source'ning muhim dizayn signali: method availability hamma instantiationda bir xil bo'lishi shart emas.
- [[const-generics|Const generics]] type-level shape'ni compile-time constant bilan parametrizatsiya qiladi; `[T; SIZE]` va `make_array<T: Copy, const SIZE: usize>` bu bobning asosiy amaliy misoli.

## Concepts

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

## Examples

```rust
struct Holder<T> {
    v: T,
}

fn make_holder<T>(v: T) -> Holder<T> {
    Holder { v }
}
```

```rust
fn make_empty_vec<T>() -> Vec<T> {
    Vec::new()
}

let xs: Vec<i32> = make_empty_vec();
let ys = make_empty_vec::<i32>();
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

## Exercises

- `Holder<T>`ga `map` yoki `into_inner` qo'shib, qaysi variant ownership olishini ko'rsating.
- Bitta example yozing: generic trait bilan bir type uchun ikki impl ishlasin, associated type bilan esa ishlamasin.
- `print_produced` misolini `impl FnMut() -> impl Display` shaklida yozib, compiler nima uchun rad etishini ko'ring.
- `impl Holder<i32>` va `impl<T: Clone> Holder<T>` method surface'larini bir xil API'da solishtiring.
- `const SIZE: usize` ishlatadigan yana bitta array helper yozing.

## Review Questions

- Nega `Holder<T>`ni "type" emas, "type template" deb o'qish to'g'riroq?
- `monomorphization` generic ergonomics va runtime performance orasida qanday bridge beradi?
- `turbofish` qachon kerak bo'ladi, qachon oddiy type annotation yetadi?
- Generic trait va associated type orasidagi eng muhim ikki farq nima?
- Nega `impl Trait` nested `Fn` trait bound ichidagi return type'ni ifodalay olmaydi?
- `const generics` oddiy type genericdan qaysi jihati bilan sifat jihatdan boshqa?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-generics]]
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
