---
title: "drop"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, ownership, memory, smart-pointers]
source_count: 6
---

# drop

## Short Definition

`drop` Rust value scope'dan chiqqanda cleanup qilish uchun avtomatik chaqiriladigan function/pattern.

## Why It Matters

Heap allocation ishlatadigan values, masalan `String`, owner scope'dan chiqqanda memoryni allocatorga qaytarishi kerak. Rust buni deterministic tarzda qiladi.

Smart pointerlarda [[drop|Drop trait]] ayniqsa muhim: `Box<T>` scope'dan chiqqanda stackdagi box ham, heapdagi inner data ham cleanup qilinadi.

Concurrency kodda ham `Drop` juda amaliy: `ThreadPool` scope'dan chiqayotganda worker threadlar tugashini kutish, channelni yopish, va orderly shutdown qilish aynan shu hook orqali yoziladi.

## Mental Model

```rust
{
    let s = String::from("hello");
} // s goes out of scope; drop is called
```

## Syntax and Examples

`drop` odatda qo'lda chaqirilmaydi; ownership/scope tugaganda Rust chaqiradi. `String` author'i cleanup code'ni drop logiciga joylaydi.

Backend beginner source `std::mem::drop`ni ownership orqali early cleanup signalining sodda misoli sifatida ham ko'rsatadi: funksiya argumentni move qilib oladi va uni boshqa joyga uzatmaydi.

Vector scope'dan chiqqanda vectorning o'zi va ichidagi elementlar dropped bo'ladi:

```rust
{
    let v = vec![1, 2, 3, 4];
} // v and its elements are dropped here
```

Box scope'dan chiqqanda heap allocation ham tozalanadi:

```rust
{
    let b = Box::new(5);
} // b and the heap value are dropped here
```

### Drop Trait

Har qanday type uchun custom cleanup kodi yozish mumkin. `Drop` prelude da — `use` shart emas:

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}
```

O'zgaruvchilar **yaratilish tartibining teskarisida** drop qilinadi.

### Erta tozalash: std::mem::drop

`Drop::drop` metodini qo'lda chaqirish taqiqlangan — [[e0040-explicit-use-of-destructor|E0040]] xatosi chiqadi, sababi **double free** xavfi. Erta tozalash kerak bo'lsa (masalan, lock bo'shatish), prelude dagi `drop()` funksiyasidan foydalaning:

```rust
let c = CustomSmartPointer { data: String::from("some data") };
drop(c);  // xavfsiz erta tozalash
```

### ThreadPool cleanup

21.3 da `Drop` oddiy memory cleanupdan kengroq ma'no oladi:

```rust
impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());

        for worker in &mut self.workers {
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

Bu yerda `drop(...)` explicit signal sifatida ishlatiladi: sender yo'qolishi workerlar uchun channel yopilganini bildiradi.

## Common Mistakes

- Garbage collector bor deb o'ylash.
- Move qilingan oldingi binding drop qiladi deb o'ylash; ownership boshqa bindingga o'tgan bo'ladi.
- `c.drop()` chaqirib bo'ladi deb o'ylash — bu E0040 xatosi beradi; o'rniga `drop(c)` ishlatiladi.
- `std::mem::drop` va `Drop::drop` metodini bir narsa deb o'ylash — ikki alohida funksiya.
- Cleanup faqat memory bilan bog'liq deb o'ylash — thread lifecycle va channel shutdown ham `Drop`ga bog'lanishi mumkin.

## Related Concepts

- [[ownership]]
- [[scope]]
- [[string-type|String]]
- [[vector|Vec<T>]]
- [[box-t|Box<T>]]
- [[smart-pointers]]
- [[move-semantics|move semantics]]
- [[e0040-explicit-use-of-destructor|E0040]]
- [[thread-pool]]
- [[channels]]
- [[join-handle]]
- [[graceful-shutdown]]

## Sources

- [[4-1-what-is-ownership]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[wiki/sources/15-smart-pointers]]
- [[15-3-running-code-on-cleanup-with-the-drop-trait]]
- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
