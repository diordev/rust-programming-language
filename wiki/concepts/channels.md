---
title: "Channels"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, concurrency, threads, channels, mpsc]
source_count: 4
---

# Channels

## Short Definition

Threadlar orasida ma'lumot uzatish vositasi. Channelda ikki yarim bor: **transmitter** (`tx`) va **receiver** (`rx`). Rust standart kutubxonasi `std::sync::mpsc` modulida implement qiladi.

## Why It Matters

Channels [[message-passing]] concurrency modelini amalga oshiradi. Shared state o'rniga threadlar bir-biriga xabar yuboradi.

## Mental Model

**mpsc** = *multiple producer, single consumer*. Bir nechta `tx` bo'lishi mumkin, lekin faqat bitta `rx`.

`mpsc`da amaliy shutdown signali ko'pincha shunday ishlaydi: barcha senderlar drop bo'lsa, `recv()` `Err` qaytaradi.

Source'dagi `Element::Finish` demo'sida `Finish` sentineli e'lon qilingan, lekin yuborilmagan. Loop amalda sender drop bo'lgani uchun tugaydi. Demak bu explicit shutdown message emas, channel-close shutdown.

## Syntax and Examples

### Asosiy channel yaratish va xabar yuborish

```rust
use std::sync::mpsc;

let (tx, rx) = mpsc::channel();
```

### `send` ownership oladi

```rust
let val = String::from("hi");
tx.send(val).unwrap();
```

### `rx` ni iterator sifatida ishlatish

```rust
for received in rx {
    println!("Got: {received}");
}
```

### `recv` vs `try_recv`

| Metod | Xulq |
|-------|------|
| `recv()` | Block qiladi |
| `try_recv()` | Darhol qaytadi |

### Multiple producers

```rust
let tx1 = tx.clone();
```

### Thread pool queue sifatida

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;

let (sender, receiver) = std::sync::mpsc::channel::<Job>();
let receiver = std::sync::Arc::new(std::sync::Mutex::new(receiver));
```

### Bounded channel: `sync_channel`

```rust
let (tx, rx) = std::sync::mpsc::sync_channel::<i32>(3);
```

`sync_channel` to'lsa `send()` block bo'ladi. Bu producer tomonda natural backpressure beradi.

### Sender drop orqali shutdown

```rust
drop(self.sender.take());
```

## Common Mistakes

- **`send` dan keyin qiymatni ishlatish**.
- **Receiver'ni clone qilishga urinish**.
- **Receiver'ni loop ichida ownership bilan workerlarga tarqatish**.
- **`recv` cheksiz block bo'lib qolishi**.
- **Shutdown semanticsni e'tiborsiz qoldirish**.

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
- [[sync-channel]]

## Sources

- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
