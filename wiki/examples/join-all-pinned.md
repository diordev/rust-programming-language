---
title: "join_all with Pinned Futures"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, pin, example]
source_count: 1
---

# join_all with Pinned Futures

## Description

Compile-time'da soni noma'lum bo'lgan future'larni `Vec` ichida to'plab `join_all` bilan bajarish. `pin!` macro bilan Pin muammosini hal qilish.

## Source

[[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]] — Listing 17-23, 17-24.

## Xato versiya (Listing 17-23)

```rust
// KOMPILYATSIYA BO'LMAYDI:
let futures: Vec<Box<dyn Future<Output = ()>>> =
    vec![Box::new(tx1_fut), Box::new(rx_fut), Box::new(tx_fut)];
trpl::join_all(futures).await;

// error[E0277]: `dyn Future<Output = ()>` cannot be unpinned
//   the trait `Unpin` is not implemented for `dyn Future<Output = ()>`
```

## To'g'ri versiya (Listing 17-24)

```rust
use std::pin::{Pin, pin};
use std::time::Duration;

fn main() {
    trpl::block_on(async {
        let (tx, mut rx) = trpl::channel();
        let tx1 = tx.clone();

        let tx1_fut = pin!(async move {
            let vals = vec!["hi", "from", "the", "future"];
            for val in vals {
                tx1.send(val.to_string()).unwrap();
                trpl::sleep(Duration::from_millis(500)).await;
            }
        });

        let rx_fut = pin!(async {
            while let Some(value) = rx.recv().await {
                println!("received '{value}'");
            }
        });

        let tx_fut = pin!(async move {
            let vals = vec!["more", "messages", "for", "you"];
            for val in vals {
                tx.send(val.to_string()).unwrap();
                trpl::sleep(Duration::from_millis(1500)).await;
            }
        });

        // Vec type: Pin<&mut dyn Future<Output = ()>>
        let futures: Vec<Pin<&mut dyn Future<Output = ()>>> =
            vec![tx1_fut, rx_fut, tx_fut];

        trpl::join_all(futures).await;
    });
}
```

## Tushuntirish

```
pin!(async { ... })
  ↓
Pin<&mut impl Future<Output=()>>
  ↓
Vec<Pin<&mut dyn Future<Output=()>>> — trait object + pinned
  ↓
join_all(...) — barchasi birgalikda
```

`pin!` — future'ni joriy stack frame'ga "mix qiladi". Future endi ko'chirilmaydi — `Vec`ga ishonch bilan solinishi mumkin.

## join! vs join_all solishtirish

```rust
// join! — compile-time soni ma'lum (3 ta):
trpl::join!(fut1, fut2, fut3);

// join_all — runtime dinamik (Vec):
let futures = vec![pin!(fut1), pin!(fut2), pin!(fut3)];
trpl::join_all(futures).await;
```

## Concepts Used

- [[join-all|join_all]] — dinamik future'lar to'plami
- [[pin|Pin va Unpin]] — async future'larni xotirada qotirish
- [[async-join|async join]] — join! bilan farq
- [[future|Future trait]] — trait object `dyn Future`

## Related Pages

- [[wiki/chapters/17-5-a-closer-look-at-the-traits-for-async]]
- [[async-channel-messages]] — bir xil future'lar, compile-time `join!` bilan
