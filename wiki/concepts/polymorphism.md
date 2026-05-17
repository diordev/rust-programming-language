---
title: "Polymorphism"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, oop, generics, traits]
source_count: 2
---

# Polymorphism

## Short Definition

Polymorphism — "bir necha turdagi ma'lumot bilan ishlay oladigan kod" yozish. Rust ikki xil polimorfizm beradi: generics orqali **compile-time** va trait objects orqali **runtime**.

## Why It Matters

Ko'p OOP tillarida polimorfizm inheritance orqali ta'minlanadi. Rust inheritance'ni qo'llamaydi, lekin ikkala xil polimorfizmni aniqroq mexanizmlar bilan beradi. Har biri uchun tradeoff bor — to'g'ri tanlash muhim.

## Mental Model

**Compile-time polimorfizm** (generics): shablonlar fabrikasi — `largest<T>` kabi funksiya har bir konkret tur uchun alohida versiya yaratadi. Traits kontekstida bu ko'pincha [[static-dispatch|static dispatch]] va [[monomorphization]] orqali ko'rinadi. Tezroq, lekin barcha turlar compile-time'da ma'lum bo'lishi kerak.

**Runtime polimorfizm** (trait objects): interfeysga dasturlash — `&dyn Draw` "draw qila oladigan narsani menga bering" deydi. Sekinroq, lekin runtime'da yangi turlar qo'shish mumkin.

## Syntax and Examples

### Generics — bounded parametric polymorphism

```rust
// T: Summary bound bilan — compile-time polimorfizm
fn notify<T: Summary>(item: &T) {
    println!("{}", item.summarize());
}

// Monomorphization: kompilyator ikkita versiya yaratadi
notify::<NewsArticle>(&article);  // inlined, zero-cost
notify::<SocialPost>(&post);
```

### Trait Objects — runtime polimorfizm

```rust
// &dyn Summary — runtime'da istalgan Summary impl
fn notify(item: &dyn Summary) {
    println!("{}", item.summarize());
}

// Heterojen to'plam:
let items: Vec<Box<dyn Summary>> = vec![
    Box::new(NewsArticle { /* ... */ }),
    Box::new(SocialPost { /* ... */ }),
];
for item in &items { notify(item.as_ref()); }
```

### Qachon qaysi?

```rust
// Generics — bir xil turdagi collection va tez kod:
fn largest<T: PartialOrd>(list: &[T]) -> &T { /* ... */ }
// Vec<T> — homojen, T compile-time ma'lum

// Trait objects — aralash turlar, kutubxona kengaytirish:
struct Screen { components: Vec<Box<dyn Draw>> }
// Button, SelectBox, Image — barchasi birgalikda
```

## Polymorphism turlari

| Tur | Mexanizm | Qachon |
|---|---|---|
| **Parametric** (generics) | Monomorphization | Homojen to'plam, max performance |
| **Ad-hoc** (trait bounds) | Static dispatch | Bir funksiya, ko'p tur |
| **Runtime** (trait objects) | Dynamic dispatch, vtable | Heterojen to'plam, kengaytiriladigan API |

Rust'da **inheritance polimorfizmi yo'q** — bu ataylab qilingan dizayn qarori.

## Common Mistakes

- **Har doim trait objects ishlatmoq**: ko'p holatlarda generics yetarli va tezroq.
- **Inheritance o'rni sifatida kutish**: Rust'da `Button extends Component` yo'q — `impl Draw for Button` bor.

## Related Concepts

- [[generics]] — compile-time polimorfizm
- [[trait-object|trait object]] — runtime polimorfizm mexanizmi
- [[dynamic-dispatch|dynamic dispatch]] — trait objects ostidagi mexanizm
- [[monomorphization]] — generics'dan static dispatch
- [[static-dispatch|static dispatch]]

## Sources

- [[wiki/sources/18-1-characteristics-of-object-oriented-languages]]
- [[18-2-using-trait-objects-to-abstract-over-shared-behavior]]
- [[wiki/sources/rust-for-backend-developers-traits]]
