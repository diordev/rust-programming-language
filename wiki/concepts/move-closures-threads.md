---
title: "move closures (threads)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, closures, move]
source_count: 1
---

# move closures (threads)

## Short Definition

`thread::spawn`ga uzatiladigan closurelarda `move` kalit so'zi: closure muhitdagi qiymatlar ustida borrow emas, **ownership** oladi. Bu ownership ni parent threaddan spawned threadga o'tkazadi.

## Why It Matters

Thread qancha umr ko'rishini Rust bila olmaydi. Agar closure borrow qilsa, original qiymat borrow muddati tugashidan oldin drop bo'lishi mumkin → dangling reference. `move` bu muammoni to'liq bartaraf etadi.

## Mental Model

"Thread boshqa olamda yashaydi" — unga berilgan narsalarning **to'liq egasi** bo'lishi kerak, chunki qarz olgan narsa asl egasidan uzoq umr ko'rishi mumkin.

Umumiy `move` semantikasi ([[move-semantics]]) bilan farqi: bu yerda motivatsiya thread lifetime noma'lumligi, faqat closure ergonomikasi emas.

## Syntax and Examples

### E0373 — borrow bilan xato

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(|| {
        println!("{v:?}"); // XATO: E0373
    });
    // Rust v ni borrow qilishga urinadi,
    // lekin thread qancha yashashini bila olmaydi.

    handle.join().unwrap();
}
```

### To'g'ri — `move` bilan ownership o'tkazish

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("{v:?}"); // OK: v ownership thread'ga o'tdi
    });

    handle.join().unwrap();
}
```

### `move` mantiqiy xatoni to'g'irlamaydi — E0382

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("{v:?}"); // v thread'ga ko'chdi
    });

    drop(v); // XATO: E0382 — v allaqachon moved
    handle.join().unwrap();
}
```

`move` faqat ownership ni ko'chiradi. Agar kod asl scopeda qiymatni ham ishlatmoqchi bo'lsa, bu mantiqiy xato — `move` uni yashirmaydi, balki aniq ko'rsatadi.

## Common Mistakes

- **`move` hamma muammoni hal qiladi deb o'ylash**: `move` E0373 ni to'g'irlaydi, lekin agar asl thread ham qiymatni ishlatsa E0382 chiqadi.
- **`move` siz thread'dan data o'tkazishga urinish**: hamma vaqt `move` ishlatish kerak, aks holda E0373.
- **`clone()` o'rniga `move`**: ba'zan ikkalasida ham kerak bo'lsa, oldin `clone()` qiling, so'ng `move` bilan clone'ni o'tkazing.

## Related Concepts

- [[closures]]
- [[move-semantics]]
- [[threads]]
- [[ownership]]
- [[e0373-closure-may-outlive-current-function]]
- [[e0382-borrow-of-moved-value]]

## Sources

- [[16-1-using-threads-to-run-code-simultaneously]]
