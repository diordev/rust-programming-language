---
title: "17.1. Futures and the Async Syntax"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, futures]
source_count: 1
---

# 17.1. Futures and the Async Syntax

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-01-futures-and-syntax.html
- Raw: `raw/books/the_rust_programming_language/17.1. Futures and the Async Syntax.md`

## Detailed Summary

Bu bo'lim Rust'ning async programmingining asosini — `Future` trait va `async`/`await` sintaksisini ko'rsatadi. Birinchi amaliy async dastur sifatida kichik web scraper quriladi.

### Future trait

**Future** — hali tayyor bo'lmagan, lekin bir vaqt kelib tayyor bo'ladigan qiymat. Ko'p tillarda `task` yoki `promise` deb nomlanadi. Rust standart kutubxonasida `std::future::Future` trait'i bor:

```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

Muhim jihat: **Rust future'lari lazy** — `.await` ishlatilmaganda hech narsa bajarmaydi. Bu `thread::spawn`dan farqli: u closure'ni darhol boshlaydi.

### async fn va async block

`async` keywordi funksiyalarga ham, bloklarga ham qo'llaniladi:

```rust
async fn page_title(url: &str) -> Option<String> {
    let response_text = trpl::get(url).await.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

`async fn` compilator tomonidan `impl Future<Output=T>` qaytaradigan oddiy funksiyaga aylantiriladi:

```rust
fn page_title(url: &str) -> impl Future<Output = Option<String>> {
    async move {
        let text = trpl::get(url).await.text().await;
        Html::parse(&text)
            .select_first("title")
            .map(|title| title.inner_html())
    }
}
```

### await postfix sintaksisi

Rust'da `await` **postfix** keyword — ifodaning keyin yoziladi, oldin emas:

```rust
let response = trpl::get(url).await;          // C#/JS dan farqli
let text = trpl::get(url).await.text().await; // zanjirli ishlatish mumkin
```

Bu method chain'larini yozishni ancha qulay qiladi.

### Async state machine

Har bir `await point` — runtime'ga boshqaruvni qaytarish nuqtasi. Compilator async blok uchun yashirin state machine yaratadi:

```rust
enum PageTitleFuture<'a> {
    Initial { url: &'a str },
    GetAwaitPoint { url: &'a str },
    TextAwaitPoint { response: trpl::Response },
}
```

Buni qo'lda yozish juda qiyin bo'lardi. Compilator buni avtomatik boshqaradi va ownership/borrowing qoidalari ham avtomatik tekshiriladi.

### Runtime va nima uchun `main` async bo'lolmaydi

Async kodni bajarish uchun **runtime** kerak. `main` funksiyasi async bo'lolmaydi — runtime `main` dan _oldin_ ishlashni boshlashi kerak.

```rust
// XATO
async fn main() { ... }

// To'g'ri
fn main() {
    trpl::block_on(async {
        // bu yerda async kod yoziladi
    })
}
```

`block_on` tokio runtime'ini ichida ishga tushiradi va future tugaguncha joriy thread'ni to'sadi.

Ko'pchilik runtime cratelari `#[tokio::main]` kabi macrolari bilan `async fn main()` yozishga imkon beradi — aslida ular `block_on` ga teng ishlaydi.

### Future'larni parallel race qilish

`trpl::select` ikkita future'dan birinchi tayyor bo'lganini qaytaradi:

```rust
let title_fut_1 = page_title(&args[1]);
let title_fut_2 = page_title(&args[2]);

let (url, maybe_title) = match trpl::select(title_fut_1, title_fut_2).await {
    Either::Left(left) => left,
    Either::Right(right) => right,
};
```

`Either` tipi `Result` ga o'xshaydi, lekin success/failure yo'q — faqat `Left` va `Right`:

```rust
enum Either<A, B> {
    Left(A),
    Right(B),
}
```

### trpl crate

Bu bob `trpl` cratesini ishlatadi — `futures` va `tokio` cratelari ustidagi yupqa wrapper. Bu o'quv maqsadida ekosistema murakkabligini yashiradi va asosiy kontseptsiyalarga e'tiborni qaratadi.

## Key Concepts

- [[future|Future trait]] — lazy asinxron qiymat
- [[async-await|async/await]] — async fn va await sintaksisi
- [[polling|polling]] — future tayyor yoki yo'qligini tekshirish
- [[async-state-machine|async state machine]] — compilator tomonidan yaratilgan yashirin state machine
- [[async-runtime|async runtime]] — executor, `block_on`, tokio
- [[concurrency]] — `trpl::select` bilan concurrent future'lar

## Code Examples

- [[async-page-scraper]] — `page_title` funksiyasi va `trpl::select` bilan ikki URL raqobati

## Exercises or Practice Ideas

1. `page_title` funksiyasini `async fn` o'rniga qo'lda `fn -> impl Future` ko'rinishida yozing.
2. `trpl::block_on` o'rniga `tokio::main` macro'sini ishlatib ko'ring.
3. `trpl::select` bilan 3 ta URL o'rtasida raqobat qiling (2 ta emas).
4. Future'ning lazy ekanligini ko'rsatib bering: `.await` qo'shmasdan future yarating va kuzating.

## Questions Raised

- `Future::poll` ichida `Pin<&mut Self>` nima uchun kerak?
- `async move` va `async` (move'siz) qanday farqlanadi?
- `select` va `join` qanday farqlanadi?

## Links To Update

- [[17-1-futures-and-async-syntax]] — yangi chapter page
- [[future]] — yangi concept page
- [[async-runtime]] — yangi concept page
- [[async-state-machine]] — yangi concept page
- [[async-page-scraper]] — yangi example page
