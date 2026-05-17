---
title: "Async Page Scraper"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, futures, example]
source_count: 1
---

# Async Page Scraper

## Description

Birinchi Rust async dastur: ikkita URL'dan `<title>` elementini concurrent oladi va birinchi javob berganini chiqaradi. `trpl` crate orqali `tokio` ekotizimini ishlatadi.

## Source

[[17-1-futures-and-the-async-syntax]] — Listing 17-1 dan 17-5 gacha.

## Bosqichma-bosqich qurilish

### 1. Loyiha yaratish

```shell
cargo new hello-async
cd hello-async
cargo add trpl
```

### 2. async fn — birinchi async funksiya (Listing 17-1)

```rust
use trpl::Html;

async fn page_title(url: &str) -> Option<String> {
    let response = trpl::get(url).await;
    let response_text = response.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

### 3. Zanjirli await (Listing 17-2)

```rust
async fn page_title(url: &str) -> Option<String> {
    let response_text = trpl::get(url).await.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

`.await` postfix bo'lgani uchun method chaining qulay.

### 4. main + block_on (Listing 17-4)

```rust
fn main() {
    let args: Vec<String> = std::env::args().collect();

    trpl::block_on(async {
        let url = &args[1];
        match page_title(url).await {
            Some(title) => println!("The title for {url} was {title}"),
            None => println!("{url} had no title"),
        }
    })
}
```

`main` async bo'lolmaydi → `block_on` orqali runtime ishga tushiriladi.

### 5. Ikkita URL race (Listing 17-5)

```rust
use trpl::{Either, Html};

fn main() {
    let args: Vec<String> = std::env::args().collect();

    trpl::block_on(async {
        let title_fut_1 = page_title(&args[1]);
        let title_fut_2 = page_title(&args[2]);

        let (url, maybe_title) =
            match trpl::select(title_fut_1, title_fut_2).await {
                Either::Left(left) => left,
                Either::Right(right) => right,
            };

        println!("{url} returned first");
        match maybe_title {
            Some(title) => println!("Its page title was: '{title}'"),
            None => println!("It had no title."),
        }
    })
}

async fn page_title(url: &str) -> (&str, Option<String>) {
    let response_text = trpl::get(url).await.text().await;
    let title = Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html());
    (url, title)
}
```

`trpl::select` ikkita future'dan birinchi tayyor bo'lganini `Either::Left` yoki `Either::Right` sifatida qaytaradi.

### Ishga tushirish

```shell
cargo run -- "https://www.rust-lang.org" "https://docs.rs"
```

## Key Observations

- Future'lar **lazy**: `page_title(&args[1])` chaqirilganda hech narsa bajarmaydi; faqat `trpl::select(...)await` da ishlaydi.
- `page_title`'ning birinchi versiyasi `Option<String>` qaytaradi; race versiyasida `url`ni ham qaytarishi kerak — bo'lmasa qaysi URL g'olib bo'lganini bilolmaymiz.
- Ikkala future ham bitta async blok ichida concurrent ishlaydi — parallel thread'lar emas.

## Concepts Used

- [[async-await|async/await]] — `async fn`, `async { }`, `.await`
- [[future|Future trait]] — `page_title` va `trpl::select` future qaytaradi
- [[async-runtime|async runtime]] — `trpl::block_on` tokio runtime'ini ishga tushiradi
- [[io-bound|I/O-bound]] — tarmoq so'rovlari

## Related Pages

- [[17-1-futures-and-async-syntax]]
- [[17-1-futures-and-the-async-syntax]]
