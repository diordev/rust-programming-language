---
title: "Job boxed FnOnce alias"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, type-alias, closures, thread-pool]
source_count: 1
---

# Job boxed FnOnce alias

Listings 21-19 va 21-20 closure job'larni qisqa va aniq ifodalash uchun type alias kiritadi.

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;

pub fn execute<F>(&self, f: F)
where
    F: FnOnce() + Send + 'static,
{
    let job = Box::new(f);
    self.sender.send(job).unwrap();
}
```

Worker tomoni:

```rust
loop {
    let job = receiver.lock().unwrap().recv().unwrap();
    job();
}
```

## Why It Matters

Bu yerda uchta g'oya birlashadi: `FnOnce` job bir marta bajarilishi uchun, `Send` uni worker threadga o'tkazish uchun, `'static` esa task worker ichida qancha yashashi noma'lum bo'lgani uchun.

## Related

- [[type-alias]]
- [[fn-traits|Fn traits]]
- [[send-trait|Send trait]]
- [[static-lifetime|static lifetime]]
- [[thread-pool]]
