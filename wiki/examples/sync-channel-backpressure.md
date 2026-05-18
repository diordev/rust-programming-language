---
title: "sync_channel backpressure"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, channels, sync-channel, backpressure]
source_count: 1
---

# sync_channel backpressure

Bounded channel to'lgach sender block bo'ladi.

```rust
use std::{sync::mpsc, thread, time::{Duration, Instant}};

fn main() {
    let (tx, rx) = mpsc::sync_channel::<i32>(3);

    let producer = thread::spawn(move || {
        for i in 0..5 {
            let start = Instant::now();
            tx.send(i).unwrap();
            println!("send took {} ms", start.elapsed().as_millis());
        }
    });

    thread::spawn(move || loop {
        thread::sleep(Duration::from_secs(1));
        if rx.recv().is_err() {
            break;
        }
    });

    producer.join().unwrap();
}
```

## Related

- [[sync-channel]]
- [[channels]]
