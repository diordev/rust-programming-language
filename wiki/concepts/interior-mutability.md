---
title: "interior mutability"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-17
tags: [rust, smart-pointers, refcell, borrowing, patterns]
source_count: 3
---

# interior mutability

## Short Definition

Immutable referenslar orqali ham ma'lumotni o'zgartira oladigan dizayn patterni. Rust da `RefCell<T>` va `Cell<T>` orqali amalga oshiriladi.

## Why It Matters

Rustning oddiy borrow qoidalari: immutable referens bo'lsa, hech qanday mutable referens bo'lishi mumkin emas. Bu qoida ba'zi to'g'ri dasturlash patternlarini, masalan mock objectlarni, imkonsiz qilib qo'yadi. Interior mutability shu holatlarda escape hatch beradi.

## Mental Model

Tashqi dunyo ob'ektni "o'zgarmas" ko'radi. Lekin ob'ekt ichida, masalan, hisob yurituvi, log to'plami, cache kabi internal state o'zgarishi kerak. Interior mutability "tashqaridan muzlatilgan, ichkaridan harakatdagi" tuzilmalarni ifodalaydi.

Bu pattern bitta implementatsiyaga yopishmaydi: [[cell-t|Cell<T>]] whole-value replacement beradi, [[refcell-t|RefCell<T>]] esa borrow-based mutationni runtime'da tekshiradi.

## Syntax and Examples

```rust
use std::cell::RefCell;

struct Logger {
    log: RefCell<Vec<String>>,  // ichki state
}

impl Logger {
    fn record(&self, msg: &str) {
        // &self immutable, lekin log o'zgaradi
        self.log.borrow_mut().push(msg.to_string());
    }

    fn count(&self) -> usize {
        self.log.borrow().len()
    }
}
```

`Rc<RefCell<T>>` — shared + mutable pattern:

```rust
use std::rc::Rc;
use std::cell::RefCell;

let shared_val = Rc::new(RefCell::new(0));
let clone1 = Rc::clone(&shared_val);
let clone2 = Rc::clone(&shared_val);

*clone1.borrow_mut() += 5;
*clone2.borrow_mut() += 3;
println!("{}", shared_val.borrow()); // 8
```

## Common Mistakes

- Interior mutability = borrow qoidalarini aylanib o'tish deb o'ylash. Yo'q — qoidalar baribir kuchda, lekin runtime da tekshiriladi.
- `RefCell<T>` multithreaded — yo'q, faqat single-threaded. Ko'p oqimli uchun `Mutex<T>` ishlatiladi — u runtime o'rniga locking mexanizmi bilan interior mutability'ni thread-safe qiladi.

## `RefCell<T>` vs `Mutex<T>` taqqoslash

| | `RefCell<T>` | `Mutex<T>` |
|---|---|---|
| Thread-safety | Single-threaded | Multi-threaded |
| Borrow checking | Runtime (panic) | Lock orqali (block) |
| `Sync` | ✗ | ✓ |
| Kombinatsiya | `Rc<RefCell<T>>` | `Arc<Mutex<T>>` |

## Related Concepts

- [[refcell-t|RefCell<T>]] — asosiy implementatsiya
- [[cell-t|Cell<T>]] — whole-value replacement bilan yengilroq variant
- [[mutex-t|Mutex<T>]] — thread-safe analog
- [[rc-t|Rc<T>]] — ko'p egali variant bilan kombinatsiya
- [[arc-t|Arc<T>]] — thread-safe ko'p egalilik
- [[borrowing]] — qoidalar runtime da ham bir xil
- [[unsafe-rust]] — `RefCell<T>` ichkarida unsafe code
- [[threads]] — multithreaded alternativ uchun
- [[send-trait|Send trait]] — `RefCell<T>` Send emas, `Mutex<T>` Send + Sync

## Sources

- [[15-5-refcell-t-and-the-interior-mutability-pattern]]
- [[16-3-shared-state-concurrency]]
- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
