---
title: "Raw Borrow Operators"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe, pointers, syntax]
source_count: 3
---

# Raw Borrow Operators

## Short Definition

`&raw const` va `&raw mut` — qiymatdan to'g'ridan-to'g'ri raw pointer yaratuvchi yangi operatorlar. Reference avval yaratib, keyin cast qilishdan ko'ra xavfsizroq.

## Why It Matters

Eski usul (`&value as *const _`) avval **reference** yaratardi, keyin uni raw pointer'ga aylantirardi. Lekin reference yaratish o'zi borrow qoidalariga bo'ysundi va vaqtinchalik (transient) xatoga olib kelishi mumkin edi — masalan, packed struct yoki mutable static uchun. Raw borrow operatorlari oraliq referencni yaratmasdan to'g'ri pointer beradi.

## Mental Model

```
&raw const value   →  *const T   (immutable raw pointer)
&raw mut value     →  *mut T     (mutable raw pointer)
```

`&value` (reference) yaratilmaydi — borrow checker chaqirilmaydi.

Appendix B `*const T` va `*mut T` raw pointer type syntax'ini reference qiladi. `&raw const` va `&raw mut` operatorlari amalda aynan shu raw pointer turlarini value'dan hosil qilishning zamonaviy usulidir.

Oldingi ecosystem'da shu vazifa ko'pincha `std::ptr::addr_of!` va `addr_of_mut!` bilan bajarilgan; source ularni historical alternative sifatida qayd qiladi.

## Syntax and Examples

```rust
let mut num = 5;

// Yangi sintaksis (idiomatik)
let r1 = &raw const num;   // *const i32
let r2 = &raw mut num;     // *mut i32

// Eski sintaksis (hali ishlaydi, lekin tavsiya etilmaydi)
let r3 = &num as *const i32;
let r4 = &mut num as *mut i32;
```

### Mutable static uchun — majburiy

```rust
static mut COUNTER: u32 = 0;

unsafe fn add(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}

fn main() {
    unsafe {
        add(3);
        // *(&raw const COUNTER) — reference yaratmasdan o'qish
        println!("COUNTER: {}", *(&raw const COUNTER));
    }
}
```

Mutable static'ga reference yaratish (`&COUNTER`) `static_mut_refs` lint orqali default'da man etilgan. Raw borrow operatori bu lintdan chetlab o'tadi.

### Packed struct field uchun

```rust
#[repr(packed)]
struct P { x: u32 }

let p = P { x: 5 };
let ptr = &raw const p.x;   // unaligned ham bo'lsa OK
// let bad = &p.x;           // E0793 — unaligned reference
```

## Common Mistakes

- **`&raw const` ni dereference qilish unutish.** `*(&raw const x)` ni unsafe blokda yozish kerak; faqat `&raw const x` raw pointer beradi.
- **Eski cast sintaksis bilan aralashtirish.** `&val as *const _` deb yozsangiz oraliq reference yaratiladi; mutable static yoki packed field uchun xavfli.
- **`raw` keyword'ini boshqa joyda ishlatishga urinish.** Bu faqat `&raw const`/`&raw mut` ichida ma'noga ega.

## Related Concepts

- [[raw-pointer|raw pointer]] — natijaviy turi
- [[unsafe-rust|unsafe Rust]] — dereference uchun zarur
- [[mutable-static|mutable static]] — asosiy foydalanish konteksti
- [[reference]] — alternativ, lekin borrow qoidalariga bog'liq
- [[rust-2024-edition|Rust 2024 Edition]] — yangi sintaksis editioni
- [[rust-operators-and-symbols]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
