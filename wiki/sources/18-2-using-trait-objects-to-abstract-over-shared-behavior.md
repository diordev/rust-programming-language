---
title: "18.2. Using Trait Objects to Abstract over Shared Behavior"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, trait-objects, dynamic-dispatch]
source_count: 1
---

# 18.2. Using Trait Objects to Abstract over Shared Behavior

## Source

- URL: https://doc.rust-lang.org/stable/book/ch18-02-trait-objects.html
- Raw: `raw/books/the_rust_programming_language/18.2. Using Trait Objects to Abstract over Shared Behavior.md`

## Detailed Summary

### Muammo: kengaytiriladigan GUI kutubxonasi

Kutubxona `Button`, `TextField` tiplarini ta'minlaydi. Lekin foydalanuvchi `Image`, `SelectBox` kabi yangi tiplar qo'shishini xohlaydi. Kutubxona yozilayotgan vaqtda barcha tiplar ma'lum emas — enum yopiq to'plam beradi, generic esa compile-time'da bitta turdagi to'plam.

Yechim: `Draw` trait + `Box<dyn Draw>` — runtime'da istalgan yangi tipni qo'llash imkonini beradi.

### Trait Object sintaksisi

```rust
pub trait Draw { fn draw(&self); }

// Screen istalgan Draw implementorini ushlab turadi:
pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,  // trait object
}
impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();  // runtime'da to'g'ri metod chaqiriladi
        }
    }
}
```

`Box<dyn Draw>` — *trait object*: `Draw` ni implement qilgan istalgan tur, heap'da, pointer orqali.

### Generic vs Trait Object

```rust
// GENERIC — homojen to'plam (faqat bitta T):
pub struct Screen<T: Draw> {
    pub components: Vec<T>,  // Vec<Button> YOKI Vec<TextField>, lekin aralash emas
}

// TRAIT OBJECT — heterojen to'plam:
pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,  // Button va TextField birga
}
```

| | Generic `Screen<T>` | Trait Object `Screen` |
|---|---|---|
| To'plam | Homojen (faqat bitta tur) | Heterojen (aralash turlar) |
| Dispatch | Static (monomorphization) | Dynamic (vtable) |
| Performance | Tezroq (zero-cost) | Sekinroq (runtime lookup) |
| Kengayish | Faqat compile-time ma'lum turlar | Runtime'da yangi turlar |

### Duck typing

`run` metodida `component`ning konkret turi tekshirilmaydi. Faqat `.draw()` chaqiriladi. `Draw` implement qilgan har qanday tur ishlaydi — bu dinamik tillardagi duck typing'ga o'xshash, lekin type-safe: kompilyator kafolatlaydiki, faqat `Draw` implement qilganlar `Vec`ga kirishi mumkin.

```rust
// E0277 — String Draw implement qilmaydi:
components: vec![Box::new(String::from("Hi"))]
// error: the trait `Draw` is not implemented for `String`
```

### Dynamic vs Static Dispatch

**Static dispatch** (generics): kompilyator compile-time'da qaysi metod chaqirilishini biladi → monomorphization → inlining mumkin → tezroq.

**Dynamic dispatch** (`dyn`): kompilyator qaysi metod chaqirilishini bilmaydi → runtime'da **vtable** (virtual dispatch table) orqali qidiradi → qo'shimcha overhead → inlining imkonsiz.

`dyn` compatibility qoidalari — barcha traitlar `dyn` bilan ishlavermaydi (masalan, `Clone` işlamaydi). Bu qoidalar Chapter 20'da batafsil.

### Button va SelectBox implementatsiyasi

```rust
// Kutubxona ichida:
pub struct Button { pub width: u32, pub height: u32, pub label: String }
impl Draw for Button { fn draw(&self) { /* ... */ } }

// Foydalanuvchi kodi — tashqi:
struct SelectBox { width: u32, height: u32, options: Vec<String> }
impl Draw for SelectBox { fn draw(&self) { /* ... */ } }

// Birgalikda heterojen Vec:
let screen = Screen {
    components: vec![
        Box::new(SelectBox { width: 75, height: 10, options: vec![...] }),
        Box::new(Button { width: 50, height: 10, label: String::from("OK") }),
    ],
};
screen.run();
```

## Key Concepts

- [[trait-object|trait object]] — `Box<dyn Trait>`, `&dyn Trait`, vtable
- [[dynamic-dispatch|dynamic dispatch]] — runtime metod qidirish, vtable, overhead
- [[polymorphism]] — trait objects bilan runtime polimorfizm
- [[generics]] — compile-time, static dispatch, homojen to'plam
- [[monomorphization]] — generics'dan static dispatch sababchisi

## Code Examples

- [[gui-draw-trait]] — GUI kutubxonasi: Screen, Button, SelectBox

## Exercises or Practice Ideas

1. `Image` strukturasini qo'shing va `Draw` ni implement qiling.
2. `Screen<T: Draw>` (generic) versiyasini yozing va farqni ko'rsating.
3. `Clone` trait'ini `dyn Clone` sifatida ishlatmoqchi bo'ling — xatolik sababini tushuntiring.

## Questions Raised

- `dyn compatibility` nima? Qaysi trait'lar `dyn` bilan ishlamaydi?
- `Box<dyn Trait>` va `&dyn Trait` qachon qaysi?

## Links To Update

- [[18-2-using-trait-objects]]
- [[trait-object]] — yangi concept page
- [[dynamic-dispatch]] — yangi concept page
