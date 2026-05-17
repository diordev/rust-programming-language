---
title: "Sized Trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, traits, types, marker]
source_count: 3
---

# Sized Trait

## Short Definition

`std::marker::Sized` — marker trait. Tipning o'lchami compile-time'da ma'lumligini ko'rsatadi. Avtomatik implement qilinadi har sized tip uchun. Generic funksiyalar default'da `T: Sized` talab qiladi.

## Why It Matters

Rust kompilyatori har qiymatning stack'da qancha joy egallashini bilishi kerak. `Sized` — bu kafolat. Aksariyat tiplar `Sized`; lekin `str`, `[T]`, `dyn Trait` — [[dynamically-sized-types|DST]] va `Sized` emas. DST bilan ishlash uchun `?Sized` syntax kerak.

## Mental Model

```
Sized      ✓  →  i32, String, Vec<T>, MyStruct, [T; N]
                 (compile-time o'lchami ma'lum)

!Sized     ✗  →  str, [T], dyn Trait
                 (runtime'da aniqlanadi)
```

## Implicit `Sized` Bound

Har generic funksiya parametri default'da `Sized`:

```rust
fn generic<T>(t: T) { /* ... */ }
```

Aslida kompilyator quyidagicha o'qiydi:

```rust
fn generic<T: Sized>(t: T) { /* ... */ }
```

Sabab: `t: T` argument stack'da yashashi kerak → o'lchami ma'lum bo'lishi kerak.

## `?Sized` — Cheklovni Bo'shatish

DST qabul qila oladigan generic uchun:

```rust
fn print<T: ?Sized + std::fmt::Display>(t: &T) {
    println!("{}", t);
}

fn main() {
    print(&42);       // i32 — Sized
    print("hello");   // str — !Sized, lekin &str orqali OK
}
```

`t: T` → `t: &T` (DST'lar pointer orqali).

`?Sized` — "T may or may not be Sized." Faqat `Sized` uchun bu sintaksis mavjud — `?Send`, `?Sync` mavjud emas.

Amaliy signal: `&str`, `&[T]`, va `Box<dyn Trait>` `Sized`, chunki reference yoki smart pointerning o'lchami ma'lum; `str`, `[T]`, va bare `dyn Trait` esa `!Sized`.

## Auto Implementation

Kompilyator avtomatik implement qiladi:

```rust
struct A { x: i32 }     // implicitly: A: Sized
struct B(String);       // implicitly: B: Sized
struct C { data: [u8; 32] }    // implicitly: C: Sized

// Lekin:
struct D(str);          // D !Sized, chunki str !Sized
```

DST field'i bo'lgan struct ham DST bo'ladi, lekin DST faqat **oxirgi field** sifatida ruxsat etiladi (chunki o'lcham noma'lumligi struct oxirida bo'lishi kerak).

## DST Field — Custom DST

```rust
struct Wrapper {
    metadata: u32,
    data: str,        // DST — faqat oxirgi field bo'la oladi
}

let w: &Wrapper = ...;   // fat pointer (data + length)
```

Bu pattern `std`'da `[T]` ustida ishlaydigan custom struct'lar uchun ishlatiladi (kam ko'riladi).

## Common Mistakes

- **`?Sized` bo'lgan generic'da `t: T` ishlatish.** DST stack'ga sig'maydi — `&T`, `Box<T>` kerak.
- **`?Sized` `Sized` ni o'chiradi deb o'ylash.** Yo'q — bu cheklovni *bo'shatadi* (Sized va !Sized ikkalasini qabul qiladi).
- **Custom DST struct'da DST field'ni boshida joylash.** Compile error — DST field oxirgi bo'lishi kerak.
- **`Sized`'ni qo'lda implement qilishga urinish.** Auto-derive — qo'lda implement mumkin emas (sealed trait).
- **Bare `dyn Trait` value sifatida ishlaydi deb o'ylash.** `dyn Trait` `!Sized`, shuning uchun trait object odatda `&dyn Trait` yoki `Box<dyn Trait>` bilan uzatiladi.

## Related Concepts

- [[dynamically-sized-types|dynamically sized types (DST)]]
- [[generics]]
- [[trait-bounds|trait bounds]]
- [[reference]]
- [[box-t|Box<T>]]
- [[trait-object|trait object]]
- [[string-slice|string slice]]
- [[slices]]

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-traits]]
- [[wiki/sources/rust-for-backend-developers-common-traits]]
