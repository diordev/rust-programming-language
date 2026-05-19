---
title: "Async State Machine"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-19
tags: [rust, async, compiler]
source_count: 2
---

# Async State Machine

## Short Definition

Compilator har bir async blok va funksiyani yashirin **state machine** (holat mashinasi) ga aylantiradi. Har bir `await point` — bu state machine'ning bir holati. Runtime bu machine'ni `poll` qilib oldinga siljitadi.

## Why It Matters

`async`/`await` sintaksisi qulay ko'rinsa-da, ostida Rust compilatori juda murakkab ishni bajaradi. State machine tufayli:
1. Async kod zero-cost abstraction bo'ladi — qo'shimcha heap allocation yo'q
2. Ownership va borrowing qoidalari `await` nuqtalari orqali to'liq saqlanadi
3. Compilator xatolarni runtime'da emas, compile time'da ushlab qoladi

## Mental Model

`async fn` ga qarang:

```
async fn f() {
    let a = step1().await;  // holat 1
    let b = step2(a).await; // holat 2
    step3(b).await;         // holat 3
}
```

Compilator buni taxminan quyidagiga aylantiradi:

```
enum FStateMachine {
    State0 { /* boshlang'ich */},
    State1 { /* step1 kutmoqda */ },
    State2 { a: A, /* step2 kutmoqda */ },
    State3 { b: B, /* step3 kutmoqda */ },
    Done,
}
```

Har `poll` chaqiruvida machine bir holattan keyingisiga o'tadi.

## Syntax and Examples

Compilator qanday transformatsiya qiladi:

```rust
// Siz yozasiz:
async fn page_title(url: &str) -> Option<String> {
    let response_text = trpl::get(url).await.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}

// Compilator taxminan shu ko'rinishdagi state machine yaratadi:
enum PageTitleFuture<'a> {
    Initial { url: &'a str },
    GetAwaitPoint { url: &'a str },
    TextAwaitPoint { response: trpl::Response },
}
```

Har bir holat kerakli ma'lumotlarni saqlaydi. Runtime `poll` chaqirganda machine joriy holatidan keyingi holat bo'yicha ishni davom ettiradi.

Manbadagi "fiber" analogiyasini shu yerda ehtiyotkor ishlatish mumkin: async function bajarilishi `.await` nuqtalarida to'xtab, keyin davom etadi. Rustdagi real interface esa [[future|Future]] state machine va [[executor]] tomonidan `poll` qilish modelidir.

### Nega `async move` kerak?

```rust
fn page_title(url: &str) -> impl Future<Output = Option<String>> {
    async move {  // move kerak: url parametrini capture qiladi
        trpl::get(url).await.text().await
    }
}
```

State machine holatlar orasida ma'lumotlarni saqlaydi. Shuning uchun `url` kabi parameter state machine ichiga `move` qilinishi kerak.

## Common Mistakes

- **State machine size haqida tashvish**: Compiler bu optimallashtirishni o'zi qiladi; odatda bunga qo'l urmaslik kerak.
- **`await` orqali borrow qilish**: `await` nuqtasida tirik bo'lgan borrow state machine'da saqlanishi kerak — ba'zan lifetime xatolari chiqishi mumkin.

```rust
// Muammo: mutex guard await nuqtasida tirik
async fn problematic() {
    let mut guard = mutex.lock().await;
    *guard += 1;
    some_async_fn().await; // MutexGuard hali tirik — deadlock xavfi
    // Yechim: guard ni await dan oldin drop qilish
}
```

## Related Concepts

- [[future|Future trait]] — state machine shu trait'ni implement qiladi
- [[polling|polling]] — har poll chaqiruvida machine bir qadam oldinga boradi
- [[waker|Waker]] — pending state'dan keyin davom ettirish signalini beradi
- [[async-await|async/await]] — state machine'ni yashiradigan sintaksis
- [[async-runtime|async runtime]] — state machine'ni ishlatuvchi
- [[zero-cost-abstractions|zero-cost abstractions]] — state machine qo'shimcha heap sarflamaydi

## Sources

- [[17-1-futures-and-the-async-syntax]]
- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
