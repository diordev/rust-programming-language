---
title: "16.2. Transfer Data Between Threads with Message Passing"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, channels, mpsc]
source_count: 1
source_path: "raw/books/the_rust_programming_language/16.2. Transfer Data Between Threads with Message Passing.md"
---

# 16.2. Transfer Data Between Threads with Message Passing

## Source

- **Fayl:** `raw/books/the_rust_programming_language/16.2. Transfer Data Between Threads with Message Passing.md`
- **URL:** https://doc.rust-lang.org/stable/book/ch16-02-message-passing.html

## Detailed Summary

### Message-passing falsafasi

Go tilidan kelgan mashhur slogan: *"Do not communicate by sharing memory; instead, share memory by communicating."* Threadlar yoki actorlar bir-biriga **xabar yuborish** orqali muloqot qiladi — shared mutable state'ning ko'p xatolarini bartaraf qiladi.

### Channels

Rust standart kutubxonasi `std::sync::mpsc` modulida channel implement qiladi. Channel ikki yarimga ega:
- **Transmitter** (yuboruvchi, `tx`) — daryoning yuqori qismi
- **Receiver** (qabul qiluvchi, `rx`) — daryoning quyi qismi

Channel **yopiladi** agar `tx` yoki `rx` yarmi drop bo'lsa.

`mpsc` = **multiple producer, single consumer**: bir nechta `tx` bo'lishi mumkin (clone orqali), lekin faqat bitta `rx`.

### `mpsc::channel`

```rust
use std::sync::mpsc;

let (tx, rx) = mpsc::channel(); // tuple destructuring
```

`(transmitter, receiver)` tuple qaytaradi.

### `tx.send(val)`

```rust
thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();
});
```

- `tx`ni thread'ga `move` orqali o'tkazish kerak.
- `send` ownership oladi — yuborilgandan keyin `val`ni ishlatib bo'lmaydi (E0382).
- `Result<T, E>` qaytaradi — `Err` agar receiver drop bo'lsa.

### `rx.recv()` va `rx.try_recv()`

```rust
let received = rx.recv().unwrap();
```

| Metod | Xulq |
|-------|------|
| `recv` | Block qiladi, qiymat kelguncha kutadi |
| `try_recv` | Block qilmaydi, `Result` darhol qaytaradi |

`try_recv` polling pattern uchun — thread boshqa ish qila olsa.

### Ownership transfer (E0382)

```rust
tx.send(val).unwrap();
println!("val is {val}"); // E0382: borrow of moved value
```

`send` qiymat ownership'ini channel'ga ko'chiradi. Bu **compile-time** himoya: yuborilgan qiymat boshqa thread tomonidan modify yoki drop qilinishi mumkin.

### Iterator pattern

```rust
for received in rx {
    println!("Got: {received}");
}
```

`rx`ni iterator sifatida ishlatish — channel yopilganda iteratsiya tugaydi.

### Multiple producers — `tx.clone()`

```rust
let tx1 = tx.clone();
thread::spawn(move || tx1.send(...));
thread::spawn(move || tx.send(...));
```

Har bir clone alohida thread'ga beriladi. Receiver bir xil — barchasi ungaga yuboradi.

## Key Concepts

- [[channels]] — `std::sync::mpsc` (multiple producer, single consumer)
- [[message-passing]]
- [[threads]]
- [[ownership]]
- [[result|Result]]

## Code Examples

- [[channel-basic-messages]] — Listing 16-7..16-11 to'plami

## Exercises or Practice Ideas

1. Bitta channel yarating, bitta thread'dan main thread'ga `String` yuboring.
2. `recv` o'rniga `try_recv` ishlatib polling pattern yozing.
3. Bir nechta producer va bitta consumer bilan chat-like dastur tuzing.
4. `send` dan keyin qiymatni qayta ishlatishga urinib, E0382 ni o'qing.
5. Receiver drop bo'lsa `send` qaytaradigan `Err`ni handle qilib ko'ring.

## Questions Raised

- Bir nechta receiver kerak bo'lsa nima qilish kerak? (`crossbeam-channel` kabi external crate)
- Bounded vs unbounded channellar farqi? (`mpsc::sync_channel`)
- Async kontekstida channellar qanday ishlaydi? (ch17)

## Links To Update

- [[wiki/chapters/16-fearless-concurrency]]
- [[concurrency]]
- [[index]]
