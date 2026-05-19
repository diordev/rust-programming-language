---
title: "minimal waker executor"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, executor, example]
source_count: 1
---

# minimal waker executor

## Context

Source'dagi executor real runtime emas. U `Future::poll`, `Poll::Pending`, [[waker|Waker]], va queue'ga qaytarish oqimini ko'rsatadigan o'quv modeli.

## Code

```rust
use std::{
    collections::VecDeque,
    future::Future,
    pin::Pin,
    sync::{Arc, Mutex},
    task::{Context, Poll, Wake},
};

type BoxFuture = Pin<Box<dyn Future<Output = ()> + Send + 'static>>;

struct Task {
    future: Mutex<Option<BoxFuture>>,
}

struct TaskWaker {
    task: Arc<Task>,
    queue: Arc<Mutex<VecDeque<Arc<Task>>>>,
}

impl Wake for TaskWaker {
    fn wake(self: Arc<Self>) {
        self.queue.lock().unwrap().push_back(self.task.clone());
    }
}

fn poll_task(task: Arc<Task>, queue: Arc<Mutex<VecDeque<Arc<Task>>>>) {
    let mut future_slot = task.future.lock().unwrap();
    let Some(mut future) = future_slot.take() else {
        return;
    };

    let waker = Arc::new(TaskWaker {
        task: task.clone(),
        queue,
    })
    .into();
    let mut cx = Context::from_waker(&waker);

    match future.as_mut().poll(&mut cx) {
        Poll::Ready(()) => {}
        Poll::Pending => {
            *future_slot = Some(future);
        }
    }
}
```

## Explanation

- Task future'ni `Pin<Box<dyn Future<Output = ()>>>` sifatida saqlaydi.
- Executor `poll_task` orqali future'ni bir qadam oldinga siljitadi.
- `Pending` bo'lsa future task ichiga qaytariladi.
- `Waker::wake` taskni queue'ga yana qo'shadi.
- Real runtime bundan ancha murakkab: timer, I/O driver, cancellation, fairness, va thread pool policy kerak.

## Related Concepts

- [[executor]]
- [[future|Future]]
- [[polling]]
- [[poll-enum|Poll]]
- [[waker|Waker]]
- [[task-context|Context]]
- [[pin|Pin]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]
