---
title: "thread::spawn basic examples"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads]
source_count: 1
---

# thread::spawn basic examples

Uchta variant: join yo'q, join keyin (interleaved), join oldin (sequential).

## Listing 16-1: join yo'q — spawned thread to'liq ishlamaydi

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }
    // main tugadi → spawned thread ham to'xtaydi (5 dan keyin chiqmaydi)
}
```

**Tipik chiqish:** `spawned: 5` ga yetib, main tugatadi.

## Listing 16-2: join loopdan KEYIN — interleaved, lekin spawned to'liq ishlaydi

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap(); // spawned to'liq ishlaydi
}
```

**Tipik chiqish:** main va spawned chiqishlari aralash, lekin spawned 9 gacha boradi.

## Listing 16-3: join loopdan OLDIN — sequential

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    handle.join().unwrap(); // avval spawned tugasin

    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }
}
```

**Tipik chiqish:** avval spawned 1–9, keyin main 1–4. Aralashma yo'q.

## Taqqoslash jadvali

| Variant | Join joyi | Natija |
|---------|-----------|--------|
| 16-1 | Yo'q | Spawned to'liq ishlamaydi |
| 16-2 | Loopdan keyin | Interleaved, spawned to'liq ishlaydi |
| 16-3 | Loopdan oldin | Sequential: spawned → main |

## Related

- [[threads]]
- [[join-handle]]
- [[16-1-using-threads-to-run-code-simultaneously]]
