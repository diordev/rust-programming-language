---
title: "Async Channel"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# Async Channel

## Short Definition

Async channel — future'lar orasida xabar almashish uchun `trpl::channel()`. `std::sync::mpsc::channel`ga o'xshash, lekin `recv` blocking emas — future qaytaradi.

## Why It Matters

Thread'lar orasida `std::sync::mpsc` kanal ishlatiladi. Async kontekstda esa blocking recv qilish runtime'ni to'sib qo'yadi — butun async ekotizimi muzlab qoladi. Async channel esa `recv().await` qilib, runtime boshqa future'larni ishlata oladi.

## Mental Model

Oddiy kanal: `recv()` xabar kelguncha thread'ni "uxlatadi". Async kanal: `recv().await` xabar kelguncha future'ni pauza qiladi va runtime boshqa future'larni ishlatadi. Birinchisi thread to'sadi, ikkinchisi faqat bu future'ni to'xtatadi.

## Syntax and Examples

### Asosiy ishlatish

```rust
fn main() {
    trpl::block_on(async {
        let (tx, mut rx) = trpl::channel();  // mut rx!

        tx.send(String::from("salom")).unwrap();  // non-blocking
        let received = rx.recv().await.unwrap();  // async recv
        println!("{received}");
    });
}
```

`std::sync::mpsc` dan farqlari:
- `rx` — `mut` bo'lishi kerak
- `rx.recv()` — future qaytaradi, to'g'ridan-to'g'ri qiymat emas
- `tx.send()` — non-blocking (unbounded channel)

### while let bilan receive loop

```rust
let rx_fut = async {
    while let Some(value) = rx.recv().await {
        println!("received: {value}");
    }
    // rx.recv() → None bo'lganda loop tugaydi
};
```

`std::sync::mpsc`'da `for value in rx` bo'lardi — Rust hali `for` loopni async iteratorlar bilan ishlatishni qo'llab-quvvatlamaydi.

### async move bilan send

```rust
let tx_fut = async move {  // tx ownership blokka kiradi
    for val in ["hi", "from", "the", "future"] {
        tx.send(val.to_string()).unwrap();
        trpl::sleep(Duration::from_millis(500)).await;
    }
    // tx bu yerda dropped → channel yopiladi
};

let rx_fut = async {
    while let Some(v) = rx.recv().await {
        println!("{v}");
    }
};

trpl::join(tx_fut, rx_fut).await;
```

### Multiple producer

```rust
let (tx, mut rx) = trpl::channel();
let tx1 = tx.clone();

let tx1_fut = async move { /* tx1 bilan yuborish */ };
let tx_fut  = async move { /* tx bilan yuborish  */ };
let rx_fut  = async      { /* receive            */ };

// Barcha tx (clone'lar ham) dropped bo'lganda channel yopiladi:
trpl::join!(tx1_fut, tx_fut, rx_fut);
```

### Channel yopilish sharti

Channel faqat **barcha** sender'lar (asl `tx` va barcha `tx.clone()`) dropped bo'lganda yopiladi. Shuning uchun har bir sender alohida `async move` blokka move qilinishi kerak.

## Common Mistakes

- **`async move` unutish**: `tx` outer scope'da qolsa, channel hech qachon yopilmaydi — `rx.recv()` cheksiz kutadi.
- **`rx.recv().await` o'rniga `rx.recv()`**: `.await` bo'lmasa future yaratiladi, lekin hech narsa bajarmaydi.
- **`for` loop ishlatmoq**: Rust hali `for v in rx` async iteratorlar bilan ishlamaydi — `while let` kerak.
- **`tx.send()` ga await qo'shmoq**: `send` blocking emas va future qaytarmaydi — `await` kerak emas.

## Related Concepts

- [[channels|std::sync::mpsc channel]] — thread'lardagi sinxron ekvivalent
- [[message-passing|message passing]] — concurrency uchun xabar almashish paradigmasi
- [[async-move-block|async move block]] — tx'ni blokka o'tkazish
- [[async-join|async join]] — tx_fut va rx_fut'ni birgalikda kutish
- [[future|Future trait]] — recv future qaytaradi

## Sources

- [[wiki/sources/17-2-applying-concurrency-with-async]]
