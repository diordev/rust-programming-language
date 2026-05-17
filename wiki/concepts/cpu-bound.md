---
title: "CPU-bound"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, performance, concurrency]
source_count: 1
---

# CPU-bound

## Short Definition

CPU-bound operatsiya — ishlash tezligi CPU yoki GPU hisoblash quvvati bilan chegaralangan ish. Protsessor to'liq band bo'ladi va kutish vaqti yo'q.

## Why It Matters

CPU-bound va [[io-bound|I/O-bound]] operatsiyalar turli concurrency strategiyalarini talab qiladi. CPU-bound ishlar uchun async/await katta foyda bermaydi — CPU baribir band turadi. Buning uchun haqiqiy parallelism — ko'p thread yoki ko'p process — kerak.

## Mental Model

Tasavvur qiling: video montaj qilishni. Processor har bir kadr uchun juda ko'p hisoblash qilishi kerak. Bu ish CPU'ni to'liq yuklab oladi — kutish yo'q, faqat hisoblash. Bitta CPU'da bu ishni tezlashtirish uchun yordamchi thread'lar kerak.

Qarama-qarshi holat: internet orqali video yuklash — CPU ko'p vaqt bekor turadi, tarmoqdan paket kelishini kutadi. Bu [[io-bound|I/O-bound]].

## Syntax and Examples

```rust
use std::thread;

// CPU-bound ish — parallel threadlar bilan
fn heavy_computation(data: Vec<f64>) -> f64 {
    data.iter().map(|x| x.sqrt() * x.ln()).sum()
}

fn main() {
    let data1 = vec![1.0; 1_000_000];
    let data2 = vec![2.0; 1_000_000];

    // Ko'p thread bilan parallel hisoblash
    let h1 = thread::spawn(move || heavy_computation(data1));
    let h2 = thread::spawn(move || heavy_computation(data2));

    let r1 = h1.join().unwrap();
    let r2 = h2.join().unwrap();
    println!("{} {}", r1, r2);
}
```

CPU-bound ishlar uchun `rayon` crate ham keng ishlatiladi — data parallelism uchun.

## Common Mistakes

- **CPU-bound ishni async bilan tezlashtirmoq**: `async`/`await` CPU-bound ishlarga yordam bermaydi — CPU baribir to'liq band. Bunday holda haqiqiy ko'p thread yoki `rayon` kerak.
- **`spawn_blocking`ni unutish**: Async runtime ichida CPU-bound ish qilsangiz, runtime'ning boshqa future'larini ham to'sib qo'yasiz. Buning uchun `tokio::task::spawn_blocking` ishlatiladi.

```rust
// Xato: async runtime thread'ini to'sib qo'yadi
async fn bad() {
    let result = heavy_cpu_work(); // bu runtime'ni to'sadi!
}

// To'g'ri: alohida thread'da bajar
async fn good() {
    let result = tokio::task::spawn_blocking(|| heavy_cpu_work()).await.unwrap();
}
```

## Related Concepts

- [[io-bound|I/O-bound]] — CPU kutib turishi kerak bo'lgan operatsiya
- [[concurrency]] — concurrent va parallel ishlash modellari
- [[async-await|async/await]] — I/O-bound uchun afzalroq
- [[threads]] — CPU-bound uchun haqiqiy parallelism

## Sources

- [[17-fundamentals-of-asynchronous-programming]]
