---
title: "20.4. Advanced Functions and Closures"
type: source
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, functions, closures, advanced]
source_count: 1
---

# 20.4. Advanced Functions and Closures

## Source

- File: `raw/books/the_rust_programming_language/20.4. Advanced Functions and Closures.md`
- URL: <https://doc.rust-lang.org/stable/book/ch20-04-advanced-functions-and-closures.html>
- Book: *The Rust Programming Language*

## Detailed Summary

20.4 Rust'da function va [[closures]] bilan bog'liq ikki advanced mavzuni ko'rsatadi: [[function-pointers|function pointers]] va [[returning-closures|closure qaytarish]]. Bu bo'lim Chapter 13'dagi closures va Chapter 10/18'dagi `impl Trait` hamda trait object mental modellarini birlashtiradi.

### Function Pointers

Oddiy function ham qiymat sifatida boshqa functionga uzatilishi mumkin. Buning tipi lowercase `fn` bilan yoziladi:

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}
```

Bu yerda `fn(i32) -> i32` function pointer tipi. U `Fn`, `FnMut`, `FnOnce` traitlaridan farq qiladi:

- `fn` - konkret type.
- `Fn`, `FnMut`, `FnOnce` - closure traitlari.
- Function pointer uchala closure traitni implement qiladi.

Shu sababli ko'p hollarda API'ni `fn(...) -> ...` bilan emas, generic closure bound bilan yozish yaxshiroq:

```rust
fn apply_twice<F>(f: F, arg: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(arg) + f(arg)
}
```

Bunday signature ham named functionni, ham closure'ni qabul qiladi. Faqat `fn` qabul qilish kerak bo'ladigan muhim joylardan biri - C kabi closure tushunchasi yo'q tashqi kod bilan ishlash, ya'ni [[ffi|FFI]].

### Named Functions and Enum Variant Initializers in `map`

Iterator `map` closure qabul qiladi, lekin named function ham closure traitlarini implement qilgani uchun ishlaydi:

```rust
let list_of_strings: Vec<String> =
    list_of_numbers.iter().map(ToString::to_string).collect();
```

Bu misolda [[fully-qualified-syntax|fully qualified syntax]] kerak, chunki `to_string` nomli function bir nechta joyda bo'lishi mumkin. `ToString::to_string` standard library `ToString` traitidagi methodni aniq tanlaydi.

Enum variant nomi ham initializer function bo'ladi:

```rust
enum Status {
    Value(u32),
    Stop,
}

let list_of_statuses: Vec<Status> = (0u32..20).map(Status::Value).collect();
```

`Status::Value` har `u32` qiymatdan `Status::Value(u32)` yaratadigan function sifatida ishlatiladi. Inline closure (`|x| Status::Value(x)`) ham, initializer (`Status::Value`) ham bir xil compile bo'lishi mumkin; o'qilishi aniqroq uslub tanlanadi.

### Returning Closures

Closure'lar traitlar orqali ifodalanadi, shuning uchun "closure type" nomini odatda to'g'ridan-to'g'ri yozib bo'lmaydi. Agar function bitta closure implementation qaytarsa, return position `impl Trait` ishlatiladi:

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

Bu ishlaydi, chunki compiler aniq concrete closure typeni biladi, caller esa faqat `Fn(i32) -> i32` contractini ko'radi.

Lekin har closure o'zining alohida, nomsiz concrete type'iga ega. Ikki function bir xil signature bilan `impl Fn(i32) -> i32` qaytarsa ham, ularning return typelari bir xil emas:

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

fn returns_initialized_closure(init: i32) -> impl Fn(i32) -> i32 {
    move |x| x + init
}
```

Bu ikki `impl Fn(i32) -> i32` alohida [[opaque-types|opaque type]] hisoblanadi. Ularni bitta `Vec` ichida saqlashga urinish [[e0308-mismatched-types|E0308 mismatched types]] beradi: "expected opaque type, found a different opaque type".

Yechim: common type sifatida trait object ishlatish:

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn returns_initialized_closure(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}
```

`Box<dyn Fn(i32) -> i32>` barcha closure handlerlarni bitta type ostida saqlashga imkon beradi, lekin dynamic dispatch va heap allocation tradeoffini kiritadi.

## Key Concepts

- [[function-pointers|function pointers]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[returning-closures|returning closures]]
- [[opaque-types|opaque types]]
- [[impl-trait|impl Trait]]
- [[trait-object|trait object]]
- [[box-t|Box<T>]]
- [[dynamic-dispatch|dynamic dispatch]]
- [[fully-qualified-syntax|fully qualified syntax]]
- [[enum-variants|enum variants]]
- [[ffi|FFI]]
- [[iterators]]

## Code Examples

- [[function-pointer-do-twice|Function pointer with do_twice]] - Listing 20-28.
- [[map-with-named-functions|map with named functions and enum initializers]] - Listings 20-29, 20-30, 20-31.
- [[boxed-closure-handlers|Boxed closure handlers]] - Listings 20-32, 20-33, 20-34.

## Exercises or Practice Ideas

1. `fn do_twice(f: fn(i32) -> i32, arg: i32)` ni `F: Fn(i32) -> i32` bilan qayta yozing. Named function va closure bilan test qiling.
2. `Vec<i32>` ni `Vec<String>` ga ikki usulda convert qiling: `|i| i.to_string()` va `ToString::to_string`.
3. Enum variant initializer (`Status::Value`) va closure (`|x| Status::Value(x)`) variantlarini solishtiring.
4. `fn make_adder(n: i32) -> impl Fn(i32) -> i32` yozing. `move` nima uchun kerakligini tushuntiring.
5. Ikki xil closure qaytaruvchi functionlarni `Vec<Box<dyn Fn(i32) -> i32>>` ichida saqlang va har birini chaqiring.
6. `impl Fn` va `Box<dyn Fn>` uchun performance/API tradeoff jadvali tuzing.

## Questions Raised

- Qachon API `fn(...)` qabul qilishi kerak, qachon `F: Fn(...)` yaxshi?
- `impl Fn` qaytarish va `Box<dyn Fn>` qaytarish orasidagi cost farqi real dasturda qachon muhim?
- Closure type nomlanmagan bo'lsa, compiler diagnosticlarida "opaque type"ni qanday o'qish kerak?
- `FnOnce`, `FnMut`, `Fn` return type sifatida tanlashda capture behavior qanday rol o'ynaydi?

## Links To Update

- [[index|Rust Wiki Index]] - 20.4 source/chapter, yangi concept va examples.
- [[overview]] - Chapter 20.4 synthesis.
- [[wiki/chapters/20-4-advanced-functions-and-closures|Chapter 20.4]]
- [[closures]] - return closure va function pointer contexti.
- [[fn-traits]] - function pointer uchala traitni implement qilishi.
- [[impl-trait]] - return position opaque type.
- [[trait-object]] - `Box<dyn Fn>` handler collection.
- [[ffi]] - C closure qabul qilmagani uchun `fn` pointer contexti.
