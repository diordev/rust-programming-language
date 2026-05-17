---
title: "Async Await"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-08
tags: [rust, async]
source_count: 3
---

# Async Await

## Short Definition

`async`/`await` — asynchronous computationsni ifodalash uchun Rust sintaksisi. `async` funksiya yoki blok `Future` qaytaradi; `.await` uni bajaradi va tayyor bo'lguncha cooperative tarzda kutadi.

## Why It Matters

I/O-bound operatsiyalar (tarmoq so'rovlari, fayl o'qish) uchun thread'larga qaraganda ancha samarali. Bitta thread ichida yuzlab/minglab concurrent so'rovlarni boshqarish mumkin. Web serverlar, CLI toollar, network daemons — barchasida async zarur.

## Mental Model

`async` blok yoki funksiya yozilganda — compilator bu kodni `Future` trait'ini implement qiladigan **state machine**ga aylantiradi. `.await` — state machine'ning "davom et" buyrug'i:
- Tayyor bo'lsa natija olinsn
- Tayyor bo'lmasa boshqaruv runtime'ga qaytarilsin

Iterator bilan o'xshashlik:
- Iterator `.next()` chaqirilmasa ishlamaydi
- Future `.await` bo'lmasa ishlamaydi — ikkalasi **lazy**

## Syntax and Examples

### Oddiy async funksiya

```rust
async fn greet(name: &str) -> String {
    format!("Salom, {name}!")
}

fn main() {
    trpl::block_on(async {
        let msg = greet("Rustacean").await;
        println!("{msg}");
    });
}
```

### Zanjirli await

```rust
async fn fetch_title(url: &str) -> Option<String> {
    // postfix sintaksis — method chaining qulay
    let text = trpl::get(url).await.text().await;
    Html::parse(&text)
        .select_first("title")
        .map(|t| t.inner_html())
}
```

### async fn desugaring

```rust
// Siz yozasiz:
async fn add(a: i32, b: i32) -> i32 { a + b }

// Compilator shunga aylantiradi:
fn add(a: i32, b: i32) -> impl Future<Output = i32> {
    async move { a + b }
}
```

### main funksiyasida runtime

```rust
fn main() {
    trpl::block_on(async {
        // async kod bu yerda
    });
}

// Yoki tokio bilan:
#[tokio::main]
async fn main() {
    // async kod
}
```

## Common Mistakes

- **`.await` async bo'lmagan funksiyada ishlatish**: `.await` faqat `async` funksiya yoki `async {}` blok ichida ishlaydi.
- **Runtime'ni unutish**: `main` async bo'lolmaydi — `block_on` yoki `#[tokio::main]` kerak.
- **Lazy ekanligini unutish**: Future yaratish uni ishlatish emas — `.await` bo'lmasa hech narsa bajarmaydi.

```rust
// Xato — natija hech qachon olinmaydi
let _ = fetch_data("https://example.com");

// To'g'ri
let result = fetch_data("https://example.com").await;
```

## Related Concepts

- [[future|Future trait]] — async funksiya qaytaradigan tip
- [[async-runtime|async runtime]] — future'larni bajoruvchi executor
- [[async-state-machine|async state machine]] — compilator async kodni bunga aylantiradi
- [[polling|polling]] — runtime future'ni qanday tekshiradi
- [[io-bound|I/O-bound]] — async'ning asosiy qo'llanish sohasi
- [[concurrency]] — async concurrent model
- [[zero-cost-abstractions|zero-cost abstractions]] — state machine heap allocationsiz ishlaydi

## Sources

- [[wiki/sources/0-2-introduction]]
- [[17-fundamentals-of-asynchronous-programming]]
- [[17-1-futures-and-the-async-syntax]]
