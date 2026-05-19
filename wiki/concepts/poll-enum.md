---
title: "Poll"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, futures]
source_count: 1
---

# Poll

## Short Definition

`Poll<T>` - `Future::poll` qaytaradigan enum: `Ready(T)` future natijasi tayyorligini, `Pending` hali tayyor emasligini bildiradi.

## Why It Matters

Rust async modeli blocking kutish o'rniga cooperative polling ishlatadi. `Poll` executor va future orasidagi eng kichik protokol.

## Mental Model

`poll` savol beradi: "Hozir natijang bormi?"

- `Poll::Ready(value)` - "ha, mana natija"
- `Poll::Pending` - "yo'q, keyinroq waker orqali chaqiraman"

## Syntax and Examples

```rust
pub enum Poll<T> {
    Ready(T),
    Pending,
}
```

Oddiy darhol tayyor future:

```rust
fn poll(self: Pin<&mut Self>, _cx: &mut Context<'_>) -> Poll<i32> {
    Poll::Ready(42)
}
```

Kutayotgan future:

```rust
fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<()> {
    save_waker(cx.waker().clone());
    Poll::Pending
}
```

## Common Mistakes

- `Pending`ni error deb tushunish. Bu xato emas, future hali tayyor emas degani.
- `Ready`dan keyin future'ni qayta ishlatish mumkin deb o'ylash. Future tugagach, odatda completed holatda bo'ladi.
- `Pending` qaytarib, waker signali bermaslik.

## Related Concepts

- [[future|Future]]
- [[polling]]
- [[waker|Waker]]
- [[task-context|Context]]
- [[executor]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

