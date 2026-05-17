---
title: "17.1. Futures and the Async Syntax"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, futures]
source_count: 1
---

# 17.1. Futures and the Async Syntax

## Learning Goal

`Future` trait'ini, `async fn`/`async block` sintaksisini, runtime'ning rolini va `trpl::select` bilan concurrent future'larni race qilishni tushunish.

## Main Ideas

### Future — lazy qiymat

`Future` — hali tayyor bo'lmagan, lekin keyin tayyor bo'ladigan qiymat. Rust future'lari **lazy**:

```rust
let fut = some_async_fn(); // hech narsa bajarmaydi!
fut.await;                 // faqat shu yerda ishlaydi
```

Bu `thread::spawn`dan keskin farqlanadi — u closure'ni darhol boshlaydi.

Iterator'lar bilan o'xshashlik: iterator `next()` chaqirilmasa ishlamas, xuddi shunday future `await` bo'lmasa ishlamas.

### async fn va async block

`async` funksiyalar va bloklarga qo'llaniladi:

```rust
async fn page_title(url: &str) -> Option<String> {
    let response_text = trpl::get(url).await.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

Compilator bu funksiyani automatik ravishda quyidagiga aylantiradi:

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

- `impl Future<Output = Option<String>>` — 10-bobdagi `impl Trait` sintaksisi
- `async move` — closure'ga o'xshab, `url` parametrini capture qiladi
- Qaytariladigan `Output` turi asl `async fn`'ning return type'iga teng

### await postfix sintaksisi

```rust
let r = trpl::get(url).await;          // postfix — keyin yoziladi
let t = trpl::get(url).await.text().await; // zanjirli ishlatish
```

Boshqa tillarda (`C#`, `JavaScript`) `await expr` ko'rinishida prefix bo'ladi. Rust'da postfix method chain'larni qulay qiladi.

### Async state machine — ichki mexanizm

Compilator har bir `await point`ni state machine holatiga aylantiradi:

```rust
// Compilator shu ko'rinishdagi enumni yashirin yaratadi:
enum PageTitleFuture<'a> {
    Initial { url: &'a str },
    GetAwaitPoint { url: &'a str },
    TextAwaitPoint { response: trpl::Response },
}
```

Har bir `await` — runtime'ga boshqaruvni qaytarish nuqtasi. Runtime boshqa tayyorroq ishni bajarib, bu future'ga keyinroq qaytadi.

Ownership va borrowing qoidalari bu state machine uchun ham kompilyator tomonidan tekshiriladi.

### Runtime — nima uchun main async bo'lolmaydi

Async kodni bajaruvchi **runtime** (executor) kerak. `main` funksiya async bo'lolmaydi — runtime'ning o'zi `main`dan oldin ishga tushirilishi lozim:

```rust
// XATO: E0752
async fn main() { ... }

// TO'G'RI
fn main() {
    trpl::block_on(async {
        let url = &std::env::args().collect::<Vec<_>>()[1];
        match page_title(url).await {
            Some(t) => println!("Title: {t}"),
            None => println!("No title"),
        }
    })
}
```

`block_on`:
- Ichida tokio runtime ishga tushiradi
- Berilgan future tugaguncha joriy thread'ni **blocks** qiladi
- Future qaytargan qiymatni qaytaradi

Ko'pchilik runtime cratelari `#[tokio::main]` macro'si bilan `async fn main()` yozishga imkon beradi — bu macro aslida `block_on` ga tenglashtirilgan kod generatsiya qiladi.

### trpl::select — ikkita future'ni race qilish

```rust
use trpl::{Either, Html};

trpl::block_on(async {
    let title_fut_1 = page_title(&args[1]);
    let title_fut_2 = page_title(&args[2]);

    let (url, maybe_title) = match trpl::select(title_fut_1, title_fut_2).await {
        Either::Left(left) => left,
        Either::Right(right) => right,
    };

    println!("{url} returned first");
})
```

`Either` tipi:

```rust
enum Either<A, B> {
    Left(A),   // birinchi argument g'olib bo'lsa
    Right(B),  // ikkinchi argument g'olib bo'lsa
}
```

`Result`dan farqli — success/failure yo'q, faqat "biri yoki ikkinchisi".

### trpl crate

O'quv maqsadida `futures` va `tokio` ustidagi yupqa wrapper. Real loyihalarda to'g'ridan-to'g'ri `tokio` yoki `async-std` ishlatiladi.

## Concepts

- [[future|Future trait]] — `poll`, `Output`, lazy evaluation
- [[async-await|async/await]] — sintaksis va semantika
- [[async-state-machine|async state machine]] — compilator ishlab chiqaradigan yashirin enum
- [[polling|polling]] — future'ni qayta-qayta tekshirish mexanizmi
- [[async-runtime|async runtime]] — `block_on`, executor, tokio
- [[concurrency]] — `trpl::select` bilan concurrent future race

## Examples

- [[async-page-scraper]] — `page_title` funksiyasi, `trpl::block_on`, `trpl::select`

## Exercises

1. `page_title` funksiyasini `async fn` emas `fn -> impl Future<...>` sifatida qayta yozing.
2. `block_on` o'rniga `#[tokio::main]` ishlatadigan versiyasini yozing.
3. `trpl::select` bilan 3 ta URL race qiling.
4. `.await` qo'shmasdan future yarating — compiler warning kuzating.

## Review Questions

1. Rust future'lari nima uchun lazy? Bu qanday foyda beradi?
2. `async fn f() -> T` aslida qanday tip qaytaradi?
3. Nima uchun `main` funksiyasi `async` bo'lolmaydi?
4. `trpl::select` va `trpl::block_on` rollarini farqlang.
5. State machine nima uchun kerak va uni kim yaratadi?

## Related Pages

- [[17-async-programming|17. Async Programming Intro]] — parallelism vs concurrency, CPU/I/O-bound
- [[future|Future trait]] — to'liq ta'rif va misollar
- [[async-runtime|async runtime]] — executor va runtime'lar
- [[17-1-futures-and-the-async-syntax|Source: 17.1]]
