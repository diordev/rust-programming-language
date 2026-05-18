---
title: "Cell<T>"
type: concept
status: active
created: 2026-05-17
updated: 2026-05-18
tags: [rust, smart-pointers, interior-mutability, cell]
source_count: 2
---

# Cell<T>

## Short Definition

`Cell<T>` single-threaded interior mutability wrapper'i. U borrow qaytarmaydi; value'ni butunlay `set`/`replace` qiladi yoki `Copy` bo'lsa `get` bilan nusxasini beradi.

## Why It Matters

Ba'zi holatda `RefCell<T>` darajasidagi runtime borrow checking ortiqcha. Agar sizga faqat whole-value replacement kerak bo'lsa, `Cell<T>` soddaroq va aniqroq signal beradi.

## Mental Model

`Cell<T>` "ichidagi qiymatni qarzga bermayman, lekin o'zim xavfsiz almashtirib beraman" deydi.

Muhim boundary: `Cell<T>` thread-shared state uchun emas, `Sync` emas. Lekin [[thread-local-storage]] ichida har thread o'z nusxasini olganda juda qulay.

## Syntax and Examples

```rust
use std::cell::Cell;

let cell = Cell::new(String::from("aaa"));
let old = cell.replace(String::from("bbb"));
cell.set(String::from("ccc"));
```

```rust
use std::{cell::Cell, rc::Rc};

let shared = Rc::new(Cell::new(1));
Rc::clone(&shared).set(5);
```

## Common Mistakes

- `Cell<T>` ichidan `&T` yoki `&mut T` olish mumkin deb o'ylash.
- `Cell<T>`ni thread-safe wrapper deb o'ylash.
- `static` shared state'da ishlatishga urinish.

## Related Concepts

- [[interior-mutability|interior mutability]]
- [[refcell-t|RefCell<T>]]
- [[rc-t|Rc<T>]]
- [[smart-pointers]]
- [[arc-t|Arc<T>]]
- [[thread-local-storage]]

## Sources

- [[wiki/sources/rust-for-backend-developers-smart-pointers]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
