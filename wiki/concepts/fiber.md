---
title: "Fiber"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, concurrency]
source_count: 1
---

# Fiber

## Short Definition

Fiber - user-space scheduler tomonidan to'xtatib-davom ettiriladigan yengil execution flow. Rust async kontekstida bu atama rasmiy API nomi emas, [[future|Future]] va async function uchun mental model sifatida ishlatilishi mumkin.

## Why It Matters

Rust async modelini tushuntirishda "fiber" analogiyasi foydali: async function ichidagi `.await` nuqtalari scheduler to'xtashi mumkin bo'lgan joylar kabi ko'rinadi. Lekin kod yozishda Rust atamalari `Future`, task, executor, runtime bo'lib qoladi.

## Mental Model

OS thread stack va kernel schedulerga tayanadi. Fiber esa user-space schedulerga tayanadi va odatda arzonroq context switch modelini maqsad qiladi. Rustda compiler async functionni state machinega aylantiradi; runtime shu state machine'larni `poll` qiladi.

## Syntax and Examples

Rustda odatda "fiber" deb yozilmaydi:

```rust
async fn load() -> i32 {
    1
}

let future = load();
```

Bu yerda `future` manbadagi fiber analogiyasiga mos tushadi, lekin Rust type system nuqtai nazaridan u `impl Future<Output = i32>`.

## Common Mistakes

- Fiber atamasini Rust standard library tipi deb qabul qilish. Bunday public Rust tipi yo'q.
- Future va OS threadni bir xil execution unit deb o'ylash. Future executor tomonidan poll qilinadi; OS thread kernel scheduler tomonidan boshqariladi.
- `.await` har doim boshqa OS threadga o'tkazadi deb o'ylash. Bu runtime implementationiga bog'liq.

## Related Concepts

- [[future|Future]]
- [[async-task|async task]]
- [[executor]]
- [[async-runtime|async runtime]]
- [[async-state-machine|async state machine]]
- [[threads]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

