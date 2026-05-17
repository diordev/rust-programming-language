---
title: "sender Option::take shutdown"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, option, channels, shutdown]
source_count: 1
---

# sender Option::take shutdown

21.3 shutdown signalini channel'ning sending uchini yopish orqali beradi.

```rust
pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());
        // keyin workerlarni join qilish
    }
}
```

## Why It Matters

`drop(self.sender.take())` ikki ish qiladi: `Sender<Job>` ownership bilan olinadi va darhol drop qilinadi. Shundan keyin worker tarafdagi `recv()` `Err` qaytarib, shutdown signalini oladi.

## Related

- [[option]]
- [[channels]]
- [[graceful-shutdown]]
- [[thread-pool]]
