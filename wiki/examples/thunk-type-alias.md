---
title: "Thunk type alias — closure box"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, types, alias, closures]
source_count: 1
---

# Thunk Type Alias — Closure Box

## Listing 20-25 — Type alias siz

```rust
fn main() {
    let f: Box<dyn Fn() + Send + 'static> = Box::new(|| println!("hi"));

    fn takes_long_type(f: Box<dyn Fn() + Send + 'static>) {
        // --snip--
    }

    fn returns_long_type() -> Box<dyn Fn() + Send + 'static> {
        // --snip--
        Box::new(|| ())
    }
}
```

`Box<dyn Fn() + Send + 'static>` — to'rt komponent:
- `Box<...>` — heap allocation
- `dyn Fn()` — closure trait object (no arguments, no return)
- `+ Send` — boshqa thread'ga uzatilishi mumkin
- `+ 'static` — closure ichidagi reference'lar `'static` lifetime

Har funksiya signaturasida bir xil tip qaytadan yozilishi — **chiqimchi va xatoga moyil**.

## Listing 20-26 — `Thunk` alias

```rust
fn main() {
    type Thunk = Box<dyn Fn() + Send + 'static>;

    let f: Thunk = Box::new(|| println!("hi"));

    fn takes_long_type(f: Thunk) {
        // --snip--
    }

    fn returns_long_type() -> Thunk {
        // --snip--
        Box::new(|| ())
    }
}
```

Endi har joyda `Thunk` yoziladi.

## "Thunk" Atamasi

*Thunk* — kechiktirilgan baholanish uchun saqlangan kod. Functional programming va compiler theory'dagi atama. Closure box'i bu ta'rifga to'liq mos keladi: code stored to be evaluated later.

Mos nom tanlash — kodni o'qishini va xato'larni topishni osonlashtiradi.

## Tahlil

| | Avval | Type alias bilan |
|---|-------|---------------------|
| Yozish hajmi | Uzun, har joyda takror | Qisqa, bir martalik |
| Xato xavfi | `Send` yoki `'static`'ni unutish | Markazlashgan ta'rif |
| Refactor | Har joyni o'zgartirish | Bitta `type` deklaratsiyasi |
| Mos nomlash | Qiyin (uzun tip) | `Thunk` ma'noli |

## Type Alias vs Newtype

```rust
// Type alias — `Thunk` va `Box<dyn Fn()...>` bir xil tip
type Thunk = Box<dyn Fn() + Send + 'static>;
let t1: Thunk = Box::new(|| ());
let b1: Box<dyn Fn() + Send + 'static> = t1;   // OK — bir xil tip

// Newtype — alohida tip
struct Thunk2(Box<dyn Fn() + Send + 'static>);
let t2 = Thunk2(Box::new(|| ()));
let b2: Box<dyn Fn() + Send + 'static> = t2;   // E0308 — boshqa tip
```

Maqsadga qarab tanlash:
- Faqat qisqartirish kerak → **type alias**
- Type safety, encapsulation kerak → **newtype**

## Related

- [[type-alias|type alias]]
- [[newtype-pattern|newtype pattern]]
- [[box-t|Box<T>]]
- [[closures]]
- [[fn-traits|Fn traits]]
- [[send-trait|Send trait]]
- [[static-lifetime|'static lifetime]]
- [[trait-object|trait object]]
- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
