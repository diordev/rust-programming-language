---
title: "Future"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, futures]
source_count: 2
---

# Future

## Short Definition

`Future` — hali tayyor bo'lmagan, lekin bir vaqt kelib tayyor bo'ladigan qiymatni ifodalovchi trait. Rust async programmingining asosiy qurilish bloki.

## Why It Matters

`Future` trait Rust async ekotizimining yadrosida turadi. Har qanday async operatsiya — tarmoq so'rovi, fayl o'qish, `async fn` — `Future` implementatsiyasiga aylanadi. Bu trait tufayli turli xil async operatsiyalar bir xil interfeys orqali boshqarilishi mumkin.

## Mental Model

Future'ni "eventual bir marta keltiriladigan posylka" sifatida tasavvur qiling: hozir qo'lingizdamas, lekin keyin keladi. Siz uni kutishingiz (`await`) yoki boshqa ish bilan shug'ullanib qaytib tekshirishingiz mumkin.

Iterator bilan o'xshashlik:
- Iterator `next()` chaqirilmaganda hech narsa bajarmas → Future `await` bo'lmaganda hech narsa bajarmaydi
- Ikkalasi ham **lazy** — faqat so'ralganda ishlaydi

`thread::spawn` bilan farq:
- `thread::spawn` closure'ni **darhol** boshlaydi
- `Future` `.await` bo'lmaguncha **butunlay bekor turadi**

## Syntax and Examples

```rust
use std::future::Future;

// async fn — Future qaytaradi
async fn fetch_data(url: &str) -> String {
    trpl::get(url).await.text().await
}

// Yuqoridagi async fn aslida quyidagiga teng:
fn fetch_data_desugared(url: &str) -> impl Future<Output = String> {
    async move {
        trpl::get(url).await.text().await
    }
}
```

`Future` trait ta'rifi (soddalashtirilgan):

```rust
pub trait Future {
    type Output;  // future tayyor bo'lganda qaytariladigan tip

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}

enum Poll<T> {
    Ready(T),    // tayyor — qiymat mavjud
    Pending,     // hali kutmoqda
}
```

Future'larni `.await` bilan ishlatish:

```rust
fn main() {
    trpl::block_on(async {
        // future yaratildi, lekin hali ishlamaydi:
        let fut = fetch_data("https://example.com");

        // shu yergacha kelganda runtime fut ni ishga tushiradi:
        let result = fut.await;
        println!("{result}");
    })
}
```

## Common Mistakes

- **`.await` unutib qolish**: Rust compiler warning beradi (`unused future`), lekin beginner'lar buni ko'rmay o'tishi mumkin.
- **Future + async bo'lmagan kontekstda ishlatish**: `.await` faqat async funksiya yoki async blok ichida ishlaydi.
- **Thread::spawn bilan aralashtirib yuborish**: `thread::spawn` darhol boshlaydi, future esa lazy.

```rust
// Xato — await qo'shilmagan, hech narsa bajarilmaydi
let _ = fetch_data("https://example.com");

// To'g'ri
let result = fetch_data("https://example.com").await;
```

## Related Concepts

- [[async-await|async/await]] — future yaratish va kutish sintaksisi
- [[polling|polling]] — runtime future'ni qanday tekshiradi
- [[async-state-machine|async state machine]] — future ichidagi yashirin holat mashinasi
- [[async-runtime|async runtime]] — future'larni bajoruvchi executor
- [[pin|Pin va Unpin]] — `poll(self: Pin<&mut Self>)` — Pin zarurligi
- [[join-all|join_all]] — Vec ichida future'lar, Pin bilan
- [[iterators]] — o'xshash lazy model

## Sources

- [[17-1-futures-and-the-async-syntax]]
- [[wiki/sources/17-5-a-closer-look-at-the-traits-for-async]]
