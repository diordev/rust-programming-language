---
title: "split_at_mut — safe abstraction over unsafe"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, slices, safe-abstraction]
source_count: 1
---

# split_at_mut — Safe Abstraction Over Unsafe

## Tavsif

Bir mutable `&mut [T]` ni ikki **non-overlapping** mutable slice'ga bo'lish. [[borrow-checker|Borrow checker]] buni qabul qilmaydi (bir slice'ni ikki marta mutably borrow qilolmaysiz), lekin pozitsiyalar non-overlapping bo'lsa — xavfsiz. `unsafe` bilan implement qilinadi, lekin tashqi API safe.

## Listing 20-4 — Foydalanish

```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5, 6];
    let r = &mut v[..];
    let (a, b) = r.split_at_mut(3);

    assert_eq!(a, &mut [1, 2, 3]);
    assert_eq!(b, &mut [4, 5, 6]);
}
```

Standart kutubxonadagi `split_at_mut` ana shu pattern bilan implement qilingan.

## Listing 20-5 — Safe Rust Bilan Bo'lmaydi

```rust
fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    assert!(mid <= len);
    (&mut values[..mid], &mut values[mid..])
}
```

Compile error:

```text
error[E0499]: cannot borrow `*values` as mutable more than once at a time
```

Sabab: borrow checker ikki slice non-overlapping ekanligini bilmaydi, faqat ikki marta `&mut values` qilingan deb hisoblaydi.

## Listing 20-6 — Unsafe Bilan To'g'ri Implementatsiya

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();

    assert!(mid <= len);

    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}

fn main() {
    let mut vector = vec![1, 2, 3, 4, 5, 6];
    let (left, right) = split_at_mut(&mut vector, 3);
}
```

## Tahlil

1. **`values.as_mut_ptr()`** — `*mut i32` raw pointer oladi.
2. **`assert!(mid <= len)`** — invariantni isbotlaydi: `mid` slice ichida.
3. **`slice::from_raw_parts_mut(ptr, mid)`** — `[ptr, ptr+mid)` oraliqdan slice quradi (unsafe).
4. **`ptr.add(mid)`** — pointer arifmetikasi (unsafe), `[ptr+mid, ptr+len)` boshlanish nuqtasi.
5. **Funksiya `unsafe` emas** — caller hech qanday majburiyat olmaydi.

## Nima Uchun Bu [[safe-abstraction|Safe Abstraction]]?

- **`mid <= len`** — assertion bilan tekshiriladi.
- **`ptr` valid** — `values` slice'dan kelgan, allocate qilingan xotira.
- **Ikki slice non-overlapping** — `[..mid]` va `[mid..]` to'qnashmaydi.
- **Lifetime'lar to'g'ri** — qaytadigan slice'lar `values` lifetime'iga bog'liq (compiler infer qiladi).

Hech bir invariant caller tomonidan buzilishi mumkin emas — har qanday `&mut [i32]` va har qanday `usize` bilan chaqirsa ham xavfsiz (yomon argumentda panic, UB emas).

## Listing 20-7 — Anti-Pattern

```rust
let address = 0x01234usize;
let r = address as *mut i32;
let values: &[i32] = unsafe { slice::from_raw_parts_mut(r, 10000) };
```

`values` ishlatilganda — UB. Bu safe abstraction emas, chunki:
- `0x01234` valid manzil emas.
- 10000 ta `i32` joylashishi noma'lum.
- Caller `values` ni ko'rgani — segfault.

[[miri|Miri]] aniq aniqlaydi: "pointer must be dereferenceable for 40000 bytes."

## Related

- [[safe-abstraction|safe abstraction]] — pattern
- [[raw-pointer|raw pointer]] — `*mut i32`
- [[unsafe-rust|unsafe Rust]]
- [[slices]] — slice data layout
- [[borrow-checker|borrow checker]] — nima uchun safe Rust qabul qilmaydi
- [[invariants]]
- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
