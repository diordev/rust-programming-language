---
title: "Rust for Backend Developers: 4. Async"
type: chapter
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, backend, async, chapter]
source_count: 1
---

# Rust for Backend Developers: 4. Async

## Learning Goal

Backend I/O uchun thread-per-request modelidan Rust async modeliga o'tishni tushunish: [[future|Future]], [[async-await|async/await]], [[executor]], [[async-runtime|async runtime]], [[polling]], [[waker|Waker]], va [[pin|Pin]] orasidagi aloqani amaliy mental modelga keltirish.

## Main Ideas

- Rust `async fn`, `async {}` va `async ||` orqali future yaratish sintaksisini beradi.
- Rust standard library [[future|Future]] traitini beradi, lekin to'liq runtime yoki executor bermaydi.
- [[executor]] future'ni `poll` qiladi; [[async-runtime|async runtime]] esa odatda executor bilan birga timer, I/O driver, task queue, va thread poolni ham beradi.
- `.await` future natijasini sequential style'da olishga imkon beradi, lekin aslida suspension point hisoblanadi.
- `Future::poll` [[poll-enum|Poll::Ready]] yoki [[poll-enum|Poll::Pending]] qaytaradi.
- [[task-context|Context]] ichidagi [[waker|Waker]] future tayyor bo'lganda executor'ni qayta uyg'otish uchun kerak.
- [[pin|Pin]] future state machine ko'chmasligi kerak bo'lgan holatlarni xavfsiz saqlash uchun Future trait signature'ida turadi.
- Minimal executor misoli production runtime emas, faqat `poll`, `Pending`, `wake`, va requeue oqimini ko'rsatadigan o'quv modeli.

## Concepts

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

## Examples

```rust
async fn get_1() -> i32 {
    1
}

let future = get_1();
```

```rust
async fn get_user_with_address(user_id: u64) -> UserInfo {
    let user = get_user_by_id(user_id).await;
    let addr = get_address_by_id(user.addr_id).await;
    UserInfo { user, addr }
}
```

```rust
let future = async { 1 };
let closure = async || { 1 };
```

## Exercises

- `async fn` chaqiruvi natijani emas, future qaytarishini type annotation orqali ko'rsating.
- `block_on` bilan bitta oddiy future'ni bajaring.
- `Future::poll` signature'idagi `Pin`, `Context`, va `Poll` rollarini yozib chiqing.
- Minimal executor misolida `Waker::wake` queue'ga nima qaytarishini tushuntiring.

## Review Questions

- Executor bilan async runtime orasidagi farq nima?
- `.await` nima uchun suspension point hisoblanadi?
- `Poll::Pending`dan keyin future qanday qilib yana `poll` qilinadi?
- Nima uchun `Future::poll` `Pin<&mut Self>` oladi?
- Source'dagi `Sleep` future production runtime timeridan nimasi bilan farq qiladi?

## Related Pages

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
- [[wiki/chapters/rust-for-backend-developers-async-in-rust]]
- [[wiki/chapters/17-5-a-closer-look-at-the-traits-for-async]]
- [[future]]
- [[async-runtime|async runtime]]

