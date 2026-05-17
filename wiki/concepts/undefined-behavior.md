---
title: "Undefined Behavior"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe, memory, correctness]
source_count: 3
---

# Undefined Behavior

## Short Definition

**Undefined Behavior (UB)** — Rust til specifikatsiyasi natijasini belgilamagan operatsiya. Kompilyator UB sodir bo'lmaydi deb taxmin qiladi va kod optimizatsiyalarini shu farazga asoslab quradi. UB sodir bo'lsa: dasturning butun xatti-harakati noaniq.

## Why It Matters

UB — segfault, ma'lumotlar buzilishi, sirli kompilyator optimizatsiyalari, va xavfsizlik teshiklarining asosi. Safe Rust UB'ni yaratmasligi shart; unsafe Rust'da esa dasturchi javobgar.

Backend application yozishda unsafe Rustdan qochish UB riskini keskin kamaytiradi; unsafe kerak bo'lsa invariantlar local va hujjatlangan bo'lishi kerak.

## Mental Model

UB "crash" emas — bu spektor: dastur to'g'ri ishlayotgandek ko'rinishi, faqat kichik condition'da yiqilishi, yoki butunlay loyiq yo'l bilan ishlamasligi mumkin. Compiler optimizatsiyalari UB'siz fan asosida quriladi, demak UB sodir bo'lganda taxminlar buziladi va natija butunlay ko'zga ko'rinmas yo'lga ketishi mumkin.

## UB'ning Asosiy Manbalari

| Manba | Misol |
|-------|-------|
| Invalid pointer dereference | `let r = 0x1234 as *const i32; unsafe { *r }` |
| Use-after-free | `Box::leak` qilingandan keyin `drop` |
| Out-of-bounds (raw pointer bilan) | `ptr.add(100)` keyin dereference |
| Data race | `static mut` ko'p thread'da |
| Unaligned access | Unaligned `*const T` dereference |
| Invalid type representation | `unsafe { mem::transmute::<u8, bool>(2) }` |
| Aliasing rules buzish | Bir xil xotirada `&mut T` va `&T` |
| Panic across FFI boundary | `extern "C"` orqali panic chiqarish |

## Misollar

### Reference qoidalarini buzish

```rust
let mut x = 5;
let p1 = &raw mut x;
let p2 = &raw const x;

unsafe {
    *p1 = 10;
    let v = *p2;   // UB? — p1 va p2 bir xil xotirada
}
```

Reference orqali bo'lmasin, **aliasing rules** raw pointer'da ham ba'zan zarur (Stacked Borrows model).

Backend pointer source `&mut -> *mut -> &mut` kabi patternning amaliy kuchini ko'rsatadi, lekin aynan shu pattern noto'g'ri invariant bilan UBga ketishi mumkinligini ham eslatadi. Bunday kod test va [[miri|Miri]] bilan tekshirilishi kerak.

### Listing 20-7 — ixtiyoriy adres

```rust
use std::slice;

let address = 0x01234usize;
let r = address as *mut i32;
let values: &[i32] = unsafe { slice::from_raw_parts_mut(r, 10000) };
// values'dan o'qish — UB
```

[[miri|Miri]] aniq aniqlaydi: "pointer must be dereferenceable for 40000 bytes, but got 0x1234 which is a dangling pointer."

## UB ni Topish

- [[miri|Miri]] — runtime UB detector (nightly)
- `cargo test` ga to'liq ishonmaslik — UB testdan chiqib ketishi mumkin
- AddressSanitizer, ThreadSanitizer (nightly orqali)
- `RUSTFLAGS="-Z sanitizer=address" cargo +nightly run`

## Common Mistakes

- **"Mening kompyuterda ishlayapti, demak UB yo'q"** — UB platforma, optimizatsiya, va kompilyator versiyasiga sezgir. CI'da boshqa muhitda yiqilishi mumkin.
- **"Unsafe block ichida hamma narsa OK"** — Unsafe blok faqat 5 ta operatsiyaga ruxsat beradi. Borrow rules, type rules saqlanadi.
- **`unsafe` bloklarni audit qilmaslik.** Har `unsafe` blok o'z invariantlari bilan hujjatlanishi kerak (`SAFETY:` izohi).

## Related Concepts

- [[unsafe-rust|unsafe Rust]]
- [[memory-safety|memory safety]] — UB'ning qarama-qarshi tomoni
- [[data-race|data race]] — eng keng tarqalgan UB sababi
- [[raw-pointer|raw pointer]] — UB'ning eng keng manbai
- [[miri|Miri]] — UB detection tool
- [[invariants]] — saqlash kerak bo'lganlar

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/rust-for-backend-developers-safe-rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
