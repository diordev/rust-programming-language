---
title: "Task Context"
type: concept
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, futures]
source_count: 1
---

# Task Context

## Short Definition

`Context<'_>` - `Future::poll` argumenti bo'lib, future'ga joriy task uchun [[waker|Waker]]ga kirish beradi.

## Why It Matters

Future `Pending` qaytargandan keyin executor uni qachon davom ettirishni bilishi kerak. `Context` future ichiga waker uzatishning standard yo'li.

## Mental Model

`Context` - executor future'ga beradigan kichik execution envelope. Bu envelope ichidagi eng muhim narsa waker.

## Syntax and Examples

```rust
use std::task::{Context, Poll};
use std::pin::Pin;
use std::future::Future;

impl Future for MyFuture {
    type Output = ();

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<()> {
        let waker = cx.waker().clone();
        register_waker_somewhere(waker);
        Poll::Pending
    }
}
```

## Common Mistakes

- `Context`ni application context yoki error context bilan aralashtirish. Bu `std::task::Context`, ya'ni async task polling konteksti.
- `Context`dan waker olib, uni ishlatmaslik. `Pending` future qayta uyg'onishi uchun waker real event bilan bog'lanishi kerak.

## Related Concepts

- [[waker|Waker]]
- [[future|Future]]
- [[polling]]
- [[executor]]
- [[poll-enum|Poll]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

