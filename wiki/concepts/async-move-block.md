---
title: "Async Move Block"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, ownership]
source_count: 1
---

# Async Move Block

## Short Definition

`async move { }` — tashqi scope'dan qiymatlarni ownership bilan capture qiladigan async blok. Closure'dagi `move` kalit so'zi bilan bir xil semantika, async bloklar uchun.

## Why It Matters

Async bloklar state machine'ga aylanadi va `await` nuqtalarida qiymatlarni saqlashi kerak. Ba'zan tashqi scope'dagi qiymat async blokka o'tishi (move bo'lishi) lozim — masalan, channel sender'ni async blok ichida drop qilish uchun. `async move` bu imkonni beradi.

## Mental Model

`move` closures bilan o'xshashlik:

```
closure:     let f = move || { uses(x) };  // x moved
async block: let fut = async move { uses(x).await };  // x moved
```

Ikkalasi ham capture qilishni borrow'dan move'ga o'zgartiradi. Xuddi closures bilan `thread::spawn`da `move` kerak bo'lgani kabi, async bloklar bilan ham `move` kerak bo'ladi.

## Syntax and Examples

### Oddiy async move

```rust
let x = String::from("hello");

let fut = async move {
    println!("{x}");  // x moved — borrow emas
};

// println!("{x}");  // XATO: x moved bo'ldi
fut.await;
```

### async move vs async (borrow)

```rust
let s = String::from("hello");

// Borrow qiladi:
let fut_borrow = async { println!("{s}"); };

// Move qiladi:
let fut_move = async move { println!("{s}"); };

// Borrow bilan: s hali ishlatilishi mumkin (fut_borrow scoepdan chiqqandan keyin)
// Move bilan: s fut_move'ga o'tdi, keyin ishlatib bo'lmaydi
```

### Channel uchun async move

Eng keng tarqalgan use case — channel sender'ni async blok ichida drop qilish:

```rust
let (tx, mut rx) = trpl::channel();

let tx_fut = async move {    // tx ownership bu blokka o'tadi
    tx.send("hi").unwrap();
    trpl::sleep(Duration::from_millis(500)).await;
    tx.send("bye").unwrap();
    // blok tugaganda tx DROPPED → channel yopiladi
};

let rx_fut = async {
    while let Some(v) = rx.recv().await {
        println!("{v}");
    }
    // tx dropped bo'lgandan keyin rx.recv() → None → loop tugaydi
};

trpl::join(tx_fut, rx_fut).await;
```

`async move` bo'lmasa:

```rust
let tx_fut = async {    // tx borrow qilinadi, outer scope'da qoladi
    tx.send("hi").unwrap();
    // blok tugasada tx outer scope'da hali tirik
    // channel YOPILMAYDI → rx.recv() cheksiz kutadi
};
```

### Multiple producer bilan async move

```rust
let tx1 = tx.clone();

let fut1 = async move { /* tx1 ishlatadi, keyin tx1 dropped */ };
let fut2 = async move { /* tx  ishlatadi, keyin tx  dropped */ };
// Har ikkisi dropped bo'lganda channel yopiladi
```

## Common Mistakes

- **`async move` kerak joyda oddiy `async` qo'llash**: Channel sender outer scope'da qolib, channel hech qachon yopilmaydi.
- **Move qilingan qiymatni keyin ishlatmoq**: `async move` blok yaratilgandan keyin o'sha qiymatni tashqarida ishlatib bo'lmaydi.

```rust
let s = String::from("hello");
let fut = async move { println!("{s}"); };
println!("{s}"); // XATO: s moved bo'ldi
```

## Related Concepts

- [[async-channel|async channel]] — async move eng keng tarqalgan use case
- [[async-await|async/await]] — async block semantikasi
- [[move-closures-threads|move closures (threads)]] — thread'lardagi o'xshash pattern
- [[ownership]] — move semantikasi
- [[async-state-machine|async state machine]] — moved qiymatlar state machine'da saqlanadi

## Sources

- [[wiki/sources/17-2-applying-concurrency-with-async]]
