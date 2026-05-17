---
title: "17.2. Applying Concurrency with Async"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# 17.2. Applying Concurrency with Async

## Learning Goal

16-bobdagi thread-based concurrency yechimlarini async bilan qayta implement qilishni o'rganish: task spawning, join, message passing, va `async move` ownership semantikasini tushunish.

## Main Ideas

### Thread vs Async — solishtiruv

| Thread | Async |
|--------|-------|
| `thread::spawn(closure)` | `trpl::spawn_task(async { })` |
| `handle.join()` | `handle.await.unwrap()` |
| `std::sync::mpsc::channel()` | `trpl::channel()` |
| `for v in rx` | `while let Some(v) = rx.recv().await` |
| `thread::sleep(d)` | `trpl::sleep(d).await` |
| OS thread (og'ir) | Async task (yengil) |

API'lar o'xshash ko'rinadi, lekin xatti-harakat va performance farqlanadi.

### Bitta async blok = ketma-ket bajarilish

**Asosiy tushuncha**: bitta `async { }` ichidagi hamma `await` nuqtalari ketma-ket bajariladi.

```rust
// BU CONCURRENT EMAS:
async {
    for val in vals { tx.send(val); sleep(500ms).await; }  // birinchi tugaydi
    while let Some(v) = rx.recv().await { println!("{v}"); } // keyin boshlanadi
}

// BU CONCURRENT:
let tx_fut = async { for val in vals { tx.send(val); sleep(500ms).await; } };
let rx_fut = async { while let Some(v) = rx.recv().await { println!("{v}"); } };
trpl::join(tx_fut, rx_fut).await;  // ikkalasi birgalikda
```

### spawn_task — task yaratish

```rust
let handle = trpl::spawn_task(async {
    for i in 1..10 {
        println!("hi {i} from first task!");
        trpl::sleep(Duration::from_millis(500)).await;
    }
});

for i in 1..5 {
    println!("hi {i} from main task!");
    trpl::sleep(Duration::from_millis(500)).await;
}

handle.await.unwrap(); // siz tugashini kut
```

`handle.await` — task handle o'zi future; `thread.join()` ga teng. Natija `Result<T, JoinError>`.

### trpl::join — ikki future birgalikda

```rust
let fut1 = async { /* ... */ };
let fut2 = async { /* ... */ };
trpl::join(fut1, fut2).await;
// ikkalasi tugashini kutadi, fair scheduling bilan
```

**Fair**: runtime ikkala future'ni teng tez-tez tekshiradi — birortasi oldinga "oshib ketmaydi".

### async move — ownership o'tkazish

```rust
let tx_fut = async move {   // tx shu blokka move bo'ladi
    for val in vals {
        tx.send(val).unwrap();
        trpl::sleep(Duration::from_millis(500)).await;
    }
    // blok tugaganda tx dropped → channel yopiladi
};
```

Nima uchun kerak: async blok closure kabi capture qiladi. `async move` bo'lmasa `tx` outer scope'da qoladi, hech qachon dropped bo'lmaydi, channel yopilmaydi, `rx.recv()` cheksiz `None` kutadi.

### trpl::join! makrosi — N ta future

```rust
let tx1_fut = async move { /* tx1 bilan yuborish */ };
let tx_fut  = async move { /* tx bilan yuborish  */ };
let rx_fut  = async      { /* receive            */ };

trpl::join!(tx1_fut, tx_fut, rx_fut);  // 3 ta future birgalikda
```

`join!` — macro, compile-time sondagi future'lar uchun. Noma'lum son: `join_all` (keyingi bob).

### Async channel patterns

```rust
let (tx, mut rx) = trpl::channel();  // mut rx!

// Multiple producer:
let tx1 = tx.clone();

// Send async blok ichida:
let tx_fut = async move {
    tx.send(val).unwrap();           // .await yo'q — non-blocking
    trpl::sleep(500ms).await;
};

// Receive:
let rx_fut = async {
    while let Some(value) = rx.recv().await {  // .await kerak!
        println!("{value}");
    }
    // rx.recv() → None bo'lganda loop tugaydi (channel yopilganda)
};
```

## Concepts

- [[async-task|async task]] — `spawn_task`, task handle, `handle.await`
- [[async-join|async join]] — `trpl::join`, `trpl::join!`, fair scheduling
- [[async-channel|async channel]] — `trpl::channel`, async `recv`, `while let`
- [[async-move-block|async move block]] — ownership async blokka o'tkazish
- [[future|Future trait]] — task handle o'zi future
- [[concurrency]] — bitta async blokda ketma-ket, alohida blokda concurrent

## Examples

- [[async-concurrent-tasks]] — spawn_task va join bilan ikki concurrent loop
- [[async-channel-messages]] — async channel, async move, multiple producers

## Exercises

1. `spawn_task` + `handle.await` bilan ikkita concurrent counter yozing.
2. `trpl::join` bilan ikkita async blokni birgalikda ishlatib, output tartibini kuzating.
3. `async move` yo'q holda `tx` channel muammosini takrorlang va xatolikni tushuntiring.
4. `tx.clone()` bilan ikkita sender, bitta receiver yozing.

## Review Questions

1. Nima uchun bitta `async {}` blok ichida send+receive concurrent ishlamaydi?
2. `async move` va oddiy `async` blok farkini tushuntiring.
3. `trpl::join` vs `trpl::join!` — qachon qaysi?
4. `rx.recv().await` qachon `None` qaytaradi?
5. `trpl::join` "fair" deyiladi — bu nima degani?

## Related Pages

- [[17-async-programming|17. Async Programming Intro]]
- [[17-1-futures-and-async-syntax|17.1. Futures and the Async Syntax]]
- [[wiki/chapters/16-fearless-concurrency|16. Fearless Concurrency]] — thread ekvivalentlari
- [[wiki/sources/17-2-applying-concurrency-with-async|Source: 17.2]]
