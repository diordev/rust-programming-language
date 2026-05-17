---
title: "16. Fearless Concurrency"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, channels, mutex, arc]
source_count: 5
---

# 16. Fearless Concurrency

## Learning Goal

Rustda concurrent kod yozishni ownership va type system kafolatlari bilan tushunish: threadlar yaratish, xavfsiz data almashinuvi, va compiler darajasidagi himoya mexanizmlari.

## Main Ideas

- **Concurrent** = dasturning qismlari mustaqil ishlaydi; **parallel** = bir vaqtda ishlaydi. Rust ikkalasini qo'llab-quvvatlaydi.
- Ownership + type system ko'p concurrency buglarini **compile time'da** tutadi — runtime debugging o'rniga kompilyator xato beradi. Bu "fearless concurrency" nomi bilan tanilgan.
- Rust **1:1 thread modeli**: har bir language thread bitta OS thread.
- Boshqa tillar (masalan, Erlang) faqat bitta yondashuv taklif qiladi; Rust moslashuvchan.
- **Message passing** ([[channels]]) va **shared state** ([[mutex-t]]) — ikki asosiy concurrency pattern. Rust ikkalasini qo'llab-quvvatlaydi va ownership system orqali ikkalasini xavfsiz qiladi.

## 16.0 — Fearless Concurrency falsafasi

Rust jamoasi avvalo memory safety va concurrency'ni alohida muammolar deb bilgan. Ammo ownership va type system ikkalasini ham qamrab olishi ma'lum bo'ldi: data race kabi muammolar compile time'da aniqlanadi, runtime'da emas.

Bob qamroviga kiruvchi mavzular:
1. Threadlar — `thread::spawn`
2. Message-passing — channels (`mpsc`)
3. Shared-state — `Mutex<T>`, `Arc<T>`
4. `Send` va `Sync` traitlari — user-defined typelarda concurrency kafolatlari

## 16.1 — Threadlar bilan parallel ishlash

### `thread::spawn`

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }
    // main tugadi → spawned ham to'xtaydi
}
```

Main thread tugaganda barcha spawned threadlar ham to'xtaydi — `join` bo'lmasa.

### `JoinHandle<T>` va `.join()`

```rust
let handle = thread::spawn(|| { /* ... */ });
// ... boshqa ish ...
handle.join().unwrap();
```

| Join joyi | Natija |
|-----------|--------|
| Loop**dan keyin** | Interleaved — parallel ishlaydi, spawned to'liq tugaydi |
| Loop**dan oldin** | Sequential — spawned tugaguncha main kutadi |

### `move` closures

Thread parent thread'dan data ishlatishi kerak bo'lganda Rust borrow qilishga urinadi, lekin thread'ning lifetime'i noma'lum — **E0373** chiqadi:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(|| println!("{v:?}")); // XATO: E0373
```

`move` bilan ownership o'tkaziladi:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(move || println!("{v:?}")); // OK
handle.join().unwrap();
```

`move` mantiqiy xatoni to'g'irlamaydi: agar asl scope ham qiymatni ishlatsa, `drop(v)` — **E0382** chiqadi. Ownership qoidalari threadlar uchun ham amal qiladi.

## 16.2 — Threadlar orasida xabar uzatish

Go tilidan kelgan slogan: *"Do not communicate by sharing memory; instead, share memory by communicating."* Rust [[channels]] orqali message-passing concurrency'ni qo'llab-quvvatlaydi.

### `mpsc::channel`

```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    tx.send(String::from("hi")).unwrap();
});

let received = rx.recv().unwrap();
println!("Got: {received}");
```

`mpsc` = **multiple producer, single consumer**. Channelning ikki yarmi: `tx` (transmitter) va `rx` (receiver).

### `send` ownership oladi

`tx.send(val)` qiymat ownership'ini channel'ga ko'chiradi. Yuborilgandan keyin asl scope'da `val`ni ishlatib bo'lmaydi — bu **compile-time** himoya (E0382).

### Iterator pattern va multiple producers

```rust
let tx1 = tx.clone();
thread::spawn(move || tx1.send(String::from("a")).unwrap());
thread::spawn(move || tx.send(String::from("b")).unwrap());

