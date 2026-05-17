---
title: "Threads"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, concurrency, threads]
source_count: 4
---

# Threads

## Short Definition

`std::thread` moduli orqali parallel bajariladigan mustaqil execution unitlar. Rust **1:1 modelidan** foydalanadi: har bir language thread bitta OS thread.

## Why It Matters

Ko'p yadrodan foydalanish va uzluksiz ishni parallel bajarish imkonini beradi. Lekin xavflari ham bor: race conditions, deadlocks, va qiyin reproducible buglar. Rust ownership va type system yordamida bu xavflarni **compile time'da** tutadi — runtime debugging o'rniga compiler xato ko'rsatadi.

## Mental Model

Har bir thread o'z stack'iga ega mustaqil execution context. **Main thread tugasa hamma spawned threadlar ham to'xtaydi** — agar `join` qilinmagan bo'lsa.

OS scheduler threadlarni almashtirish tartibini kafolatlamaydi; shuning uchun spawned threadlar hamma vaqt ham ishlab tugamaydi.

## Syntax and Examples

### Asosiy thread yaratish

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("spawned thread: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("main thread: {i}");
        thread::sleep(Duration::from_millis(1));
    }
    // main tugadi → spawned thread ham to'xtaydi!
}
```

### JoinHandle bilan to'liq bajarilishini kutish

```rust
use std::thread;

fn main() {
    let handle = thread::spawn(|| {
        println!("spawned thread ishlayapti");
    });

    // ... boshqa ish ...

    handle.join().unwrap(); // spawned thread tugaguncha kut
}
```

### move closure bilan data o'tkazish

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];
    let handle = thread::spawn(move || {
        println!("vektor: {v:?}");
    });
    handle.join().unwrap();
}
```

### Request-per-thread pattern

```rust
for stream in listener.incoming() {
    let stream = stream.unwrap();

    thread::spawn(move || {
        handle_connection(stream);
    });
}
```

Bu concurrency beradi, lekin har request uchun alohida thread yaratgani uchun cheksiz growth xavfi bor. Amaliyroq variant: [[thread-pool]].

## Common Mistakes

- **`join` ni unutish**: spawned thread ishlab tugamadan main tugatadi. `JoinHandle`ni saqlab, `.join()` chaqirish shart.
- **`move` siz closure**: `thread::spawn(|| ...)` closure'da tashqi o'zgaruvchi ishlatilsa E0373 xatosi.
- **`join` joyini noto'g'ri tanlash**: `join`ni for loopdan *oldin* qo'ysangiz sequential ishlaydi; keyin qo'ysangiz interleaved. Qarang: [[join-handle]].

## Threadlar orasida muloqot

Threadlarning natijalarini main thread'ga uzatish uchun ikki asosiy pattern:
- **Message passing** — [[channels]] (`mpsc::channel`) orqali xabar yuborish.
- **Shared state** — `Mutex<T>` + `Arc<T>` (ch16.3).

Message passing ko'pincha xavfsizroq, chunki ownership transfer aniq.

Threadlardan amaliy foydalanishning keng patternlaridan biri - fixed-size [[thread-pool]]. Bu yerda worker threadlar oldindan yaratilib, keyin kelgan job'larni queue'dan olib bajaradi.

## Related Concepts

- [[concurrency]]
- [[join-handle]]
- [[move-closures-threads]]
- [[channels]]
- [[message-passing]]
- [[thread-pool]]
- [[worker-thread]]
- [[send-trait]]
- [[sync-trait]]
- [[data-race]]
- [[closures]]
- [[e0373-closure-may-outlive-current-function]]

## Sources

- [[wiki/sources/16-fearless-concurrency]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
