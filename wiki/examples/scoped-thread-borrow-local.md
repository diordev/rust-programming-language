---
title: "Scoped thread borrows local"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, threads, scope, lifetimes]
source_count: 1
---

# Scoped thread borrows local

`thread::scope` local `String`ni `move` va `Arc`siz borrow qilishga ruxsat beradi.

```rust
fn main() {
    let s = String::from("Hello");

    std::thread::scope(|scope| {
        scope.spawn(|| {
            println!("{s}");
        });
    });
}
```

## Related

- [[scoped-threads]]
- [[threads]]
