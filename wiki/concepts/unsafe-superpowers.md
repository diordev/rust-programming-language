---
title: "Unsafe Superpowers"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe]
source_count: 2
---

# Unsafe Superpowers

## Short Definition

`unsafe` blok ichida ruxsat etiladigan **5 ta operatsiya**. Faqat shu beshtasi `unsafe` talab qiladi; qolgan barcha xavfsizlik tekshiruvlari (borrow checker, type checker) hali ham ishlaydi.

## Why It Matters

`unsafe` "tekshiruvlarni o'chirish" emas — bu aniq ma'lum, kichik miqdordagi qobiliyatlarni ochish. Buning ahamiyati: memory bug topilganda, qarash kerak bo'lgan joy aniq — barcha `unsafe` bloklar.

## 5 ta Superpower

| # | Superpower | Misol |
|---|------------|-------|
| 1 | [[raw-pointer|Raw pointer]] dereference | `unsafe { *r1 }` |
| 2 | Unsafe function/method chaqirish | `unsafe { dangerous() }` |
| 3 | Mutable [[mutable-static|static]] ga kirish | `unsafe { COUNTER += 1 }` |
| 4 | [[unsafe-trait|Unsafe trait]] implement qilish | `unsafe impl Send for ...` |
| 5 | [[union-type|Union]] field'iga kirish | `unsafe { u.field }` |

## Mental Model

`unsafe` blok = "Men kompilyatorga aytmoqdaman: bu yerda men o'zim invariantlarni saqlayman." Kompilyator hech qanday tekshirishni o'chirmaydi, faqat 5 ta operatsiyaga ruxsat beradi.

```rust
unsafe {
    // 5 ta superpower'ning qaysi biri kerak bo'lsa shu yerda
    // Boshqa Rust kodi (borrow check, type check) hali ham ishlaydi
}
```

## Syntax and Examples

### Block kichik bo'lsin

```rust
fn safe_wrapper(...) -> ... {
    let prepared = ...;       // safe
    let result = unsafe {
        dangerous(prepared)   // faqat shu chaqiruv unsafe
    };
    process(result)           // safe
}
```

### Unsafe function ham kichik scope ishlatsin

```rust
unsafe fn do_thing() {
    let safe_part = compute();
    unsafe {
        raw_op();             // faqat zarur joy
    }
    cleanup(safe_part);
}
```

Rust 2024'da `unsafe fn` ichida ham `unsafe` block kerak (warn-by-default).

Backend pointer source superpower #1 - raw pointer dereference - uchun eng konkret beginner misolni beradi va nega pointer yaratish bilan pointer dereference'ni alohida ajratish kerakligini ko'rsatadi.

## Common Mistakes

- **`unsafe` "tekshiruvlarni o'chiradi" deb o'ylash.** Borrow checker, type checker — hammasi davom etadi.
- **Unsafe blokni ham keng qilish.** Memory bug topilganda har joyga qarashga to'g'ri keladi. Eng minimal scope tutib qoling.
- **`unsafe` nomli funksiyani ichidagi unsafe operatsiyani izohsiz qoldirish.** `SAFETY:` izohi konventsiyasi shart.
- **`unsafe` ni "to'liq xavfsiz emas" o'rniga "majburiyatlar bor" deb tushunmaslik.** Unsafe code to'g'ri ishlatilsa xavfsiz.

## Safe Abstraction

5 ta superpower'ning eng muhim ishlatilishi — ichkarida unsafe ishlatib, tashqarida safe API berish. Misol: [[smart-pointers|smart pointer]] yoki [[safe-abstraction|safe abstraction]] sahifasiga qarang.

## Related Concepts

- [[unsafe-rust|unsafe Rust]] — umumiy
- [[raw-pointer|raw pointer]] — superpower #1
- [[mutable-static|mutable static]] — superpower #3
- [[unsafe-trait|unsafe trait]] — superpower #4
- [[union-type|union]] — superpower #5
- [[safe-abstraction|safe abstraction]] — unsafe'ni o'rab olish
- [[memory-safety|memory safety]] — `unsafe` chetlab o'tishi mumkin

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
