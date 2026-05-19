---
title: "Pin va Unpin"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-19
tags: [rust, async, memory, safety]
source_count: 2
---

# Pin va Unpin

## Short Definition

`Pin<P>` — `P` pointer'i ko'rsatgan qiymatni xotirada qotirib qo'yuvchi wrapper. `Unpin` — marker trait: "bu tip `Pin` ichida ham ko'chirish xavfsiz" degani. `Pin` ko'proq async future'lar bilan ishlashda kerak bo'ladi.

## Why It Matters

Async `Future`'larning state machine'lari **self-referential** bo'lishi mumkin — bir variant boshqa variantga reference saqlaydi. Bunday tur ko'chirilsa, ichki reference yangi manzilga ko'rsatmay, eski (endi yaroqsiz) manzilga ko'rsatib qoladi — dangling reference. `Pin` bu muammoni hal qiladi: qiymatni xotirada qotirib, ko'chirishni taqiqlaydi.

Shuning uchun `Future::poll` signature'i `self: Pin<&mut Self>` oladi. Oddiy backend async kodda `Pin` ko'pincha bevosita yozilmaydi, lekin custom future yoki executor yozilganda u darhol ko'rinadi.

## Mental Model

Devorga mix (pin) qoqilgan plakat: plakat boshqa joyga olib ketilolmaydi, lekin uni ko'rsatuvchi chiziq har qanday joyda bo'lishi mumkin.

```
Pin ──→ Box ──→ [Future state machine (qotgan, ko'chmas)]
                        ↑
               ichki reference — doim valid
```

`Box` pointer ko'chishi mumkin. Faqat `Future` ma'lumotlarining o'zi qotadi.

## Syntax and Examples

### Pin yaratish: pin! macro va Box::pin

```rust
use std::pin::{Pin, pin};

// Stack'da pin:
let fut = pin!(async { /* ... */ });

// Heap'da pin (tashqaridan qaytarish kerak bo'lsa):
let fut: Pin<Box<dyn Future<Output=()>>> = Box::pin(async { /* ... */ });
```

### join_all bilan Vec ichida future'lar

```rust
use std::pin::{Pin, pin};

let fut1 = pin!(async move { /* tx yuborish */ });
let fut2 = pin!(async       { /* rx qabul qilish */ });
let fut3 = pin!(async move  { /* tx2 yuborish */ });

let futures: Vec<Pin<&mut dyn Future<Output = ()>>> =
    vec![fut1, fut2, fut3];

trpl::join_all(futures).await;
```

**Xatosiz kompilyatsiya**: `pin!` bilan `Vec<Pin<&mut dyn Future>>` ishlaydi.

### await o'zi pin qiladi

```rust
let result = my_future.await;
// ^^^ bu yerda Rust implicit ravishda my_future ni pin qiladi
// Manual pin kerak emas!
```

`pin!` faqat future'ni Vec yoki boshqa strukturaga solish kerak bo'lganda kerak.

### Unpin implement qilmagan tip

```rust
use std::marker::PhantomPinned;

struct SelfReferential {
    data: String,
    self_ref: *const String,
    _pin: PhantomPinned,  // !Unpin marker
}
```

`PhantomPinned` qo'shilsa, tur `!Unpin` bo'ladi va `Pin` kafolati kuchga kiradi.

## Common Mistakes

- **Har doim `pin!` kerak deb o'ylash**: `await` avtomatik pin qiladi. `pin!` faqat future'ni koleksiyaga solish kerak bo'lsa ishlatiladi.
- **`Box<dyn Future>` vs `Box::pin(dyn Future)`**: birinchisi `Unpin` talab qiluvchi API'larda ishlamaydi.
- **`Pin` pointer'ni qotirib qo'yadi deb o'ylash**: `Pin` ma'lumotni qotirib qo'yadi, pointer'ni emas.

## Unpin — "ko'chirish xavfsiz" markeri

`Unpin` implement qilgan tiplar uchun `Pin` hech qanday qo'shimcha kafolat bermaydi — ular `Pin` bilan ham bemalol ko'chiriladi. Shuning uchun `String`, `Vec`, `i32` uchun `Pin` bo'lsa ham bular ko'chirilaveradi.

```rust
// Unpin tiplar:
String    // ✓ Unpin — ichki reference yo'q
Vec<T>    // ✓ Unpin
i32       // ✓ Unpin

// !Unpin tiplar:
async {}  // ✗ !Unpin — self-referential bo'lishi mumkin
```

## Related Concepts

- [[future|Future trait]] — `poll(self: Pin<&mut Self>...)` — Pin zarurligi
- [[executor]] — custom executor future'ni `Pin<Box<dyn Future>>` sifatida saqlashi mumkin
- [[async-state-machine|async state machine]] — self-referential variantlar sabab
- [[join-all|join_all]] — Vec ichida future'lar, Pin talab qiladi
- [[send-trait|Send trait]] — o'xshash marker trait pattern

## Sources

- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]]
- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
