---
title: "18.1. Characteristics of Object-Oriented Languages"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, traits]
source_count: 1
---

# 18.1. Characteristics of Object-Oriented Languages

## Source

- URL: https://doc.rust-lang.org/stable/book/ch18-01-what-is-oo.html
- Raw: `raw/books/the_rust_programming_language/18.1. Characteristics of Object-Oriented Languages.md`

## Detailed Summary

### Ob'ektlar: ma'lumot va xatti-harakat

Gang of Four ta'rifi: "OOP dasturlari ob'ektlardan iborat. Ob'ekt ma'lumot va shu ma'lumotni qayta ishlovchi protseduralarni birlashtiradi."

Rust'da bu ta'rifga ko'ra OOP: `struct`/`enum` ma'lumot saqlaydi, `impl` bloki metodlar beradi. Ular rasman "object" deb atalmasada, funksional jihatdan ekvivalent.

### Encapsulation (kapsullash)

Implementatsiya tafsilotlarini yashirish — faqat public API orqali muloqot. Rust `pub` kalit so'zi bilan bu xususiyatni to'liq qo'llab-quvvatlaydi.

`AveragedCollection` misoli — ichki `list` va `average` fieldlari private, faqat `add`, `remove`, `average` metodlari public. Bu invariant ta'minlaydi: `add`/`remove` chaqirilganda `average` doim yangilanadi. Klient kodi to'g'ridan-to'g'ri `list`ga kira olmaydi va internal implementation (`Vec` → `HashSet`ga) o'zgarganda ham klient kodi o'zgarmaydi.

```rust
pub struct AveragedCollection {
    list: Vec<i32>,    // private — invariant himoya
    average: f64,      // private — cache
}
impl AveragedCollection {
    pub fn add(&mut self, value: i32) { self.list.push(value); self.update_average(); }
    pub fn remove(&mut self) -> Option<i32> { /* ... */ }
    pub fn average(&self) -> f64 { self.average }
    fn update_average(&mut self) { /* private */ }
}
```

### Inheritance (meros)

Rust'da struct'lar uchun inheritance **yo'q** — field'larni va metod implementatsiyalarini merodan olish macro ishlatilmasdan mumkin emas.

Lekin inheritance'ning ikkita asosiy maqsadi Rust'da boshqacha yetishiladi:
1. **Kod qayta ishlatish** → default trait method implementatsiyalari
2. **Tip tizimi polimorfizmi** → trait objects (`dyn Trait`)

Inheritance'ning muammolari: ortiqcha kod ulashish, subclass'lar parent class'ning barcha xususiyatlarini meros olishi (ba'zida noto'g'ri), single inheritance cheklovi.

### Polymorphism

Polimorfizm — "bir necha turdagi ma'lumot bilan ishlay oladigan kod". Inheritance polimorfizmning faqat bir turi.

Rust ikki xil polimorfizm beradi:
- **Generics + trait bounds** — *bounded parametric polymorphism*, compile-time
- **Trait objects (`dyn Trait`)** — runtime polimorfizm, inheritance'ning to'g'ri alternativasi

## Key Concepts

- [[encapsulation]] — `pub`/private orqali implementatsiya yashirish
- [[polymorphism]] — generics + trait objects, OOP'dagi inheritance o'rniga
- [[trait-object|trait object]] — runtime polimorfizm mexanizmi
- [[default-trait-implementations|default trait implementations]] — kod qayta ishlatish (inheritance o'rniga)
- [[privacy]] — encapsulation uchun Rust mexanizmi

## Code Examples

- `AveragedCollection` — encapsulation namunasi (inline, alohida page shart emas)

## Exercises or Practice Ideas

1. `AveragedCollection` ga `contains(&self, value: i32) -> bool` metodi qo'shing.
2. `list` fieldini `HashSet<i32>`ga o'zgartiring — klient kodi o'zgaradimi?
3. Rust'da inheritance'ga ekvivalent default trait metodi yozing.

## Questions Raised

- Default trait methods inheritance'ning to'liq o'rnini bosa oladimi?
- Rust nega inheritance o'rniga trait objects'ni tanladi?

## Links To Update

- [[wiki/chapters/18-1-characteristics-of-object-oriented-languages]]
- [[encapsulation]] — yangi concept page
- [[polymorphism]] — yangi concept page
