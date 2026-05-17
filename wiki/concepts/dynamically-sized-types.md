---
title: "Dynamically Sized Types (DST)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, types, memory, layout]
source_count: 1
---

# Dynamically Sized Types (DST)

## Short Definition

O'lchami compile-time'da noma'lum bo'lgan tiplar — *DST* yoki *unsized types*. Misollar: `str`, `[T]` (slice), `dyn Trait`. **Oltin qoida:** DST'larni har doim pointer orqali (reference, `Box`, `Rc`) ishlatish kerak.

## Why It Matters

Real ma'lumotlar — string'lar, slice'lar, dynamic dispatch — runtime'da turli o'lchamga ega. Rust compile-time'da har tip o'lchamini bilishi kerak (stack layout, function calling). DST'lar bu cheklovni "pointer + metadata" sxemasi bilan hal qiladi.

## Mental Model

```
Sized type:   stack'da to'g'ridan-to'g'ri yashash mumkin (i32, struct).
DST:          stack'da yashay olmaydi. Faqat pointer orqali — pointer sized va u
              ortidagi data bilan birga uning o'lchamini saqlaydi.
```

```
&str       =  &[ptr to str data] [length]      ←  fat pointer (2 word)
&[T]       =  &[ptr to slice]    [length]      ←  fat pointer
&dyn Trait =  &[ptr to data]     [vtable ptr]  ←  fat pointer
```

DST referencei "fat pointer" — oddiy pointer + metadata.

## Asosiy Misollar

### `str` — `&str` emas, faqat `str`

```rust
let s1: str = "Hello";    // E0277 — `str` Sized emas
let s2: str = "World";
```

Compilation error: `str` o'lchami noma'lum, stack'ga sig'maydi. Yechim:

```rust
let s1: &str = "Hello";          // OK — &str fat pointer
let b1: Box<str> = Box::from("Hello");   // OK — heap'da str + length pointer
```

### Slice `[T]`

```rust
let arr: [i32] = [1, 2, 3];   // E0277 — slice `Sized` emas
let arr: &[i32] = &[1, 2, 3]; // OK
let arr: Box<[i32]> = vec![1, 2, 3].into_boxed_slice();   // OK
```

Massiv (`[T; N]`) — fixed size, `Sized`. Slice (`[T]`) — DST.

### `dyn Trait`

```rust
trait Draw { fn draw(&self); }

let item: dyn Draw = ...;          // E0277
let item: &dyn Draw = &Button { /*...*/ };   // OK — fat pointer (data + vtable)
let item: Box<dyn Draw> = Box::new(Button { /*...*/ });   // OK
```

`dyn Trait` — DST chunki implementatsiya runtime'da aniqlanadi.

## `Sized` Trait

`Sized` — marker trait. Compile-time'da o'lchami ma'lum bo'lgan har tip avtomatik `Sized`. Generic funksiyalar default'da `T: Sized` talabiga ega:

```rust
fn generic<T>(t: T) { /* ... */ }
//        ↑
//   actually: fn generic<T: Sized>(t: T)
```

Compiler bu bound'ni implicit qo'shadi, chunki `t: T` stack'da bo'ladi.

## `?Sized` — Cheklovni Bo'shatish

DST'larni qabul qila oladigan generic yozish uchun `?Sized` ishlatamiz:

```rust
fn generic<T: ?Sized>(t: &T) { /* ... */ }
```

- `?Sized` — "T may or may not be Sized."
- `t: T` o'rniga `t: &T` — DST referencesini qabul qila olishi uchun.

`?Trait` sintaksisi **faqat `Sized` uchun** mavjud, boshqa trait'lar bilan ishlamaydi.

### Misol — generic `print` DST'lar uchun

```rust
fn print<T: ?Sized + std::fmt::Display>(t: &T) {
    println!("{}", t);
}

fn main() {
    print(&42);          // i32 (Sized)
    print("hello");      // &str — &str dereference -> str (DST)
}
```

## DST'lar Qaerda Saqlanadi

Pointer orqasidan har xil joy:
- `&str`, `&[T]`, `&dyn Trait` — referans (lifetime'li)
- `Box<str>`, `Box<[T]>`, `Box<dyn Trait>` — heap, owned
- `Rc<str>`, `Arc<[T]>`, `Rc<dyn Trait>` — shared heap
- `&'static str` — string literal, binary'ning .data segmentida

## Common Mistakes

- **`let s: str = ...;` yozish.** DST stack'da yashay olmaydi.
- **`?Sized` ni `Sized` deb adash qilish.** `?Sized` cheklovni *kuchsizlantiradi* (DST qabul qiladi).
- **`fn f<T: ?Sized>(t: T)` yozish.** Argument o'zi DST bo'lsa stack'ga sig'maydi — `t: &T` yoki `t: Box<T>` kerak.
- **`?Sized` ni boshqa traitlarga qo'llash.** `?Send`, `?Sync` mavjud emas.

## Related Concepts

- [[sized-trait|Sized trait]] — DST aniqlash
- [[string-slice|string slice]] — `str` DST
- [[slices]] — `[T]` DST
- [[trait-object|trait object]] — `dyn Trait` DST
- [[reference]] — DST'lar uchun fat pointer
- [[box-t|Box<T>]] — DST'larni heap'da saqlash
- [[memory-safety|memory safety]]
- [[generics]] — `?Sized` cheklovni bo'shatish

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
