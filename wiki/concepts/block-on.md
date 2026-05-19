---
title: "block_on"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, executor]
source_count: 1
---

# block_on

## Short Definition

`block_on` - bitta [[future|Future]]ni tugaguncha executor orqali bajarib, shu vaqt davomida joriy thread'ni bloklaydigan funksiya.

## Why It Matters

Oddiy `fn main()` ichida async kodni boshlash uchun biror kirish nuqtasi kerak. `block_on` shu bridge vazifasini bajaradi: sync dunyodan async future'ni ishga tushiradi.

## Mental Model

`block_on(future)` degani: "shu future tugamaguncha bu thread boshqa ishga o'tmasin". Ichkarida executor future'ni `poll` qiladi, `Pending` bo'lsa wake signalini kutadi, `Ready` bo'lsa natijani qaytaradi.

## Syntax and Examples

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

`block_on` future qaytargan qiymatni qaytaradi:

```rust
let n: i32 = block_on(async { 5 });
```

## Common Mistakes

- `block_on`ni production async serverning to'liq runtime modeli deb o'ylash. U kirish nuqtasi yoki sodda executor demo bo'lishi mumkin, lekin yuqori yuklamali backend uchun runtime tanlovi alohida masala.
- Async context ichida `block_on` chaqirish. Ko'p runtime'larda bu deadlock yoki panicga olib kelishi mumkin.
- Nomi sabab faqat "kutish" deb tushunish. Aslida u future'ni executor orqali bajaradi.

## Related Concepts

- [[executor]]
- [[async-runtime|async runtime]]
- [[future|Future]]
- [[polling]]
- [[wiki/crates/futures|futures]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

