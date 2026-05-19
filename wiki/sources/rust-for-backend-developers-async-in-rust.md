---
title: "Асинхронность в Rust - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, source, async, futures]
source_count: 1
---

# Асинхронность в Rust - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/4. async/60. Асинхронность в Rust.md`
- URL: https://rust-for-backend-developers.github.io/async/async-in-rust.html

## Detailed Summary

Bu source Rust async modelini user-space execution modeli sifatida ochadi. Asosiy g'oya: Rust tili va standard library async function hamda [[future|Future]] interfeysini beradi, lekin [[executor]] yoki to'liq [[async-runtime|async runtime]] implementationini std ichiga kiritmaydi. Shu sabab dasturchi bitta async sintaksisdan foydalanadi, runtime esa application talabiga qarab tanlanadi.

Source executor va async runtime atamalarini ajratadi. [[executor]] future'larni `poll` qiladigan scheduling yadro bo'lsa, [[async-runtime|async runtime]] odatda executor, timer, I/O driver, task spawning, va thread pool kabi qismlarni ham o'z ichiga oladigan kengroq tizim.

`async fn` e'lon qilinganda funksiya natijani darhol qaytarmaydi. Uni chaqirish `impl Future<Output = T>` qiymatini yaratadi. Source buni "fiber" analogiyasi bilan tushuntiradi, lekin Rust rasmiy API atamasi [[future|Future]] va async function hisoblanadi. Wiki'da [[fiber]] atamasi faqat mental model sifatida ishlatiladi.

Oddiy `async fn get_1() -> i32 { 1 }` chaqiruvi `i32` emas, future yaratadi. Natijani olish uchun future executor orqali bajarilishi kerak. Source `futures = "0.3"` crate'idagi `futures::executor::block_on`ni eng sodda misol sifatida ishlatadi. `block_on` berilgan future tugaguncha joriy OS thread'ni bloklaydi va murakkab scheduler yoki thread pool ko'rsatmaydi.

Async composition bo'limi [[async-await|async/await]]ni asosiy ergonomika sifatida beradi. `.await` faqat async function yoki [[async-block|async block]] ichida ishlaydi. U sequential flow'ni callback zanjirisiz yozishga imkon beradi: avval user service'dan `User` olinadi, keyin `user.addr_id` bilan address service'dan `Address` olinadi, oxirida `UserInfo` yig'iladi.

`.await` Rustda postfix ko'rinishda yoziladi. Bu method chaining bilan mos tushadi: `get_value().await.method().await`. Muhim jihat: `.await` future ichidagi suspension point. Executor aynan shu nuqtalarda future bajarilishini to'xtatib, boshqa future'ga o'tishi yoki keyin qayta davom ettirishi mumkin.

Source [[async-closure|async closure]] va [[async-block|async block]]ni future yaratishning qo'shimcha yo'llari sifatida ko'rsatadi. `async || { ... }` closure chaqirilganda future qaytaradi. `async { ... }` esa oddiy blockga o'xshaydi, lekin block natijasini emas, future qaytaradi.

Future anatomiyasi bo'limi asosiy traitni ko'rsatadi:

```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

`Output` future yakunlanganda olinadigan natija turi. `poll` esa executor future'ni oldinga siljitish uchun chaqiradigan metod. `poll` [[poll-enum|Poll]] qaytaradi: `Ready(value)` natija tayyorligini, `Pending` esa hozircha tayyor emasligini bildiradi.

`poll` argumentidagi [[task-context|Context]] future'ga [[waker|Waker]] beradi. Future `Pending` qaytarsa, tayyor bo'lganda `Waker::wake()` chaqirib executor'ga "meni yana poll qil" deb signal berishi kerak. Source modelida executor yangi future'ni bir marta `poll` qiladi; `Pending` bo'lsa, uni active/pending queue'da saqlaydi va waker signalisiz qayta bezovta qilmaydi.

[[pin|Pin]] qisqa eslatma sifatida beriladi. `poll(self: Pin<&mut Self>, ...)` async state machine xotirada ko'chmasligini talab qiladi, chunki async future ichida self-referential holatlar paydo bo'lishi mumkin. Source Pin'ni chuqur ochmaydi, lekin Future trait signature'da uning borligi sababini eslatadi.

Oxirgi bo'lim o'quv maqsadidagi minimal executor yozadi. Executor `mpsc` queue, `Arc<SpawnedTask>`, `Mutex<Option<Pin<Box<dyn Future<Output = ()> + Send + 'static>>>>`, task id seti, va custom `Wake` implementationidan foydalanadi. `SpawnedTaskWaker::wake` taskni yana queue'ga yuboradi; runtime uni qayta `poll` qiladi.

`Sleep` future misoli tayyorlik flagini `AtomicBool`da saqlaydi, `poll` paytida alohida OS thread spawn qiladi, `sleep`dan keyin flagni true qiladi va waker'ni uyg'otadi. Bu real runtime dizayni emas: har `poll`da thread spawn qilish production async timer uchun yomon model. Misol faqat `Pending -> wake -> poll again -> Ready` oqimini ko'rsatish uchun kerak.

Source yakunida `futures-rs`dagi `futures-executor` implementationini keyingi o'rganish nuqtasi sifatida ko'rsatadi.

## Key Concepts

- [[async-await|async/await]]
- [[future|Future]]
- [[executor]]
- [[async-runtime|async runtime]]
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
- [[async-task|async task]]
- [[fiber]]

## Code Examples

```toml
[dependencies]
futures = "0.3"
```

```rust
use futures::executor::block_on;

async fn get_1() -> i32 {
    1
}

fn main() {
    let future = get_1();
    let result = block_on(future);
    println!("{result}");
}
```

```rust
async fn my_flow() -> i32 {
    let number = load_number().await;
    transform_number(number).await
}
```

```rust
let future_from_block = async { 1 };
let closure = async || { 1 };
let future_from_closure = closure();
```

## Exercises or Practice Ideas

- `async fn add(a, b)` yozib, uni `futures::executor::block_on` orqali bajaring.
- Sequential `.await` bilan user/address composition yozing va dependency sabab parallel bajarib bo'lmasligini tushuntiring.
- `async {}` block va `async || {}` closure farqini kichik misolda ko'rsating.
- Minimal `Future` implement qilib, birinchi `poll`da `Pending`, keyingi `poll`da `Ready` qaytarish oqimini kuzating.
- Source'dagi minimal executorni o'zgartirib, `Sleep` uchun har `poll`da yangi thread spawn bo'lmasligini ta'minlang.

## Questions Raised

- Nima uchun Rust standard library async executor bermaydi?
- `executor` va `async runtime` atamalarini qachon ajratish kerak?
- `Waker` signali bo'lmasa `Pending` future qanday qilib qayta ishga tushadi?
- Minimal executor real runtime'dan qaysi muhim qismlar bilan farq qiladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-4-async]]
- [[wiki/chapters/rust-for-backend-developers-async-in-rust]]
- [[future]]
- [[async-await|async/await]]
- [[async-runtime|async runtime]]
- [[polling]]
- [[pin|Pin]]
- [[wiki/crates/futures]]
