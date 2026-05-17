---
title: "Unsafe Trait"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe, traits]
source_count: 2
---

# Unsafe Trait

## Short Definition

`unsafe trait` — kompilyator tekshira olmaydigan invariantga ega trait. Implementatsiya `unsafe impl` orqali yoziladi va dasturchi shu invariantlarni qo'lda saqlashga va'da beradi.

## Why It Matters

Ba'zi trait'lar shunday majburiyatlarga ega bo'ladi-ki, kompilyator ularni kafolatlay olmaydi. Misol: `Send` (qiymatni boshqa thread'ga uzatish xavfsiz), `Sync` (ko'p thread'da shared reference orqali kirish xavfsiz). Kompilyator tomonidan auto-derived bo'ladi, lekin agar tip raw pointer kabi narsani o'z ichiga olsa va sizga safe bo'lsa — `unsafe impl Send` yozish kerak.

## Mental Model

`unsafe trait` o'qiganingizda: "Bu traitni implement qilmoqchi bo'lsangiz, **siz** invariantlarni saqlaysiz." `unsafe impl` yozganingizda: "Men shu majburiyatni o'z zimmamga olaman." Muhim nuance: trait methodlari ichida albatta `unsafe` block bo'lishi shart emas; xavf implementorning invariant va'dasida.

## Syntax and Examples

### Trait deklaratsiyasi va implementatsiyasi

```rust
unsafe trait Foo {
    // Methods may have invariants compiler can't verify
}

unsafe impl Foo for i32 {
    // Method implementations
}
```

### Standart kutubxonadagi misollar

```rust
pub unsafe auto trait Send { }
pub unsafe auto trait Sync { }
```

`auto` — auto-derive (struktura barcha field'lari `Send` bo'lsa, struktura ham `Send`).

### `Send` ni qo'lda implement qilish

```rust
struct MyType {
    ptr: *mut u8,   // *mut Send/Sync emas
}

// Dasturchi: men ptr'ni faqat to'g'ri ishlataman
unsafe impl Send for MyType {}
unsafe impl Sync for MyType {}
```

Bu juda ehtiyotkor qaror — agar `ptr` haqiqatda thread-safe bo'lmasa, butun program UB'ga olib kelishi mumkin.

## Qachon Trait `unsafe` Bo'lishi Kerak

Trait'ni `unsafe` qiling, agar:
1. Method'lar invariantga ega va kompilyator tekshira olmaydi.
2. Notog'ri implementatsiya boshqa **safe** kodning xatosiga olib kelishi mumkin.

Misol: `Iterator` trait `unsafe` emas — noto'g'ri implementatsiya buggy bo'lsa ham memory safety buzilmaydi. `Send` esa unsafe — noto'g'ri implementatsiya thread'lar orasida data race ochishi mumkin.

## Common Mistakes

- **`Send`/`Sync` ni o'zboshimcha implement qilish.** Faqat tip ichidagi har bir invariant haqiqatan ham thread-safe bo'lsa.
- **`unsafe trait` o'rniga oddiy trait yozish.** Agar method invariantga ega bo'lsa va caller buzsa UB bo'lsa — trait `unsafe` bo'lishi shart.
- **Auto trait'ni implement qilishga urinish bilan `auto` belgilash uchun.** `auto` faqat standart kutubxonada (`Send`, `Sync`, `Unpin`, `UnwindSafe`).

## Related Concepts

- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]] — superpower #4
- [[traits]] — umumiy
- [[send-trait|Send trait]] — eng keng tarqalgan unsafe trait
- [[sync-trait|Sync trait]]
- [[invariants]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-traits]]
