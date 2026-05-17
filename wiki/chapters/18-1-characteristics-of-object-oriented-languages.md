---
title: "18.1. Characteristics of Object-Oriented Languages"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, encapsulation, polymorphism]
source_count: 1
---

# 18.1. Characteristics of Object-Oriented Languages

## Learning Goal

OOPning asosiy uch xususiyati — objects, encapsulation, inheritance — Rust'da qanday ifodalanishini va Rust nima uchun inheritance'ni qo'llamasligini tushunish.

## Main Ideas

### Objects = Struct + impl

Gang of Four ta'rifi: ob'ekt = ma'lumot + metod. Rust'da `struct`/`enum` + `impl` = xuddi shu. Nom farqli, amalda ekvivalent.

### Encapsulation — AveragedCollection

Invariant: `list` o'zgarganda `average` doim yangilanishi kerak. Buni kafolatlovchi encapsulation:

```rust
pub struct AveragedCollection {
    list: Vec<i32>,   // private
    average: f64,     // private (cached)
}

impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();  // invariant saqlash
    }
    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(v) => { self.update_average(); Some(v) }
            None => None,
        }
    }
    pub fn average(&self) -> f64 { self.average }
    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

`list` private bo'lgani uchun: klient kodi `list.push()` to'g'ridan-to'g'ri chaqira olmaydi → `average` desync bo'lolmaydi. Kelajakda `Vec<i32>` o'rniga `HashSet<i32>` ishlatilsa ham public API o'zgarmaydi.

### Inheritance yo'q — nima ishlatiladi?

```
Inheritance maqsadi 1: Kod qayta ishlatish
  → Default trait method implementatsiyalari
  
Inheritance maqsadi 2: Runtime polimorfizm
  → Trait objects (dyn Trait)
```

Inheritance muammolari:
- Ortiqcha kod ulashish — subclass parent'ning keraksiz metodlarini ham oladi
- Single inheritance — ko'p tillar faqat bir parent'dan meros olishga ruxsat beradi
- Fragile base class — parent o'zgarsa barcha subclass'lar ta'sirlanadi

### Polymorphism — Rust yondashuvi

Rust *bounded parametric polymorphism* ishlatadi:

```rust
// Generics — compile-time polimorfizm:
fn notify<T: Summary>(item: &T) { println!("{}", item.summarize()); }
// T konkret turga aylantiriladi (monomorphization)

// Trait objects — runtime polimorfizm:
fn notify(item: &dyn Summary) { println!("{}", item.summarize()); }
// Runtime'da vtable orqali to'g'ri metod topiladi
```

## Concepts

- [[encapsulation]] — `pub`/private, invariant himoya
- [[polymorphism]] — generics + trait objects
- [[privacy]] — Rust'dagi encapsulation mexanizmi
- [[default-trait-implementations|default trait implementations]] — inheritance kodi qayta ishlatish alternativasi

## Exercises

1. `AveragedCollection` da `list` fieldini public qiling va muammoni ko'rsating.
2. Default trait metodi yozing — "inheritance"ni simulatsiya qiling.

## Review Questions

1. Encapsulation Rust'da qanday ta'minlanadi?
2. Inheritance'ning ikkita asosiy maqsadini va Rust alternativalarini ayting.
3. "Bounded parametric polymorphism" nima?

## Related Pages

- [[18-object-oriented-programming|18. OOP]] — umumiy ko'rinish
- [[18-2-using-trait-objects|18.2. Trait Objects]] — polimorfizm amalda
- [[wiki/sources/18-1-characteristics-of-object-oriented-languages|Source: 18.1]]
