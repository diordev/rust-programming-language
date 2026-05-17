---
title: "Dynamic Dispatch"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, oop, performance]
source_count: 2
---

# Dynamic Dispatch

## Short Definition

Dynamic dispatch — qaysi metod chaqirilishini **runtime'da** aniqlaydigan mexanizm. Trait objects (`dyn Trait`) ishlatilganda Rust dynamic dispatch qo'llaydi: vtable orqali to'g'ri metod topiladi.

## Why It Matters

[[static-dispatch|Static dispatch]] (generics + monomorphization) tezroq va inline qilish mumkin, lekin homojen to'plamlar beradi. Dynamic dispatch sekinroq, lekin heterojen to'plamlar va kengaytiriladigan API imkonini beradi. Tradeoff'ni bilish kerakli joyda to'g'ri yondashuv tanlashga yordam beradi.

## Mental Model

Restoran menyu analogi:
- **Static dispatch**: oshpaz (kompilyator) menyuni oldindan yod oladi — buyurtma kelganda darhol tayyorlaydi.
- **Dynamic dispatch**: kassir (vtable) menyuni papkadan qidiradi — bir necha daqiqa vaqt ketadi.

## Syntax and Examples

### Qachon dynamic dispatch ishlaydi

```rust
// dyn kalit so'zi → dynamic dispatch:
fn render(item: &dyn Draw) { item.draw(); }
fn render(item: Box<dyn Draw>) { item.draw(); }

// Vec<Box<dyn Draw>> — har element turli impl'ga ega bo'lishi mumkin
```

### Static vs Dynamic solishtirish

```rust
// STATIC dispatch (monomorphization):
fn notify<T: Summary>(item: &T) {
    println!("{}", item.summarize());
}
// Kompilyator notify::<NewsArticle> va notify::<SocialPost> alohida yaratadi
// Runtime'da to'g'ridan-to'g'ri chaqiruv — zero overhead

// DYNAMIC dispatch:
fn notify(item: &dyn Summary) {
    println!("{}", item.summarize());
}
// Runtime'da: item.vtable.summarize(item.data_ptr) — qo'shimcha dereference
```

### Vtable tuzilishi (konseptual)

```
Box<dyn Draw>
├── data_ptr → [Button { width: 50, label: "OK" }]
└── vtable_ptr → [Button_vtable]
                     ├── draw: Button::draw address
                     ├── drop: Button::drop address
                     └── size, align
```

Har bir `Box::new(Button {...})` → `dyn Draw`ga cast qilinganda Button uchun vtable pointer qo'shiladi.

## Performance Tradeoffs

Dynamic dispatch'ning overhead'lari:
1. **Pointer dereference** — vtable'dan metod manzilini o'qish (ekstra memory read)
2. **Inlining imkonsiz** — kompilyator qaysi metod chaqirilishini bilmaydi
3. **Branch prediction buzilishi** — CPU turli manzillarga sakrashni oldindan bila olmaydi

Amalda bu overhead ko'pincha ahamiyatsiz — I/O bound yoki murakkab biznes logikali kodda performance farqi sezilarli emas.

## Common Mistakes

- **Keraksiz trait objects**: heterojen to'plam kerak bo'lmasa generics afzal.
- **Hot path'da trait objects**: performance-critical loop'da `dyn` ishlatilsa profiling kerak.

## Related Concepts

- [[trait-object|trait object]] — dynamic dispatch'ni ishlatuvchi til konstruktsiyasi
- [[polymorphism]] — dynamic dispatch runtime polimorfizm beradi
- [[monomorphization]] — generics'dan static dispatch — dynamic dispatch'ning aksi
- [[generics]] — static dispatch, homojen to'plamlar
- [[static-dispatch|static dispatch]]

## Sources

- [[18-2-using-trait-objects-to-abstract-over-shared-behavior]]
- [[wiki/sources/rust-for-backend-developers-traits]]
