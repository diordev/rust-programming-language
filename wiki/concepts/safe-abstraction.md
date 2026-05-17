---
title: "Safe Abstraction"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, api-design, patterns]
source_count: 1
---

# Safe Abstraction

## Short Definition

Ichkarida `unsafe` kod ishlatib, tashqarida invariantlar saqlangan **xavfsiz API** beruvchi pattern. Caller `unsafe` blok yozmasdan ishlatishi mumkin.

## Why It Matters

`unsafe` Rust'ning ichidagi "ikkinchi til" — uni butun kodbazaga tarqatmasdan, bitta moduldagi nazoratli joyda saqlash mumkin. Standart kutubxona deyarli butunlay shu pattern asosida — `Vec`, `String`, `HashMap` ichida unsafe bor, lekin foydalanuvchi sezmaydi.

## Mental Model

```
┌──────────────────────────────────────┐
│   Public API (safe fn)               │
│ ┌──────────────────────────────────┐ │
│ │  fn body                         │ │
│ │   ├─ safe Rust                   │ │
│ │   ├─ unsafe { audited block }    │ │
│ │   └─ safe Rust                   │ │
│ └──────────────────────────────────┘ │
└──────────────────────────────────────┘
        ↓ caller hech narsa sezmaydi
   safe_fn(...)   // hech qanday `unsafe` kerak emas
```

Funksiya `unsafe` deb belgilanmaydi — chunki ichidagi unsafe kod shu funksiya **kontekstida** xavfsiz (caller har qanday argumentni bersa ham invariantlar saqlanadi).

## Syntax and Examples

### `split_at_mut` — klassik misol

Bir mutable slice'ni ikkita non-overlapping mutable slice'ga bo'lish. Borrow checker buni tushunmaydi (bir xil slice'ni ikki marta mutably borrow qilolmaydi), lekin pozitsiyalar non-overlapping bo'lsa, xavfsiz.

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();

    assert!(mid <= len);   // <-- invariant tekshiruv

    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

E'tibor:
- Funksiya `unsafe` emas — caller hech qanday majburiyat olmaydi.
- `assert!(mid <= len)` invariantni isbotlaydi.
- Raw pointer (`ptr`) faqat `values` slicedan kelgan, demak valid.
- Hosil bo'lgan ikki slice non-overlapping (`[..mid]` va `[mid..]`) — Rust o'zi bilmaydi, lekin biz bilamiz.

### Anti-pattern: invariantlarni saqlamasdan unsafe ishlatish

```rust
fn bad_slice() -> &'static [i32] {
    let address = 0x01234usize;
    let r = address as *const i32;
    unsafe { std::slice::from_raw_parts(r, 10000) }   // arbitrary memory!
}
```

Bu safe abstraction emas — caller `bad_slice()` chaqirib UB oladi.

## Common Mistakes

- **Funksiya `unsafe` qiyofa beradi, lekin invariantlarni hujjatlamaydi.** "Bu unsafe, ehtiyot bo'l" deb yozish yetarli emas — `SAFETY:` izohida nima saqlanishi kerakligini tushuntirish.
- **Caller invariantni buzishi mumkin.** Public API har qanday inputni qabul qilishi mumkin — barcha holatlarda xavfsiz bo'lishi shart.
- **Unsafe blokni katta qilish.** Aniq invariant tekshiruvlaridan keyin tor blokda yozing.

## Standart Kutubxona Misollari

- `Vec::push` — kapacityni oshirish ichkarida unsafe
- `String::from_utf8` — bytes UTF-8 ekanligini tekshirib, keyin to'g'ridan-to'g'ri yangi `String` quradi
- `Box::leak` — Drop'siz reference qaytaradi
- `slice::from_raw_parts` (yaratuvchi) — o'zi unsafe, lekin uni o'rab olgan API'lar safe

## Related Concepts

- [[unsafe-rust|unsafe Rust]] — qaysi tekshiruvlardan voz kechiladi
- [[unsafe-superpowers|unsafe superpowers]] — qanday operatsiyalar
- [[raw-pointer|raw pointer]] — eng keng tarqalgan ishlatish
- [[smart-pointers]] — `Box`, `Rc`, `RefCell` — barchasi safe abstraction
- [[invariants]] — saqlanadigan qoidalar
- [[function-contracts|function contracts]] — caller bilan kelishuv
- [[api-design|API design]] — safe API yaratish

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
