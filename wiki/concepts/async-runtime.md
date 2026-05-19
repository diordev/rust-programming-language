---
title: "Async Runtime"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-19
tags: [rust, async, runtime]
source_count: 2
---

# Async Runtime

## Short Definition

Async runtime — future'larni bajaruvchi va ularning holatini boshqaruvchi dastur tizimi. Executor deb ham ataladi. Rust standart kutubxonasida runtime yo'q — uchinchi tomon cratelari (`tokio`, `async-std`) kerak.

## Why It Matters

Async kodni yozish va uni bajarish ikki alohida ish. Compilator `async fn` va `await`ni state machine'larga aylantiradi, lekin bu machine'larni ishlatib, natijalarni boshqaradigan mexanizm — runtime. Rustda bir nechta runtime cratelari mavjud, har biri turli use case uchun optimallashtirilgan. Bu moslashuvchanlik beradi, lekin yangi boshlovchilar uchun qo'shimcha murakkablik ham.

Aniqroq ajratganda, [[executor]] runtime'ning future'larni `poll` qiladigan markaziy qismi. Async runtime esa ko'pincha executor bilan birga timer, I/O driver, task spawning API, va thread poolni ham beradi.

## Mental Model

Runtime'ni "ish beruvchi" sifatida tasavvur qiling. Sizning async funksiyalaringiz "ishchilar". Runtime:
1. Har bir future'ni navbatga qo'yadi
2. Tayyor bo'lganlarini ishga tushiradi (`poll` qiladi)
3. Kutmoqda bo'lganlarini to'xtatib boshqalarga o'tadi
4. Tayyor signal kelganda to'xtatilgan future'ga qaytadi

## Syntax and Examples

### Nima uchun `main` async bo'lolmaydi

```rust
// XATO — E0752
async fn main() {
    // ...
}
// error[E0752]: `main` function is not allowed to be `async`
```

`main` async bo'lolmaydi chunki runtime `main` dan **oldin** ishlashni boshlashi kerak. Kimdir runtime'ni ishga tushirishi lozim.

### block_on bilan manual runtime

```rust
fn main() {
    // trpl::block_on ichida tokio runtime yaratadi
    trpl::block_on(async {
        println!("Async kod ishlayapti!");
    });
}
```

`block_on`:
- Runtime'ni ishga tushiradi
- Berilgan future tugaguncha joriy thread'ni **bloklaydi** (shu sababli `block_on`)
- Future qaytargan qiymatni qaytaradi

`futures::executor::block_on` juda sodda executor sifatida ham ishlatilishi mumkin: u murakkab I/O driver yoki thread pool ko'rsatmaydi, lekin future bajarilishi uchun executor kerakligini aniq ko'rsatadi.

### Tokio macro bilan

Real loyihalarda `#[tokio::main]` macro'si qulay:

```rust
#[tokio::main]
async fn main() {
    // async kod yozish mumkin
}

// Yuqoridagi aslida quyidagiga aylanadi:
fn main() {
    tokio::runtime::Runtime::new()
        .unwrap()
        .block_on(async {
            // ...
        })
}
```

### Turli runtime'lar

| Runtime | Qo'llanish sohasi |
|---------|------------------|
| `tokio` | Web serverlar, yuqori yuklamali I/O |
| `async-std` | Standard kutubxonaga o'xshash API |
| `embassy` | Embedded tizimlar, heap yo'q |
| `smol` | Kichik, yengillatilgan |

## Common Mistakes

- **Runtime yo'q holda `.await` ishlatish**: Runtime bo'lmasa `block_on` yoki `#[tokio::main]` kerak.
- **Ikki xil runtime aralash ishlatish**: `tokio` va `async-std` future'larini aralashtirib bo'lmaydi.
- **`block_on`ni async kontekstda chaqirish**: `block_on` async blok ichida chaqirilsa panic bo'lishi mumkin.

## Related Concepts

- [[future|Future trait]] — runtime boshqaradigan lazy qiymat
- [[executor]] — runtime'ning polling va scheduling yadrosi
- [[block-on|block_on]] — sync entrypointdan future bajarish
- [[waker|Waker]] — runtime'ni qayta poll qilishga uyg'otadi
- [[polling|polling]] — runtime future'ni qanday tekshiradi
- [[async-await|async/await]] — runtime bilan ishlash sintaksisi
- [[concurrency]] — runtime concurrencyni ta'minlaydi

## Sources

- [[17-1-futures-and-the-async-syntax]]
- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
