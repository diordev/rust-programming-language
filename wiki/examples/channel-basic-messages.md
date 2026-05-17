---
title: "Channel basic messages"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, channels, threads]
source_count: 1
---

# Channel basic messages

`mpsc::channel` ning to'rt asosiy qo'llanish stsenariysi.

## Listing 16-7/16-8: Bitta xabar yuborish va qabul qilish

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

**Chiqish:** `Got: hi`

`rx.recv()` block qiladi — qiymat kelguncha kutadi.

## Listing 16-9: Ownership xatosi (E0382)

```rust
thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();
    println!("val is {val}"); // XATO: E0382
});
```

`send` `val` ownership'ini channel'ga ko'chiradi. Asl scope'da `val` endi yo'q.

## Listing 16-10: Bir nechta xabar va iterator

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];

        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx {
        println!("Got: {received}");
    }
}
```

**Chiqish (har sekundda bittadan):**
```
Got: hi
Got: from
Got: the
Got: thread
```

`rx` iterator sifatida ishlatilmoqda. Channel yopilganda (transmitter drop bo'lsa) iteratsiya tugaydi.

## Listing 16-11: Multiple producers via clone

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    let tx1 = tx.clone(); // ikkinchi transmitter
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread"),
        ];
        for val in vals {
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    thread::spawn(move || {
        let vals = vec![
            String::from("more"),
            String::from("messages"),
            String::from("for"),
            String::from("you"),
        ];
        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    });

    for received in rx {
        println!("Got: {received}");
    }
}
```

**Chiqish (tartib har xil bo'lishi mumkin):**
```
Got: hi
Got: more
Got: from
Got: messages
Got: for
Got: the
Got: thread
Got: you
```

`tx.clone()` yangi transmitter beradi — ikkalasi ham bir xil receiver'ga yuboradi. Bu *mpsc* (multiple producer, single consumer) modeli.

## Related

- [[channels]]
- [[message-passing]]
- [[threads]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
