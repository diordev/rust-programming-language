---
title: "Trait Object"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, oop, traits, polymorphism]
source_count: 4
---

# Trait Object

## Short Definition

Trait object — `dyn Trait` orqali ifodalanadigan runtime abstraction bo'lib, amalda `&dyn Trait`, `Box<dyn Trait>`, `Arc<dyn Trait>` kabi pointer/smart pointerlar orqali ishlatiladi. Runtime'da qaysi metod chaqirilishini vtable orqali aniqlaydi.

## Why It Matters

Generics compile-time'da bitta konkret turga speciallashadi — heterojen to'plamlar yaratib bo'lmaydi. Trait objects esa `Vec<Box<dyn Draw>>` kabi turli xil konkret turlarni bitta to'plamda saqlash imkonini beradi. Bu kengaytiriladigan API'lar uchun zarur: kutubxona yozilgandan keyin foydalanuvchi o'z turlarini qo'sha oladi.

## Mental Model

Trait object — "manzil kartasi": ichida ikkita narsa bor:
1. **Ma'lumot pointer** — konkret tur instansiyasiga
2. **Vtable pointer** — bu turning metod manzillari jadvaliga

Metod chaqirilganda: vtable'dan manzilni o'qib o'sha joyga sakrash. Generics bilan esa kompilyator to'g'ridan-to'g'ri sakraydi — oraliq yo'q.

Closure contextida `Box<dyn Fn(i32) -> i32>` turli closure concrete typelarini bitta collectionda saqlash uchun ishlatiladi. Bu [[returning-closures|returning closures]]da `impl Fn` opaque typelari bir xil bo'lmaganda kerak bo'ladi.

## Syntax and Examples

### Asosiy sintaksis

```rust
// dyn + pointer (Box, &, Arc, ...)
&dyn Trait
Box<dyn Trait>
Arc<dyn Trait>

// Concrete type bilan:
Box::new(Button { ... }) as Box<dyn Draw>
```

### GUI kutubxonasi namunasi

```rust
pub trait Draw { fn draw(&self); }

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}
impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();  // vtable orqali to'g'ri impl chaqiriladi
        }
    }
}

// Kutubxona ichida:
pub struct Button { pub width: u32, pub label: String }
impl Draw for Button { fn draw(&self) { println!("Drawing Button"); } }

// Foydalanuvchi kodi (kutubxonadan tashqari):
struct SelectBox { options: Vec<String> }
impl Draw for SelectBox { fn draw(&self) { println!("Drawing SelectBox"); } }

// Heterojen to'plam:
let screen = Screen {
    components: vec![
        Box::new(Button { width: 50, label: "OK".into() }),
        Box::new(SelectBox { options: vec!["Yes".into()] }),
    ],
};
screen.run();
```

### Funksiya parametrida

```rust
// Box<dyn Trait>:
fn render(item: Box<dyn Draw>) { item.draw(); }

// &dyn Trait (egalik olmaydi):
fn render(item: &dyn Draw) { item.draw(); }
```

Return positionda har xil concrete types chiqsa:

```rust
fn make_drawable(flag: bool) -> Box<dyn Draw> {
    if flag {
        Box::new(Button { width: 50, label: String::from("OK") })
    } else {
        Box::new(SelectBox { options: vec![String::from("Yes")] })
    }
}
```

### Box<dyn Trait> vs generic

```rust
// Generic — static dispatch, homojen:
fn draw_all<T: Draw>(items: &[T]) { for i in items { i.draw(); } }

// Trait object — dynamic dispatch, heterojen:
fn draw_all(items: &[Box<dyn Draw>]) { for i in items { i.draw(); } }
```

### Closure handler collection

```rust
let handlers: Vec<Box<dyn Fn(i32) -> i32>> = vec![
    Box::new(|x| x + 1),
    Box::new(|x| x * 2),
];
```

## Common Mistakes

- **`dyn Trait` pointer'siz ishlatmoq**: bare `dyn Draw` o'zi `Sized` emas — `&dyn Draw` yoki `Box<dyn Draw>` kerak.
- **Clone kabi non-object-safe trait**: ba'zi trait'lar `dyn` bilan ishlamaydi — dyn compatibility qoidalari.
- **Generic o'rnida keraksiz trait object**: heterojen to'plam kerak bo'lmasa, generics tezroq va aniqroq.

## dyn Compatibility

Barcha trait'lar trait object sifatida ishlayolmaydi. Asosiy qoida shuki, trait object uniform method table bera olishi kerak. `Self`ni return/extra argumentda ishlatadigan methods, generic methods, yoki receiver'siz static methods ko'pincha muammo qiladi. `Clone` odatda trait object bo'la olmaydi; associated itemlar bo'lgan traitlarda esa concrete bog'lanishlar aniq ko'rsatilishi kerak.

## Related Concepts

- [[dynamic-dispatch|dynamic dispatch]] — vtable, runtime metod lookup
- [[polymorphism]] — trait objects bilan runtime polimorfizm
- [[generics]] — compile-time, static dispatch alternativi
- [[monomorphization]] — generics'dan static dispatch
- [[static-dispatch|static dispatch]]
- [[box-t|Box<T>]] — trait object ko'p holda Box bilan ishlatiladi
- [[returning-closures|returning closures]] — `Box<dyn Fn>` handler collection uchun
- [[opaque-types|opaque types]] — `impl Trait` bilan farq
- [[pin|Pin]] — `dyn Future` — async'da trait object
- [[dynamically-sized-types|DST]] — `dyn Trait` DST; pointer orqali ishlatiladi
- [[sized-trait|Sized trait]] — `dyn Trait` `!Sized`
- [[dyn-compatibility|dyn compatibility]]

## Sources

- [[18-2-using-trait-objects-to-abstract-over-shared-behavior]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
- [[wiki/sources/rust-for-backend-developers-traits]]
