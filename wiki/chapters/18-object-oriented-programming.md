---
title: "18. Object Oriented Programming Features"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, traits]
source_count: 3
---

# 18. Object Oriented Programming Features

## Learning Goal

OOPning asosiy xususiyatlari (objects, encapsulation, inheritance, polymorphism) Rust'da qanday ifodalanishini va inheritance o'rniga trait objects'ni qachon, qanday ishlatishni tushunish.

## Main Ideas

### Rust OOPmi?

OOP ta'rifi bo'yicha konsensus yo'q. Rust ba'zi OOP xususiyatlarini qo'llab-quvvatlaydi, ba'zilarini emas:

| OOP xususiyat | Rust'da |
|---|---|
| **Objects** (data + behavior) | ✓ `struct`/`enum` + `impl` |
| **Encapsulation** | ✓ `pub`/private |
| **Inheritance** | ✗ yo'q (boshqa yechimlar bor) |
| **Polymorphism** | ✓ generics (compile-time) + `dyn Trait` (runtime) |

### Encapsulation → pub/private

```rust
pub struct AveragedCollection {
    list: Vec<i32>,    // private: invariant himoya
    average: f64,      // private: cache, doim sinxron
}
impl AveragedCollection {
    pub fn add(&mut self, value: i32) { /* list + update_average */ }
    pub fn remove(&mut self) -> Option<i32> { /* ... */ }
    pub fn average(&self) -> f64 { self.average }
    fn update_average(&mut self) { /* private */ }
}
```

Klient kodi `list`ga to'g'ridan-to'g'ri kira olmaydi → implementation (`Vec`→`HashSet`) o'zgarganda klient kodi o'zgarmaydi.

### Inheritance yo'q → alternativlar

Rust'da struct inheritance yo'q, lekin ikkala maqsad yetishiladi:

| Inheritance maqsadi | Rust alternativasi |
|---|---|
| Kod qayta ishlatish | Default trait method implementatsiyalari |
| Runtime polimorfizm | Trait objects (`Box<dyn Trait>`) |

### Polymorphism ikki ko'rinishda

```rust
// 1. Generics — compile-time, static dispatch:
fn draw_all<T: Draw>(items: &[T]) { /* T aniq bitta tur */ }

// 2. Trait objects — runtime, dynamic dispatch:
fn draw_all(items: &[Box<dyn Draw>]) { /* aralash turlar */ }
```

## Concepts

- [[encapsulation]] — `pub`/private orqali invariant himoya
- [[polymorphism]] — generics va trait objects
- [[trait-object|trait object]] — `dyn Trait`, `Box<dyn Draw>`
- [[dynamic-dispatch|dynamic dispatch]] — vtable, runtime metod qidirish
- [[default-trait-implementations|default trait implementations]] — inheritance'ning kod-ulashish alternativasi

## Examples

- [[gui-draw-trait]] — Screen, Button, SelectBox — trait objects namunasi

## Exercises

1. `AveragedCollection` ichki tuzilmasini o'zgartiring — klient kodi o'zgarmasligi kerak.
2. `Draw` trait + `Vec<Box<dyn Draw>>` bilan o'z GUI komponentingizni yozing.
3. Generics `Screen<T: Draw>` va trait object `Screen` versiyalarini solishtiring.

## Review Questions

1. Rust qaysi OOP xususiyatlarini qo'llaydi, qaysilarini emas?
2. Inheritance o'rniga Rust qanday alternativalar taklif etadi?
3. Static dispatch va dynamic dispatch — farqlari va tradeoff'lari?
4. `Box<dyn Draw>` va `Box<Button>` qanday farqlanadi?

## Related Pages

- [[wiki/chapters/10-generic-types-traits-and-lifetimes|10. Generics, Traits, Lifetimes]] — generics va traits asoslari
- [[wiki/chapters/17-6-futures-tasks-and-threads|17.6]] — trait objects (`dyn Future`) real qo'llanishi
- [[18-object-oriented-programming-features|Source: 18]]
- [[wiki/sources/18-1-characteristics-of-object-oriented-languages|Source: 18.1]]
- [[18-2-using-trait-objects-to-abstract-over-shared-behavior|Source: 18.2]]
