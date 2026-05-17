---
title: "Worker Thread"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, concurrency, threads, worker]
source_count: 2
---

# Worker Thread

## Short Definition

Thread pool ichidagi bitta doimiy thread bo'lib, u queue'dan job olib bajaradi. Chapter 21.2 design'ida worker `id` va `JoinHandle<()>` saqlaydi; 21.3 cleanup bosqichida esa `Option<JoinHandle<()>>` patterni ham paydo bo'ladi.

## Why It Matters

`ThreadPool` abstraktsiyasi ichkarida konkret ishni aynan workerlar bajaradi. Worker struct debug/log qilishni, thread lifecycle'ni, va job execution loop'ni aniq nomlangan birlikka ajratadi.

## Mental Model

Worker - "tayyor kutib turgan ijrochi". U taskni o'zi tanlamaydi; pool va queue orqali topshirilgan ishni oladi, bajaradi, keyin yana kutadi. Shutdown paytida u "endi ish yo'q" signalini channel disconnect orqali oladi.

## Syntax and Examples

```rust
struct Worker {
    id: usize,
    thread: thread::JoinHandle<()>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || {
            loop {
                let job = receiver.lock().unwrap().recv().unwrap();
                println!("Worker {id} got a job; executing.");
                job();
            }
        });

        Worker { id, thread }
    }
}
```

### Shutdown-aware worker loop

```rust
let thread = thread::spawn(move || loop {
    let message = receiver.lock().unwrap().recv();

    match message {
        Ok(job) => {
            println!("Worker {id} got a job; executing.");
            job();
        }
        Err(_) => {
            println!("Worker {id} disconnected; shutting down.");
            break;
        }
    }
});
```

## Common Mistakes

- **Worker'ni faqat `JoinHandle` deb ko'rish**: `id` kabi metadata debugging uchun muhim.
- **Receiver lock'ini `job()` davomida ushlab turish**: boshqa workerlar job ola olmaydi.
- **Worker thread doim band deb o'ylash**: `recv()` bloklaydi, ish bo'lmasa worker kutadi.
- **Disconnect signalni `unwrap()` bilan panic qilish**: graceful shutdown uchun `Err(_)` branch kerak.

## Related Concepts

- [[thread-pool]]
- [[job-queue]]
- [[mutexguard-lifetime]]
- [[threads]]
- [[channels]]
- [[join-handle]]
- [[graceful-shutdown]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
