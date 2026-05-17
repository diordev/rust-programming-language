---
title: "16.4. Extensible Concurrency with Send and Sync"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, send, sync, marker-traits]
source_count: 1
source_path: "raw/books/the_rust_programming_language/16.4. Extensible Concurrency with Send and Sync.md"
---

# 16.4. Extensible Concurrency with Send and Sync

## Source

- **Fayl:** `raw/books/the_rust_programming_language/16.4. Extensible Concurrency with Send and Sync.md`
- **URL:** https://doc.rust-lang.org/stable/book/ch16-04-extensible-concurrency-sync-and-send.html

## Detailed Summary

### Concurrency: stdlib vs til

Rustning ko'pchilik concurrency feature'lari standart kutubxona qismi, til o'zi emas. **Istisno**: `Send` va `Sync` — `std::marker` traitlari, tilning o'ziga o'rnatilgan (language-level).

### `Send` — ownership threadlar orasida o'tkazilishi mumkin

`Send` marker trait: type'ning ownership'ini threadlar orasida xavfsiz o'tkazish mumkin. Deyarli barcha Rust type'lari `Send` — istisno:

- **`Rc<T>`** — `Send` emas. `clone` qilinib boshqa threadga berilsa, ikkala thread reference count'ni bir vaqtda oshirishga urinar edi. `Rc<T>` single-threaded uchun mo'ljallangan, atomic overhead yo'q.
- **Raw pointerlar** — `Send` emas (ch20 da ko'riladi).

`Send` type'lardan tashkil topgan barcha type'lar **avtomatik** `Send`.

### `Sync` — bir nechta thread orqali reference xavfsiz

`Sync` marker trait: type'ga bir nechta thread immutable referens (`&T`) orqali xavfsiz kirishi mumkin. Boshqacha aytganda: `T: Sync` ↔ `&T: Send`.

`Sync` emas:
- **`Rc<T>`** — xuddi `Send` emas sababi bilan.
- **`RefCell<T>`** va `Cell<T>` oilasi — runtime borrow checking thread-safe emas.
- `Mutex<T>` esa `Sync` — bir nechta thread `&Mutex<T>` orqali kirishi xavfsiz (lock olish orqali).

`Sync` type'lardan tashkil topgan barcha type'lar **avtomatik** `Sync`.

### Manual implementation — unsafe

`Send` va `Sync` hech qanday metodsiz marker traitlar. Ularga composed type'lar uchun manual implement kerak emas — Rust avtomatik chiqaradi. Ammo primitiv qismlardan tashkil topgan yangi type uchun qo'lda implement qilish kerak bo'lsa — **unsafe Rust** talab qilinadi. Noto'g'ri implement etilgan type thread-safety kafolatlarini buzadi. Batafsil: "The Rustonomicon".

### Bob xulosasi

Rust concurrency: type system + borrow checker = compile time da data race va invalid reference yo'q. Compile qilingan kod bir nechta threadda xavfsiz ishlaydi. Ko'p concurrency yechimlari cratelarda — stdlib'dan tashqarida ham taraqqiy etadi.

## Key Concepts

- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[rc-t|Rc<T>]] — Send emas
- [[arc-t|Arc<T>]] — Send + Sync
- [[refcell-t|RefCell<T>]] — Sync emas
- [[mutex-t|Mutex<T>]] — Sync
- [[unsafe-rust|unsafe Rust]]
- [[concurrency]]

## Exercises or Practice Ideas

1. `Rc<T>` ni `thread::spawn`ga berishga urinib E0277 ni o'qing va nima uchun `Send` yo'qligini izohlang.
2. `Arc<T>` va `Rc<T>` API'ni solishtiring — farq faqat import va atomic kafolatlarda.
3. Qaysi type'lar `Send` va `Sync` implement qiladi, qaysilari qilmaydi — jadval tuzing.

## Questions Raised

- `Send` va `Sync`ni manual implement qilish qachon kerak bo'ladi? (`unsafe` kontekstida)
- Raw pointerlar nima uchun `Send` emas?

## Links To Update

- [[wiki/chapters/16-fearless-concurrency]]
- [[send-trait]]
- [[sync-trait]]
- [[index]]
