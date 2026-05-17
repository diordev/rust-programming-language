---
title: "17.2. Applying Concurrency with Async"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# 17.2. Applying Concurrency with Async

## Source

- URL: https://doc.rust-lang.org/stable/book/ch17-02-concurrency-with-async.html
- Raw: `raw/books/the_rust_programming_language/17.2. Applying Concurrency with Async.md`

## Detailed Summary

Bu bo'lim 17.1-bobda o'rganilgan future/async sintaksisini amalda qo'llaydi. 16-bobdagi thread-based concurrency yechimlarini async bilan solishtiradi: `spawn_task` vs `thread::spawn`, async channel vs `std::sync::mpsc`, `join` va `join!` vs `.join()`.

### spawn_task — yangi async task yaratish

`trpl::spawn_task` thread::spawn'ga o'xshash API beradi, lekin OS thread emas, async task yaratadi:

```rust
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

    handle.await.unwrap(); // thread'dagi .join() ga teng
});
```

`handle` — future implementatsiya qiladigan task handle; `handle.await` task tugaguncha kutadi. Natija `Result`, shuning uchun `.unwrap()` kerak.

Muhim farq: `main` async blokining oxirida spawned task ham to'xtaydi — xuddi main thread tugaganda spawned thread'lar kabi. `handle.await` bo'lmasa task tugamasdan o'chishi mumkin.

### trpl::join — ikkita future'ni birgalikda kutish

`spawn_task` kerak emas — async bloklar to'g'ridan-to'g'ri future bo'ladi va `trpl::join` ikkalasini birgalikda bajaradi:

```rust
let fut1 = async { /* birinchi loop */ };
let fut2 = async { /* ikkinchi loop */ };
trpl::join(fut1, fut2).await;
```

`trpl::join` ikkala future output'idan tuzilgan tuple qaytaruvchi yangi future beradi. Ikkalasi **tugagandan keyin** natija tayyor bo'ladi.

**Fair scheduling**: `trpl::join` har ikkala future'ni teng tez-tez tekshiradi va birortasi oldinga "oshib ketmasligini" ta'minlaydi. Thread scheduler'dan farqi — OS ixtiyoriy, runtime deterministik bo'lishi mumkin.

### Bitta async blok ichida ketma-ketlik

Bitta `async { }` blok ichidagi hamma kod **ketma-ket** bajariladi:

```rust
// Bu kod CONCURRENT EMAS — hamma send tugagandan keyin receive boshlanadi
let (tx, mut rx) = trpl::channel();
for val in vals {
    tx.send(val).unwrap();
    trpl::sleep(500ms).await;  // await bor, lekin receive hali ishlamaydi
}
while let Some(v) = rx.recv().await { ... }
```

Concurrency uchun send va receive'ni **alohida async bloklarga** bo'lish kerak, keyin `trpl::join` bilan birlashtirish:

```rust
let tx_fut = async { /* send loop */ };
let rx_fut = async { /* receive loop */ };
trpl::join(tx_fut, rx_fut).await; // ikkisi concurrent
```

### async move — ownershipni async blokka o'tkazish

`async` blok closure kabi capture qiladi; ownership o'tkazish uchun `async move` kerak:

```rust
let tx_fut = async move {  // tx ownership bu blokka o'tadi
    for val in vals {
        tx.send(val).unwrap();
        trpl::sleep(500ms).await;
    }
    // blok tugaganda tx dropped — channel yopiladi
};
```

Bu muhim chunki: channel faqat barcha `tx` (sender) dropped bo'lganda yopiladi. `rx.recv().await` channel yopilganda `None` qaytaradi va `while let` tugatiladi. `async move` bo'lmasa `tx` outer scope'da qoladi va channel hech qachon yopilmaydi — dastur cheksiz kutadi.

### trpl::join! makrosi — N ta future'ni kutish

Ikkitadan ko'p future uchun `trpl::join!` makrosi ishlatiladi:

```rust
let (tx, mut rx) = trpl::channel();
let tx1 = tx.clone();

let tx1_fut = async move { /* tx1 bilan yuborish */ };
let tx_fut  = async move { /* tx bilan yuborish */ };
let rx_fut  = async { /* receive */ };

trpl::join!(tx1_fut, tx_fut, rx_fut); // 3 ta future birgalikda
```

`join!` macro compile time'da future soni ma'lum bo'lganda ishlaydi. Noma'lum sondagi future'lar uchun keyingi bo'limlarda `join_all` ko'riladi.

Multiple producer: `tx.clone()` bilan bir nechta sender yaratiladi; har biri alohida async blokda `async move` bilan capture qilinadi. Barcha sender dropped bo'lganda channel yopiladi va `rx_fut` tugaydi.

### async channel

`trpl::channel()` — async versiyasi `std::sync::mpsc::channel` ga o'xshash, lekin farqlari bor:

| | `std::sync::mpsc` | `trpl::channel` (async) |
|---|---|---|
| `recv()` | blocking — thread to'xtaydi | async — future qaytaradi |
| `send()` | blocking bo'lishi mumkin | non-blocking (unbounded) |
| receiver `mut` | yo'q | ha — `mut rx` |
| loop pattern | `for v in rx` | `while let Some(v) = rx.recv().await` |

`rx.recv().await` natija:
- `Some(message)` — xabar keldi
- `None` — channel yopildi (barcha tx dropped)

## Key Concepts

- [[async-task|async task (spawn_task)]] — async task yaratish, task handle
- [[async-join|async join]] — `trpl::join`, `trpl::join!`, fair scheduling
- [[async-channel|async channel]] — `trpl::channel`, async recv, `while let` pattern
- [[async-move-block|async move block]] — ownership async blokka o'tkazish

## Code Examples

- [[async-concurrent-tasks]] — spawn_task, join ikkita loop, fair scheduling
- [[async-channel-messages]] — async channel, `async move`, multiple producers

## Exercises or Practice Ideas

1. `trpl::join` ishlatib ikkita timer future'ni concurrent ishlating.
2. Async channel orqali 5 ta xabar yuboring va ulang; `async move` yo'q bo'lsa nima bo'lishini ko'ring.
3. `trpl::join!` bilan 3 ta future'ni birlashtiring.
4. Bitta async blokda send+receive yozib, nima uchun ketma-ket ishlashini kuzating.

## Questions Raised

- `trpl::join` va `trpl::select` qanday farqlanadi? (join — ikkalasi tugashini kutadi, select — biri tugashi yetarli)
- `trpl::join!` compile-time soni talab qiladi — noma'lum son uchun nima?
- Fair scheduling kafolatlangan bo'lsa, thread scheduler'dan deterministikroqmi?

## Links To Update

- [[wiki/chapters/17-2-applying-concurrency-with-async]] — yangi chapter page
- [[future]] — join pattern qo'shish
- [[async-await]] — async move qo'shish
- [[index]] — yangi pages