for received in rx { // rx iterator sifatida
    println!("Got: {received}");
}
```

`tx.clone()` yangi transmitter beradi. `rx` iterator sifatida ishlatilganda channel yopilganda iteratsiya tugaydi.

`recv()` block qiladi; `try_recv()` darhol qaytaradi — polling pattern uchun.

## 16.3 — Shared-State Concurrency

Channels = single ownership. Shared memory = **multiple ownership** — bir nechta thread bir xil ma'lumotga kiradi.

### `Mutex<T>` — mutual exclusion

```rust
use std::sync::Mutex;

let m = Mutex::new(5);
{
    let mut num = m.lock().unwrap(); // MutexGuard<i32>
    *num = 6;
} // lock avtomatik qo'yildi
```

`.lock()` `MutexGuard<T>` qaytaradi — `Deref` (ichki qiymat) va `Drop` (lock avtomatik qo'yish) implement qiladi. Agar lock tutayotgan thread panic qilsa — "poisoned mutex".

### Ko'p threadda: `Arc<Mutex<T>>`

`Mutex<T>` yolg'iz move qilib bo'lmaydi (E0382). `Rc<Mutex<T>>` `Send` emas (E0277). Yechim — `Arc<Mutex<T>>`:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];
for _ in 0..10 {
    let counter = Arc::clone(&counter); // atomic ref count
    handles.push(thread::spawn(move || {
        *counter.lock().unwrap() += 1;
    }));
}
for h in handles { h.join().unwrap(); }
println!("Result: {}", *counter.lock().unwrap()); // 10
```

`Mutex<T>` interior mutability beradi — `RefCell<T>`ning thread-safe analogи:

| Single-threaded | Multi-threaded |
|-----------------|----------------|
| `Rc<RefCell<T>>` | `Arc<Mutex<T>>` |

Deadlock xavfi: Rust uni compile time'da aniqlamaydi — dizayn masalasi.

## 16.4 — Send va Sync traitlari

Rust concurrency feature'lari aksariyat stdlib, til o'zi emas. **Istisno**: `Send` va `Sync` — `std::marker` traitlari, tilga o'rnatilgan.

### `Send` — ownership thread orasida

`Send` implement qilgan type'ning ownership'ini `thread::spawn`ga xavfsiz o'tkazish mumkin. Deyarli barcha type'lar `Send`. Istisno: `Rc<T>` (count atomic emas), raw pointerlar.

### `Sync` — immutable referens thread orasida

`T: Sync` ↔ `&T: Send`. `Mutex<T>: Sync`, `RefCell<T>` emas. `Rc<T>` emas.

### Manual implementation — unsafe

Marker trait — metodsiz. Composed type'lar avtomatik `Send`/`Sync`. Qo'lda implement qilish **unsafe Rust** talab qiladi.

## Concepts

- [[threads]] — `thread::spawn`, 1:1 model, lifecycle
- [[join-handle]] — `.join()` semantikasi, join joyi
- [[move-closures-threads]] — `move` va thread ownership transferi
- [[concurrency]] — fearless concurrency falsafasi
- [[channels]] — `mpsc::channel`, tx/rx, send/recv
- [[message-passing]] — Go-style concurrency falsafasi
- [[mutex-t|Mutex<T>]] — mutual exclusion, locking, MutexGuard
- [[arc-t|Arc<T>]] — atomic reference counting, thread-safe
- [[send-trait|Send trait]] — ownership thread orasida
- [[sync-trait|Sync trait]] — &T thread orasida
- [[data-race]] — data race nima va Rust qanday oldini oladi

## Examples

- [[thread-spawn-basic]] — Listing 16-1/16-2/16-3 taqqoslamasi
- [[channel-basic-messages]] — Listing 16-7..16-11 channel namunalari
- [[mutex-arc-counter]] — E0382 → E0277 → Arc<Mutex<T>> yechim progressi

## Exercises

1. `thread::spawn` ni `join` siz ishlatib, spawned thread to'liq ishlamasligini kuzating.
2. `JoinHandle`ni for loopdan oldin va keyin `join` qilib, chiqish tartibini solishtiring.
3. `move` siz closure bering — E0373 ni tushuntiring.
4. `move` + `drop(v)` — E0382 ni tushuntiring.
5. Uchta thread spawn qilib, har birini alohida join'lang.
6. Channel yarating va bitta thread'dan main'ga `String` yuboring.
7. `send` dan keyin qiymatni qayta ishlatib E0382 ni qabul qiling.
8. `tx.clone()` bilan ikki producer va bitta consumer tuzing.
9. Single-threaded `Mutex<T>` ishlatib lock/unlock semantikasini tushuning.
10. `Rc<Mutex<T>>` bilan E0277, `Arc<Mutex<T>>` bilan 10 thread counter tuzing.

