---
title: "16.1. Using Threads to Run Code Simultaneously"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, concurrency, threads, move-closures]
source_count: 1
source_path: "raw/books/the_rust_programming_language/16.1. Using Threads to Run Code Simultaneously.md"
---

# 16.1. Using Threads to Run Code Simultaneously

## Source

- **Fayl:** `raw/books/the_rust_programming_language/16.1. Using Threads to Run Code Simultaneously.md`
- **URL:** https://doc.rust-lang.org/stable/book/ch16-01-threads.html

## Detailed Summary

### Process vs thread

*Process* — OS tomonidan boshqariladigan ishlab turgan dastur. *Thread* — process ichidagi mustaqil execution unit. Masalan, web server bir vaqtda ko'p so'rovni qayta ishlash uchun ko'p thread ishlatadi.

### Threading muammolari

Threadlar parallel ishlashi tufayli:
- **Race conditions** — threadlar ma'lumotlarga tartibsiz kirishadi
- **Deadlocks** — ikki thread bir-birini kutib qoladi
- **Hard-to-reproduce bugs** — faqat ayrim sharoitlarda namoyon bo'ladigan xatolar

### 1:1 thread modeli

Rust standart kutubxonasi **1:1 model** ishlatadi: har bir language thread bitta OS thread. Boshqa modellari (green threads va boshqalar) alohida cratelarda mavjud. Async/await esa alohida yondashuv (ch17).

### `thread::spawn`

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("spawned: {i}");
            thread::sleep(Duration::from_millis(1));
        }
    });
    for i in 1..5 {
        println!("main: {i}");
        thread::sleep(Duration::from_millis(1));
    }
    // main tugadi → spawned ham to'xtaydi (9 ga yetmaydi)
}
```

`thread::sleep` thread'ni bir muddat to'xtatib, OS boshqa threadga o'tish imkonini beradi.

### `JoinHandle<T>` va `.join()`

`thread::spawn` `JoinHandle<T>` qaytaradi. `.join()` chaqirilganda joriy thread spawned thread tugagunicha block bo'ladi:

```rust
let handle = thread::spawn(|| { /* ... */ });
// ... boshqa ish ...
handle.join().unwrap(); // spawned to'liq ishlaydi
```

**Join joyi muhim:** `join` for loopdan *oldin* bo'lsa — sequential; *keyin* bo'lsa — interleaved. Qarang: [[thread-spawn-basic]].

### `move` closures

Spawned thread parent thread ma'lumotlarini ishlatishi kerak bo'lsa, Rust avval borrow qilishga urinadi — ammo thread lifetime noma'lum bo'lgani uchun E0373 xatosi:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(|| println!("{v:?}")); // E0373
```

`move` bilan closure ownership oladi:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(move || println!("{v:?}")); // OK
```

**`move` mantiqiy xatoni to'g'irlamaydi.** Agar asl scope ham qiymatni ishlatmoqchi bo'lsa:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(move || println!("{v:?}"));
drop(v); // E0382: v allaqachon moved
```

Ownership qoidalari saqlanadi — `move` faqat transferni aniq belgilaydi.

## Key Concepts

- [[threads]]
- [[join-handle]]
- [[move-closures-threads]]
- [[concurrency]]
- [[closures]]
- [[ownership]]

## Code Examples

- [[thread-spawn-basic]] — Listing 16-1, 16-2, 16-3 taqqoslamasi
- E0373 va E0382 — [[move-closures-threads]] sahifasida

## Exercises or Practice Ideas

1. `thread::spawn` ni `join` siz ishlatib, spawned thread ishlamasligini kuzating.
2. `JoinHandle`ni loopdan oldin va keyin `join` qilib, farqni kuzating.
3. `move` siz closure bering va E0373 ni o'qing, keyin `move` bilan to'g'irlang.
4. Listing 16-4 ni `move` bilan tuzatishga urining — E0382 nima uchun chiqishini tushuntiring.
5. Uchta thread spawn qilib, ularni alohida `JoinHandle` orqali to'xtating.

## Questions Raised

- `thread::spawn` bilan qaytarilgan `JoinHandle` drop bo'lganda nima bo'ladi?
- Green thread va 1:1 thread modeli o'rtasidagi trade-offlar nimadan iborat?

## Links To Update

- [[wiki/chapters/16-fearless-concurrency]]
- [[threads]]
- [[join-handle]]
- [[move-closures-threads]]
- [[e0373-closure-may-outlive-current-function]]
