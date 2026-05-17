---
title: "Thread and Async Combined"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, threads, example]
source_count: 1
---

# Thread and Async Combined

## Description

Thread (blocking produce) va async (non-blocking consume) birgalikda ishlash namunasi. CPU-bound yoki blocking ish threadda, I/O-bound yoki event-driven ish async'da.

## Source

[[wiki/sources/17-6-futures-tasks-and-threads]] — Listing 17-25.

## Kod

```rust
use std::{thread, time::Duration};

fn main() {
    let (tx, mut rx) = trpl::channel();

    // Thread: sinxron, blocking produce (CPU-bound ish uchun yaxshi)
    thread::spawn(move || {
        for i in 1..11 {
            tx.send(i).unwrap();
            thread::sleep(Duration::from_secs(1));  // blocking sleep
        }
        // tx dropped → channel yopiladi
    });

    // Async: non-blocking consume
    trpl::block_on(async {
        while let Some(message) = rx.recv().await {  // .await — non-blocking
            println!("{message}");
        }
    });
}
// Output: 1, 2, 3, ..., 10 (1 sekundlik oraliqda)
```

## Qanday ishlaydi

```
Thread           │  Async runtime
─────────────────┼─────────────────
tx.send(1)       │
thread::sleep(1s)│  rx.recv().await → 1 keldi, print
tx.send(2)       │
thread::sleep(1s)│  rx.recv().await → 2 keldi, print
...              │  ...
tx dropped       │  rx.recv().await → None → loop tugadi
```

Thread blocking sleep qilsa ham, async runtime bloklanmaydi — `rx.recv().await` future'si `Pending` qaytaradi va runtime boshqalarga o'tadi (bu misolda boshqa future yo'q).

## Real dunyo misoli

```
Video encoding (CPU-bound):
  thread::spawn(move || { encode_video(frames, tx); })

UI yangilanish (event-driven):
  trpl::block_on(async {
      while let Some(progress) = rx.recv().await {
          update_progress_bar(progress);
      }
  });
```

## Concepts Used

- [[threads]] — OS thread, blocking produce
- [[async-channel|async channel]] — thread'dan async'ga xabar
- [[async-runtime|async runtime]] — non-blocking consume
- [[cpu-bound|CPU-bound]] → thread afzal
- [[io-bound|I/O-bound]] → async afzal

## Related Pages

- [[wiki/chapters/17-6-futures-tasks-and-threads]]
- [[channel-basic-messages]] — faqat thread version (16-bob)
