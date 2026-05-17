---
title: "Channels"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, concurrency, threads, channels, mpsc]
source_count: 3
---

# Channels

## Short Definition

Threadlar orasida ma'lumot uzatish vositasi. Channelda ikki yarim bor: **transmitter** (yuboruvchi `tx`) va **receiver** (qabul qiluvchi `rx`). Rust standart kutubxonasi `std::sync::mpsc` modulida implement qiladi.

## Why It Matters

Channels [[message-passing]] concurrency modelini amalga oshiradi. Shared state o'rniga threadlar bir-biriga xabar yuboradi — bu ko'p concurrency xatolarining oldini oladi. Ownership rules `send` orqali avtomatik ravishda kafolatlanadi: yuborilgan qiymatga endi yuboruvchi thread'da kira olmaysiz.

## Mental Model

Daryo: yuqorida (`tx`) rezina o'rdakcha tashlaysiz, pastda (`rx`) kimdir uni oladi. Channel yopiq deyiladi agar `tx` yoki `rx` drop bo'lsa. **mpsc** = *multiple producer, single consumer* — bir nechta `tx` bo'lishi mumkin (clone orqali), lekin faqat bitta `rx`.

`mpsc`da amaliy shutdown signali ko'pincha shunday ishlaydi: barcha senderlar drop bo'lsa, `recv()` endi block qilib turmaydi va `Err` qaytaradi.

## Syntax and Examples

### Asosiy channel yaratish va xabar yuborish

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Got: {received}");
}
```

### `send` ownership oladi

```rust
thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();
    println!("val is {val}"); // E0382: send val'ni move qildi
});
```

`send` parametrini move qiladi — yuborilgandan keyin asl scope'da ishlatib bo'lmaydi. Bu compile-time da concurrency xatolarini oldini oladi.

### `rx` ni iterator sifatida ishlatish

```rust
for received in rx {
    println!("Got: {received}");
}
// Channel yopilganda iteratsiya tugaydi.
```

### `recv` vs `try_recv`

| Metod | Xulq |
|-------|------|
| `recv()` | Block qiladi, qiymat kelguncha kutadi. `Result<T, E>` qaytaradi |
| `try_recv()` | Block qilmaydi, darhol `Result` qaytaradi (`Ok(val)` yoki `Err`) |

`try_recv` thread'ning boshqa ishi bo'lsa foydali — pollingga moslashgan.

### Multiple producers — `tx.clone()`

```rust
let (tx, rx) = mpsc::channel();
let tx1 = tx.clone();

thread::spawn(move || tx1.send(String::from("hi")).unwrap());
thread::spawn(move || tx.send(String::from("more")).unwrap());

for received in rx {
    println!("Got: {received}");
}
```

Har bir clone alohida thread'ga berilishi mumkin — barchasi bir xil receiver'ga yuboradi.

### Thread pool queue sifatida

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;

let (sender, receiver) = mpsc::channel::<Job>();
let receiver = Arc::new(Mutex::new(receiver));
```

Bu pattern'da `sender` pool ichida saqlanadi, workerlar esa shared `receiver` orqali job oladi. `mpsc` std kanalida receiver clone qilinmaydi, shuning uchun workerlar orasida uni [[arc-t|Arc<Mutex<_>>]] bilan ulashish kerak.

### Sender drop orqali shutdown

```rust
drop(self.sender.take());

match receiver.lock().unwrap().recv() {
    Ok(job) => job(),
    Err(_) => break,
}
```

Bu yerda channel message tashishdan tashqari lifecycle coordination vazifasini ham bajaryapti: sender tugashi workerlar uchun "endi yangi job yo'q" degan signal.

## Common Mistakes

- **`send` dan keyin qiymatni ishlatish** — E0382. Channel ownership oladi.
- **Receiver'ni clone qilishga urinish** — `mpsc` faqat *single consumer*. Bir nechta receiver kerak bo'lsa, boshqa pattern (`crossbeam-channel` crati) zarur.
- **Receiver'ni loop ichida ownership bilan workerlarga tarqatish** — `E0382` beradi va design noto'g'ri ekanini ko'rsatadi.
- **`unwrap` ni production'da qoldirish** — `send`/`recv` `Result` qaytaradi; receiver/transmitter drop bo'lsa `Err` chiqadi.
- **`recv` cheksiz block bo'lib qolishi** — agar transmitter drop bo'lmasa va xabar kelmasa, thread to'xtab qoladi.
- **Shutdown semanticsni e'tiborsiz qoldirish** — senderni hech qachon drop qilmaslik `join()` paytida deadlockga o'xshash kutishga olib kelishi mumkin.

## Related Concepts

- [[message-passing]]
- [[threads]]
- [[concurrency]]
- [[move-closures-threads]]
- [[ownership]]
- [[result|Result]]
- [[iterators]]
- [[e0382-borrow-of-moved-value]]
- [[thread-pool]]
- [[job-queue]]
- [[worker-thread]]
- [[graceful-shutdown]]

## Sources

- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
