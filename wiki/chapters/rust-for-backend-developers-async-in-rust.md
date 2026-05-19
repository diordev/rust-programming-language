---
title: "Rust for Backend Developers: Async in Rust"
type: chapter
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, async, futures, chapter]
source_count: 1
---

# Rust for Backend Developers: Async in Rust

## Learning Goal

Rust async kodining tashqi sintaksisi va ichki execution contractini bog'lash: `async fn` future yaratadi, `.await` composition beradi, `Future::poll` executor bilan gaplashadi, `Waker` esa tayyorlik signalini qaytaradi.

## Main Ideas

- `async fn` chaqiruvi `T` emas, `impl Future<Output = T>` qaytaradi.
- Natijani olish uchun future [[executor]] yoki [[async-runtime|async runtime]] orqali bajarilishi kerak.
- [[wiki/crates/futures|futures]] crate'idagi `futures::executor::block_on` eng sodda executor demo sifatida ishlatiladi.
- [[async-await|async/await]] sequential ko'rinadigan composition beradi; `.await` faqat async kontekstda ishlaydi.
- [[async-closure|Async closure]] va [[async-block|async block]] ham future yaratadi.
- [[future|Future]] trait `poll` metodi orqali executor bilan ishlaydi.
- [[task-context|Context]] future'ga [[waker|Waker]] beradi; `Waker::wake` executor'ga future qayta tayyorligini bildiradi.
- [[pin|Pin]] async state machine'ni ko'chirish xavfini Future API darajasida nazorat qiladi.

## Concepts

- [[async-await|async/await]]
- [[future|Future]]
- [[executor]]
- [[block-on|block_on]]
- [[wiki/crates/futures|futures]]
- [[async-closure|async closure]]
- [[async-block|async block]]
- [[polling]]
- [[poll-enum|Poll]]
- [[task-context|Context]]
- [[waker|Waker]]
- [[pin|Pin]]
- [[async-state-machine|async state machine]]
- [[fiber]]

## Examples

```rust
use futures::executor::block_on;

async fn get_1() -> i32 {
    1
}

fn main() {
    let result = block_on(get_1());
    println!("{result}");
}
```

```rust
async fn my_flow() -> i32 {
    let number = load_number().await;
    transform_number(number).await
}
```

## Exercises

- `async fn`ni desugared `fn -> impl Future<Output = T>` modeli bilan qayta yozing.
- User/address service misolini parallel emas, dependency-driven sequential flow sifatida tushuntiring.
- `Poll::Pending` qaytargan future waker saqlamasa nima bo'lishini yozing.
- Minimal executor kodida queue, pending set, va waker orasidagi data flowni chizing.

## Review Questions

- Nima uchun future lazy hisoblanadi?
- `.await` Rustda nima uchun postfix yoziladi?
- `block_on` nima uchun joriy thread'ni bloklaydi?
- `Context` va `Waker` bo'lmasa executor qanday muammoga duch keladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
- [[wiki/chapters/rust-for-backend-developers-4-async]]
- [[future]]
- [[async-await|async/await]]
- [[async-runtime|async runtime]]

