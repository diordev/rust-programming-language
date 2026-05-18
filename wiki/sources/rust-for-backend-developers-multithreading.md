---
title: "Многопоточность - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, backend, source, multithreading, advance]
source_count: 1
---

# Многопоточность - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/52. Многопоточность.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/multithreading.html

## Detailed Summary

Bu source Rust concurrency qatlamini ikki yo'lga ajratadi: OS thread'lariga tayangan traditional multithreading va user-space runtime ustidagi async execution. Bobning real fokusi birinchisi - `std::thread`, `std::sync`, atomics, TLS, va channels.

`thread::spawn` signature'i manbaning asosiy contract'ini ochadi: closure `FnOnce + Send + 'static`, natija esa `Send + 'static`. Demak oddiy "thread ochish" sintaktik operatsiya emas; u ownership transfer, lifetime boundary, va thread-safety markerlarini bir joyga jamlaydi. `JoinHandle<T>` esa thread completion va result retrieval nuqtasi. Source anonymous function va ordinary function bilan `spawn` ko'rsatadi, keyin `JoinHandle::join()` orqali natijalarni yig'adi.

Keyingi nozik qatlam `std::thread::Builder`. Source to'g'ri signal beradi: `thread::spawn` aslida default builder shortcut'i. Builder thread name va stack size kabi parametrlarga needs-based kirish beradi. Bu production debugging va deep recursion workloadlari uchun amaliy.

`Send` bo'limi ownership'ni boshqa thread'ga ko'chirish xavfsizligi haqida. `String` move bilan ishlaydi, `Rc<String>` esa ishlamaydi, chunki reference count atomic emas. Source compile error sifatida `E0277` ko'rsatadi. Yana bir muhim nuance: raw pointer field'li struct avtomatik `Send` bo'lmaydi, lekin `unsafe impl Send` bilan qo'lda va'da berish mumkin. Wiki uchun bu "yozsa bo'ladi" degan tavsiya emas; bu unsafe invariant butunlay author zimmasiga o'tishini ko'rsatadi.

`Sync` bo'limi `T: Sync` iff `&T: Send` mental modelini beradi. `String` shared immutable reference bilan ishlaydi, `Cell<T>` esa ishlamaydi. Source `static` value orqali demo qiladi, lekin durable takeaway `Cell<T>` thread-shared mutation uchun emasligida. `Cell<T>`ni thread-local storage ichida ishlatish xavfsiz, lekin umumiy `static`da emas.

Synchronization primitivlari bo'limida `Mutex`, `RwLock`, `Condvar`, va `Barrier` bir-biridan ajratiladi. `Mutex<T>` shared mutable state uchun basic gate. Source lock scope'ini alohida block bilan cheklash, `Arc<Mutex<T>>` bilan worker'lar orasida ulashish, va counter increment misolini ko'rsatadi. Eng muhim amaliy caveat `MutexGuard` lifetime'i: `let value = mutex.lock().unwrap().pop();` expression oxirida unlock bo'ladi, lekin `if let Some(x) = mutex.lock().unwrap().pop()` ichida guard butun `if let` scope'i davomida yashaydi. Bu compile bo'ladigan, lekin operational jihatdan serializatsiya yoki deadlock xavfi tug'diradigan detail.

Poisoned mutex bo'limi source'dagi eng foydali amaliy qismlardan biri. Thread panic paytida lock ushlab turgan bo'lsa, keyingi `.lock()` `PoisonError` qaytaradi. Lekin bu absolute data loss signal emas: `into_inner()` bilan guard'ni olish, state'ni tekshirish, va kerak bo'lsa `clear_poison()` qilish mumkin. Demak poison semantics "mutlaq taqiqlash" emas, "manual review required" signalidir.

`RwLock` `Mutex`dan farqli ravishda read concurrency beradi: ko'p readers yoki bitta writer. Poison semantics faqat write lock panic bo'lganda ishga tushadi. `Condvar` esa mutex bilan juft ishlaydi; `wait()` guard'ni consume qilib, mutex'ni vaqtincha bo'shatadi va notify'dan keyin yangi guard bilan qaytadi. `Barrier` esa worker batch synchronization uchun ishlatiladi: barcha thread bir nuqtaga yetmaguncha hech kim oldinga o'tmaydi.

`thread::scope` source'dagi muhim boundary correction. Oddiy `spawn` local variable reference'larini closure ichida ushlay olmaydi, chunki compiler `.join()` call'larini lifetime proof sifatida qabul qilmaydi. Scoped threads aynan shu holat uchun: ular parent scope ichida yaratiladi, local value'larni `&` bilan borrow qila oladi, va scope oxirida avtomatik join qilinadi. Source barrier misolini scoped thread bilan qayta yozib, `Arc` va manual join kodini kamaytiradi.

Atomic bo'limi primitive types uchun lock-free wrapper'larni ko'rsatadi. `AtomicI32` counter misolida `fetch_add` va `load` ishlatiladi. Bu yerda source'ning o'zida typo bor: code natijasi `1000000`, prose esa bir joyda `100000` deb yozadi. Wiki'da code va loop count asosida to'g'ri natija `1_000_000` deb saqlanadi.