## Review Questions

1. 1:1 thread modeli nima?
2. Nima uchun `thread::spawn` closurelari `move` talab qiladi?
3. `JoinHandle::join` qanday ishlaydi?
4. E0373 va E0382 farqi nima?
5. Main thread tugaganda spawned threadlarga nima bo'ladi?
6. Fearless concurrency nima uchun bunday nomlanadi?
7. `mpsc` qisqartmasi nimani anglatadi?
8. `recv` va `try_recv` farqi nimada?
9. `send` nima uchun ownership oladi?
10. `MutexGuard` nima uchun muhim?
11. Nima uchun `Rc<Mutex<T>>` threadda ishlamaydi?
12. `Arc<T>` va `Rc<T>` API farqi nima, nima uchun `Arc<T>` standart emas?
13. `Send` va `Sync` traitlari orasidagi munosabat nima?
14. `RefCell<T>/Rc<T>` va `Mutex<T>/Arc<T>` qaysi holatlarda ishlatiladi?

## Related Pages

- [[wiki/sources/16-fearless-concurrency]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[16-3-shared-state-concurrency]]
- [[16-4-extensible-concurrency-with-send-and-sync]]
- [[wiki/chapters/15-smart-pointers]]
- [[threads]]
- [[join-handle]]
- [[move-closures-threads]]
- [[channels]]
- [[message-passing]]
- [[mutex-t|Mutex<T>]]
- [[arc-t|Arc<T>]]
- [[concurrency]]
- [[data-race]]
- [[closures]]
- [[ownership]]

- [[threads]] — `thread::spawn`, 1:1 model, lifecycle
- [[join-handle]] — `.join()` semantikasi, join joyi
- [[move-closures-threads]] — `move` va thread ownership transferi
- [[concurrency]] — fearless concurrency falsafasi
- [[channels]] — `mpsc::channel`, tx/rx, send/recv
- [[message-passing]] — Go-style concurrency falsafasi
- [[send-trait]] — type'ni threadlar orasida o'tkazish xavfsizligi (stub, ch16.4)
- [[sync-trait]] — shared reference thread-safety (stub, ch16.4)
- [[data-race]] — data race nima va Rust qanday oldini oladi

## Examples

- [[thread-spawn-basic]] — Listing 16-1/16-2/16-3 taqqoslamasi
- [[channel-basic-messages]] — Listing 16-7..16-11 channel namunalari

## Exercises

1. `thread::spawn` ni `join` siz ishlatib, spawned thread to'liq ishlamasligini kuzating.
2. `JoinHandle`ni for loopdan oldin va keyin `join` qilib, chiqish tartibini solishtiring.
3. `move` siz closure bering — E0373 xatosini o'qib tushuntiring.
4. Listing 16-4 stsenariysini (`drop(v)` + `move`) yozib E0382 ni olish va izohlash.
5. Uchta thread spawn qilib, har birini alohida `JoinHandle` orqali join'lang.
6. Channel yarating va bitta thread'dan main thread'ga `String` yuboring.
7. `send` dan keyin qiymatni qayta ishlatishga urinib, E0382 ni qabul qiling.
8. `tx.clone()` orqali ikki producer va bitta consumer dasturni tuzing.

## Review Questions

1. 1:1 thread modeli nima, va alternativlari qanday?
2. Nima uchun `thread::spawn` closurelari `move` talab qiladi?
3. `JoinHandle::join` qanday ishlaydi va join joyi nima uchun muhim?
4. E0373 va E0382 o'rtasidagi farq nima?
5. Main thread tugaganda spawned threadlarga nima bo'ladi?
6. Fearless concurrency nima uchun bunday nomlanadi?
7. `mpsc` qisqartmasi nimani anglatadi?
8. `recv` va `try_recv` farqi nimada?
9. `send` nima uchun qiymat ownership'ini oladi?
10. Channel qachon yopiq deyiladi?

## Related Pages

- [[wiki/sources/16-fearless-concurrency]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/chapters/15-smart-pointers]]
- [[threads]]
- [[join-handle]]
- [[move-closures-threads]]
- [[channels]]
- [[message-passing]]
- [[concurrency]]
- [[data-race]]
- [[closures]]
- [[ownership]]
