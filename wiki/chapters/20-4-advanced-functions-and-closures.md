---
title: "20.4. Advanced Functions and Closures"
type: chapter
status: active
created: 2026-05-09
updated: 2026-05-09
tags: [rust, functions, closures, advanced]
source_count: 1
---

# 20.4. Advanced Functions and Closures

## Learning Goal

Oddiy functionlarni value sifatida uzatish, closure qaytarish, `impl Fn` opaque typelarini tushunish, va turli closure typelarini bitta collectionda saqlash uchun `Box<dyn Fn>` ishlatishni o'rganish.

## Main Ideas

- `fn` lowercase - [[function-pointers|function pointer]] tipi; `Fn`, `FnMut`, `FnOnce` esa closure traitlari.
- Function pointer uchala [[fn-traits|Fn trait]]ni implement qiladi, shuning uchun closure kutadigan ko'p APIlarga named function ham uzatiladi.
- API uchun `F: Fn(...)` ko'pincha `fn(...)`dan moslashuvchanroq, chunki u function pointer ham, closure ham qabul qiladi.
- `fn` pointer faqat function pointer talab qilinadigan holatlarda, ayniqsa C-style [[ffi|FFI]], foydali.
- Return position `impl Fn(...) -> ...` bitta concrete closure type qaytaradi.
- Har closure alohida concrete type; turli `impl Fn` returnlar bitta `Vec`da turishi uchun [[trait-object|trait object]] kerak: `Box<dyn Fn(...) -> ...>`.

## Bobning Tarkibi

| Bo'lim | Mavzu |
|--------|-------|
| Function Pointers | `fn(i32) -> i32`, named function argument sifatida |
| Function pointer vs closure bound | `fn` type, `Fn` trait, API design |
| `map` examples | Closure, `ToString::to_string`, `Status::Value` |
| Returning closures | `impl Fn`, `move`, capture |
| Opaque type problem | Har `impl Trait` return alohida type |
| Trait object solution | `Box<dyn Fn>` bilan heterogeneous handlers |

## `fn` Pointer vs `Fn` Trait

| | `fn(i32) -> i32` | `F: Fn(i32) -> i32` |
|---|------------------|---------------------|
| Nima? | Concrete function pointer type | Generic trait bound |
| Named function | Qabul qiladi | Qabul qiladi |
| Capturing closure | Qabul qilmaydi | Qabul qiladi |
| Non-capturing closure | Coerce bo'lishi mumkin | Qabul qiladi |
| Asosiy use case | FFI, exact pointer kerak | Rust API'lari, flexible callback |

## Asosiy Misollar

### Function Pointer

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

let answer = do_twice(add_one, 5);
assert_eq!(answer, 12);
```

Batafsil: [[function-pointer-do-twice]].

### Named Function in `map`

```rust
let list_of_numbers = vec![1, 2, 3];
let list_of_strings: Vec<String> =
    list_of_numbers.iter().map(ToString::to_string).collect();
```

Batafsil: [[map-with-named-functions]].

### Enum Variant Initializer

```rust
enum Status {
    Value(u32),
    Stop,
}

let statuses: Vec<Status> = (0u32..20).map(Status::Value).collect();
```

`Status::Value` - `u32 -> Status` initializer function.

### Returning One Closure Type

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

### Returning Multiple Closure Types

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}

fn returns_initialized_closure(init: i32) -> Box<dyn Fn(i32) -> i32> {
    Box::new(move |x| x + init)
}
```

Batafsil: [[boxed-closure-handlers]].

## Concepts

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
- [[iterators]]
- [[ffi|FFI]]

## Examples

- [[function-pointer-do-twice|Function pointer with do_twice]]
- [[map-with-named-functions|map with named functions and enum initializers]]
- [[boxed-closure-handlers|Boxed closure handlers]]

## Exercises

1. `do_twice`ni `fn` pointer bilan va generic `F: Fn(i32) -> i32` bilan ikki variantda yozing.
2. Capturing closure'ni `fn` pointer qabul qiladigan functionga uzatib ko'ring. Nima uchun ishlamasligini yozing.
3. `map(|i| i.to_string())` va `map(ToString::to_string)`ni solishtiring.
4. `enum Status { Value(u32), Stop }` uchun `Status::Value` initializer'ni function pointer sifatida ishlating.
5. `make_multiplier(n) -> impl Fn(i32) -> i32` yozing.
6. Ikki xil closure handlerni `Vec<Box<dyn Fn(i32) -> i32>>` ichida saqlang.

## Review Questions

1. `fn` va `Fn` orasidagi farq nima?
2. Function pointer qaysi closure traitlarini implement qiladi?
3. Nega Rust API'larida ko'pincha `F: Fn(...)` `fn(...)`dan yaxshiroq?
4. Qaysi holatda faqat `fn` qabul qilish ma'qul?
5. `ToString::to_string` nega fully qualified syntax orqali yoziladi?
6. Enum variant nomi qanday qilib initializer function bo'ladi?
7. `impl Fn(i32) -> i32` return type nimani yashiradi?
8. Nega ikki xil closure qaytaruvchi functionlarning return typelari bir xil emas?
9. `Box<dyn Fn>` qaysi muammoni hal qiladi?
10. `Box<dyn Fn>` qanday runtime tradeoff kiritadi?

## Related Pages

- [[wiki/sources/20-4-advanced-functions-and-closures|Source: 20.4]]
- [[wiki/chapters/20-3-advanced-types|Chapter 20.3: Advanced Types]] - `impl Trait`, `dyn Trait`, DST konteksti.
- [[wiki/chapters/13-functional-language-features|Chapter 13: Closures and Iterators]]
- [[wiki/chapters/18-2-using-trait-objects|Chapter 18.2: Trait Objects]]
- [[wiki/chapters/20-5-macros|Chapter 20.5: Macros]]
