---
title: "JoinHandle"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-18
tags: [rust, concurrency, threads]
source_count: 3
---

# JoinHandle

## Short Definition

`thread::spawn` qaytaradigan owned value — `JoinHandle<T>`. `.join()` chaqirilganda joriy thread spawned thread tugagunicha **block** bo'ladi.

## Why It Matters

Handle saqlanmasa, thread "detached" holda ishlaydi va main thread tugatilishi bilan o'lishi mumkin. `JoinHandle::join` thread to'liq bajarilishini kafolatlaydi.

## Mental Model

`JoinHandle` — "thread ning qo'li". Ushlab tursangiz, u tugaguncha kutasiz. Qo'yib yuborsangiz (handle ni drop qilsangiz), ishiga aralashmaysiz.

Muhim nuance: `.join()` handle'ni borrow qilmaydi, balki consume qiladi. Demak `JoinHandle` ko'pincha "bir martalik ticket" sifatida ishlaydi.

`thread::spawn` ham, [[thread-builder]] ham `JoinHandle` qaytaradi. Scoped thread'larda esa explicit `JoinHandle` bilan ishlamasdan scope exit paytida auto-join semantics olinadi.

## Syntax and Examples

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();
}
```

`.join()` `Result<T, Box<dyn Any + Send>>`ni qaytaradi — spawned closure panic bilan tugasa `Err`.

### `join` joyi execution tartibini belgilaydi

```rust
let handle = thread::spawn(|| { /* ... */ });
for i in 1..5 { /* main ish */ }
handle.join().unwrap();
```

```rust
let handle = thread::spawn(|| { /* ... */ });
handle.join().unwrap();
for i in 1..5 { /* main ish */ }
```

### Cleanup paytidagi ownership

```rust
for worker in &mut self.workers {
    worker.thread.join().unwrap(); // E0507
}
```

Bu kod ishlamaydi, chunki `join(self)` `JoinHandle` ownership'ini oladi. Borrowed struct field ichidan uni to'g'ridan-to'g'ri move qilib bo'lmaydi. Shu sabab 21.3 `Vec::drain(..)` yoki `Option::take()` bilan ownership extraction patternlarini ishlatadi.

## Common Mistakes

- **`join` ni loopdan oldin qo'yish**: natijada ikkala thread parallel emas, ketma-ket ishlaydi.
- **`unwrap` bilan panic ni yutish**: spawned thread panic qilsa `join` `Err` qaytaradi.
- **`join` borrow bilan ishlaydi deb o'ylash**: cleanup code'da bu ko'pincha [[e0507-cannot-move-out-of-borrowed-content|E0507]]ga olib keladi.

## Related Concepts

- [[threads]]
- [[thread-builder]]
- [[scoped-threads]]
- [[concurrency]]
- [[closures]]
- [[result]]
- [[worker-thread]]
- [[thread-pool]]
- [[e0507-cannot-move-out-of-borrowed-content|E0507]]

## Sources

- [[16-1-using-threads-to-run-code-simultaneously]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
