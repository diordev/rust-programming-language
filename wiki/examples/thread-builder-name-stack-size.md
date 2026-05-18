---
title: "thread::Builder name and stack size"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, threads, builder]
source_count: 1
---

# thread::Builder name and stack size

`thread::spawn` defaultlarini chetlab o'tib, thread name va stack size berish.

```rust
fn main() {
    let handle = std::thread::Builder::new()
        .name("my_thread".to_string())
        .stack_size(8192)
        .spawn(|| {
            println!("thread name: {:?}", std::thread::current().name());
        })
        .unwrap();

    handle.join().unwrap();
}
```

## Related

- [[thread-builder]]
- [[threads]]
