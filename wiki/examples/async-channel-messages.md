---
title: "Async Channel Messages"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, channel, example]
source_count: 1
---

# Async Channel Messages

## Description

Async channel orqali xabar almashish: bitta sender (Listing 17-11, 17-12), keyin ikki sender (Listing 17-13). `async move` bilan channel yopilishini boshqarish.

## Source

[[wiki/sources/17-2-applying-concurrency-with-async]] — Listing 17-9 dan 17-13 gacha.

## Bitta sender (Listing 17-12)

```rust
use std::time::Duration;

fn main() {
    trpl::block_on(async {
        let (tx, mut rx) = trpl::channel();

        let tx_fut = async move {    // tx moved — blok tugaganda dropped
            let vals = vec![
                String::from("hi"),
                String::from("from"),
                String::from("the"),
                String::from("future"),
            ];
            for val in vals {
                tx.send(val).unwrap();
                trpl::sleep(Duration::from_millis(500)).await;
            }
            // tx bu yerda dropped → channel yopiladi
        };

        let rx_fut = async {
            while let Some(value) = rx.recv().await {
                println!("received '{value}'");
            }
            // tx dropped bo'lgandan keyin None → loop tugaydi
        };

        trpl::join(tx_fut, rx_fut).await;
    });
}
```

**Nima uchun `async move`?** `tx` dropped bo'lsa channel yopiladi → `rx.recv()` `None` qaytaradi → `while let` tugaydi → dastur normal tugaydi.

## Ikki sender (Listing 17-13)

```rust
fn main() {
    trpl::block_on(async {
        let (tx, mut rx) = trpl::channel();
        let tx1 = tx.clone();

        let tx1_fut = async move {    // tx1 moved
            let vals = vec!["hi", "from", "the", "future"];
            for val in vals {
                tx1.send(val.to_string()).unwrap();
                trpl::sleep(Duration::from_millis(500)).await;
            }
        };

        let rx_fut = async {
            while let Some(value) = rx.recv().await {
                println!("received '{value}'");
            }
        };

        let tx_fut = async move {    // tx moved
            let vals = vec!["more", "messages", "for", "you"];
            for val in vals {
                tx.send(val.to_string()).unwrap();
                trpl::sleep(Duration::from_millis(1500)).await;
            }
        };

        trpl::join!(tx1_fut, tx_fut, rx_fut);  // 3 ta future — join! makro
    });
}
```

Channel yopilish sharti: `tx` **va** `tx1` — ikkalasi ham dropped bo'lishi kerak.

## Muammo: async move bo'lmasa (Listing 17-10)

```rust
// BU TO'G'RI ISHLAMAYDI:
async {
    for val in vals {
        tx.send(val).unwrap();
        trpl::sleep(500ms).await;    // await bor, lekin...
    }
    while let Some(v) = rx.recv().await { println!("{v}"); }
    // Muammo 1: barcha sleep'lar oldin tugaydi, keyin receive boshlanadi
    // Muammo 2: tx outer scope'da — channel hech yopilmaydi
}
```

## Concepts Used

- [[async-channel|async channel]] — trpl::channel, async recv
- [[async-move-block|async move block]] — tx ownership o'tkazish
- [[async-join|async join]] — trpl::join, trpl::join!
- [[message-passing|message passing]] — concurrent xabar almashish

## Related Pages

- [[wiki/chapters/17-2-applying-concurrency-with-async]]
- [[channel-basic-messages]] — thread'li ekvivalent (16-bob)