Memory ordering explanation bu source'dagi eng ehtiyotkor o'qilishi kerak bo'lgan qism. Bu `std::cmp::Ordering` emas, `std::sync::atomic::Ordering`. `Relaxed` bitta atomik ichidagi race-free consistency beradi, lekin turli atomiklar orasidagi relative order'ni kafolatlamaydi. `Release`/`Acquire` publish-observe pattern uchun, `AcqRel` read-modify-write operatsiyalar uchun, `SeqCst` esa eng kuchli global tartib signalidir. Source backendlar odatda statistic counterlar uchun `Relaxed` ishlatishini aytadi; wiki'da bu general law emas, shu source contextidagi practical heuristic sifatida saqlanadi.

`compare_exchange` bo'limi CAS loop'ni ko'rsatadi. `load + store` oddiy kombinatsiyasi concurrent increment uchun yetmaydi, chunki ular orasida boshqa thread value'ni o'zgartirib yuborishi mumkin. CAS esa "faqat hali ham kutgan qiymatim turgan bo'lsa yoz" semantikasi beradi. Shuning uchun failure path eski actual value'ni qaytaradi va loop yangidan hisoblaydi.

Thread-local storage bo'limi `thread_local!` macro va `Cell<u32>` bilan per-thread state modelini ko'rsatadi. Muhim farq: `Cell<T>` shared `static`da `Sync` emas, lekin TLS ichida har thread o'z nusxasiga ega bo'lgani uchun xavfsiz. Source web server use-case sifatida request/session context'ni keltiradi; wiki'da bu convenience pattern sifatida yoziladi, global implicit dependency sifatida emas.

Channels bo'limi `mpsc::channel` va `mpsc::sync_channel`ni ajratadi. `channel()` unbounded queue, `sync_channel(bound)` esa bounded queue bo'lib, to'lsa sender'ni block qiladi va backpressure beradi. Source'dagi `Element::Finish` enum demo'sida `Finish` variant e'lon qilingan, lekin hech qachon yuborilmagan. Loop amalda sender drop bo'lganda `recv()` `Err` qaytarishi orqali tugaydi. Demak bu example explicit sentinel emas, channel-close shutdown patternini ko'rsatadi.

`crossbeam-channel` va nightly `std::sync::mpmc` haqidagi eslatma vaqtga bog'liq. Wiki'da bu current stable API claim sifatida emas, source claim sifatida qayd qilinadi.

## Key Concepts

- [[threads]]
- [[thread-builder]]
- [[join-handle]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[mutex-t|Mutex<T>]]
- [[poisoned-mutex]]
- [[rwlock|RwLock]]
- [[condvar|Condvar]]
- [[barrier|Barrier]]
- [[scoped-threads]]
- [[atomic-types]]
- [[atomic-memory-ordering]]
- [[compare-exchange]]
- [[thread-local-storage]]
- [[channels]]
- [[sync-channel]]

## Code Examples

```rust
let t = std::thread::Builder::new()
    .name("my_thread".to_string())
    .stack_size(8192)
    .spawn(|| {
        println!("{:?}", std::thread::current().name());
    })
    .unwrap();
t.join().unwrap();
```

```rust
use std::{sync::{Arc, Mutex}, thread};

let counter = Arc::new(Mutex::new(0));
let t = {
    let counter = Arc::clone(&counter);
    thread::spawn(move || *counter.lock().unwrap() += 1)
};
t.join().unwrap();
```

```rust
use std::sync::atomic::{AtomicI32, Ordering};

let a = AtomicI32::new(0);
a.fetch_add(1, Ordering::Relaxed);
assert_eq!(a.load(Ordering::Relaxed), 1);
```

```rust
thread_local! {
    static NUM: std::cell::Cell<u32> = const { std::cell::Cell::new(0) };
}
```

```rust
let (tx, rx) = std::sync::mpsc::sync_channel::<i32>(3);
tx.send(1).unwrap();
```

## Exercises or Practice Ideas

- `Rc<String>`ni thread'ga yuborib `E0277`ni ko'ring, keyin `Arc<String>`ga o'ting.
- `Mutex<Vec<i32>>` misolida `let last = lock.pop()` va `if let Some(x) = lock.pop()` scope farqini yozib ko'ring.
- `thread::scope` bilan local `String`ni borrow qiladigan va oddiy `spawn` bilan yiqiladigan ikki misolni taqqoslang.
- `AtomicI32` counter'ni avval `fetch_add`, keyin CAS loop bilan yozib ko'ring.
- `sync_channel(1)` bilan producer/consumer backpressure demo qiling.

## Questions Raised

- `Mutex<T>` yetadimi yoki read-heavy workload uchun `RwLock<T>` kerakmi?
- `Relaxed` haqiqatan yetarlimi yoki publish/observe semantics kerakmi?
- Per-request context uchun TLS ishlatish architecture'ni soddalashtiradimi yoki implicit dependency kiritadimi?
- Channel shutdown'ni explicit message bilanmi, sender close bilanmi boshqarish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-multithreading]]
- [[threads]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[mutex-t|Mutex<T>]]
- [[poisoned-mutex]]
- [[rwlock|RwLock]]
- [[condvar|Condvar]]
- [[barrier|Barrier]]
- [[scoped-threads]]
- [[atomic-types]]
- [[atomic-memory-ordering]]
- [[compare-exchange]]
- [[thread-local-storage]]
- [[channels]]
- [[sync-channel]]
