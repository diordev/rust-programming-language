---
title: "Async Concurrent Tasks"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency, example]
source_count: 1
---

# Async Concurrent Tasks

## Description

Ikki xil usulda concurrent async task: `spawn_task` + handle.await (Listing 17-6, 17-7) va `trpl::join` bilan anonymous future'lar (Listing 17-8). Thread-based ekvivalent (16-bob) bilan solishtirish.

## Source

[[wiki/sources/17-2-applying-concurrency-with-async]] — Listing 17-6, 17-7, 17-8.

## Variant 1: spawn_task + handle (Listing 17-7)

```rust
use std::time::Duration;

fn main() {
    trpl::block_on(async {
        let handle = trpl::spawn_task(async {
            for i in 1..10 {
                println!("hi number {i} from the first task!");
                trpl::sleep(Duration::from_millis(500)).await;
            }
        });

        for i in 1..5 {
            println!("hi number {i} from the second task!");
            trpl::sleep(Duration::from_millis(500)).await;
        }

        handle.await.unwrap();
    });
}
```

- `spawn_task` — mustaqil task yaratadi
- `handle.await.unwrap()` — birinchi task tugashini kutadi
- `handle.await` bo'lmasa main blok tugaganda spawned task ham to'xtaydi

## Variant 2: trpl::join bilan (Listing 17-8)

```rust
fn main() {
    trpl::block_on(async {
        let fut1 = async {
            for i in 1..10 {
                println!("hi number {i} from the first task!");
                trpl::sleep(Duration::from_millis(500)).await;
            }
        };

        let fut2 = async {
            for i in 1..5 {
                println!("hi number {i} from the second task!");
                trpl::sleep(Duration::from_millis(500)).await;
            }
        };

        trpl::join(fut1, fut2).await;
    });
}
```

- `spawn_task` kerak emas — anonymous future'lar yetarli
- `trpl::join` fair scheduling — deterministik navbatma-navbat output
- Har doim bir xil tartibda chiqadi (thread'lardan farqli)

## Asosiy farqlar

| | spawn_task | trpl::join |
|---|---|---|
| Task mustaqilligi | Mustaqil (handle bo'lmasa o'chishi mumkin) | Birgalikda boshqariladi |
| Output tartibi | Nondeterministik | Deterministik (fair) |
| Qo'llanish | Background task | Birgalikda tugashi kerak bo'lgan ishlar |

## Concepts Used

- [[async-task|async task]] — spawn_task, task handle
- [[async-join|async join]] — trpl::join, fair scheduling
- [[async-runtime|async runtime]] — block_on, task boshqarish

## Related Pages

- [[wiki/chapters/17-2-applying-concurrency-with-async]]
- [[thread-spawn-basic]] — thread'li ekvivalent (16-bob)
