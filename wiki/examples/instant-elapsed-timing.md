---
title: "Instant elapsed timing"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, time, instant, duration]
source_count: 1
---

# Instant elapsed timing

Code segment qancha vaqt olganini o'lchash uchun `Instant` ishlatiladi, `SystemTime` emas.

```rust
use std::time::{Duration, Instant};

fn main() {
    let start = Instant::now();

    std::thread::sleep(Duration::from_millis(25));

    let took = start.elapsed();
    println!("elapsed: {took:?}");
}
```

## Related

- [[instant|Instant]]
- [[duration|Duration]]
- [[monotonic-clock|monotonic clock]]
- [[system-time|SystemTime]]

