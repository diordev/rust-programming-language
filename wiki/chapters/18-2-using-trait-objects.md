---
title: "18.2. Using Trait Objects to Abstract over Shared Behavior"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, oop, trait-objects, dynamic-dispatch]
source_count: 1
---

# 18.2. Using Trait Objects to Abstract over Shared Behavior

## Learning Goal

`Box<dyn Trait>` va `&dyn Trait` bilan heterojen to'plamlar yaratishni, static dispatch (generics) bilan dynamic dispatch (trait objects) ning farqini va qachon qaysi yondashuvni tanlashni o'rganish.

## Main Ideas

### Muammo: kengaytiriladigan GUI

Kutubxona foydalanuvchisi o'z tiplarini (Image, SelectBox) qo'sha olishi kerak. Enum yopiq to'plam — kutubxona chiqarilgandan keyin yangi variantlar qo'shib bo'lmaydi. Generic `Screen<T>` esa bitta turga cheklangan.

Yechim: `Box<dyn Draw>` — `Draw` implement qilgan **istalgan** tur qabul qilinadi.

### Box<dyn Trait> sintaksisi

```rust
pub trait Draw { fn draw(&self); }

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,  // heterojen to'plam
}
impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();  // runtime dispatch
        }
    }
}
```

`Box<dyn Draw>` — trait object:
- `dyn` — dynamic dispatch ishlatilishini bildiradi
- `Box<T>` — heap allocation (trait object o'lchami compile-time'da noma'lum)

### Generic vs Trait Object — qachon qaysi?

```rust
// Generics: bitta tur, compile-time, tez
pub struct Screen<T: Draw> { pub components: Vec<T> }
// Vec<Button> ✓  |  Vec<SelectBox> ✓  |  Vec<Box<Button, SelectBox>> ✗

// Trait objects: aralash turlar, runtime, moslashuvchan
pub struct Screen { pub components: Vec<Box<dyn Draw>> }
// vec![Box::new(Button{...}), Box::new(SelectBox{...})] ✓
```

| Mezon | Generics | Trait Objects |
|---|---|---|
| To'plam | Homojen | Heterojen |
| Dispatch | Static (compile-time) | Dynamic (runtime vtable) |
| Performance | Zero-cost | Runtime overhead |
| Kengayish | Faqat bilgan turlar | Runtime'da yangi turlar |
| Inlining | Mumkin | Mumkin emas |

**Qoida**: faqat bitta tur bo'lsa → generics; runtime'da turli turlar kerak bo'lsa → trait objects.

### Duck typing va type safety

`run` metodida `component`ning konkret turi tekshirilmaydi — faqat `.draw()` chaqiriladi. Bu dinamik tillardagi duck typing'ga o'xshash. Farqi: Rust kompilyatori `Draw` implement qilmaganlarni **compile-time'da** rad etadi:

```rust
components: vec![Box::new(String::from("Hi"))]
// error[E0277]: the trait `Draw` is not implemented for `String`
```

### Dynamic Dispatch — vtable

Trait object ishlatilaganda Rust **vtable** (virtual method table) yaratadi. `Box<dyn Draw>` ichida ikki pointer:
1. Ma'lumotga pointer (konkret tur instansiyasi)
2. Vtable'ga pointer (tur metodlarining manzillari)

`component.draw()` chaqirilganda: vtable'dan `draw` manzilini o'qib, shu manzilga sakrash. Bu runtime overhead: `static dispatch`da kompilyator to'g'ridan-to'g'ri chaqiradi.

## Concepts

- [[trait-object|trait object]] — `Box<dyn Trait>`, vtable, dyn keyword
- [[dynamic-dispatch|dynamic dispatch]] — vtable, runtime overhead
- [[polymorphism]] — heterojen to'plamlar, duck typing analogi
- [[generics]] — static dispatch, homojen to'plamlar
- [[monomorphization]] — generics'dan static dispatch

## Examples

- [[gui-draw-trait]] — Screen, Button, SelectBox

## Exercises

1. Kutubxonaga `Image` tipi qo'shing — `Draw` implement qiling.
2. `Screen<T: Draw>` versiyasini yozing — nima farq qiladi?
3. `String` ni `Vec<Box<dyn Draw>>`ga solishga harakat qiling.

## Review Questions

1. `Box<dyn Draw>` va `Box<Button>` qanday farqlanadi?
2. Static dispatch vs dynamic dispatch — qachon qaysi?
3. Nima uchun trait object heap allocation talab qiladi?
4. Duck typing va trait objects o'rtasidagi o'xshashlik va farq?

## Related Pages

- [[wiki/chapters/18-1-characteristics-of-object-oriented-languages|18.1]] — polymorphism kirish
- [[18-2-using-trait-objects-to-abstract-over-shared-behavior|Source: 18.2]]
