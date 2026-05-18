---
title: "MutexGuard Lifetime"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-18
tags: [rust, mutex, lifetimes, concurrency]
source_count: 2
---

# MutexGuard Lifetime

## Short Definition

`Mutex::lock()` qaytaradigan `MutexGuard<T>` qachon drop bo'lishi lock qachon qo'yilishini belgilaydi. Concurrency'ning haqiqiy xulqi ko'pincha aynan shu scope bilan hal bo'ladi.

## Why It Matters

Code compile bo'lishi yetarli emas; lock qancha vaqt ushlanayotganini ham to'g'ri boshqarish kerak. Aks holda technically multithreaded bo'lgan dastur amalda serial ishlashi mumkin.

## Mental Model

`Mutex`ning public `unlock()` metodi yo'q. Lock RAII asosida yashaydi: `MutexGuard` bor ekan, lock bor. `MutexGuard` drop bo'lganda unlock avtomatik bo'ladi.

## Syntax and Examples

### To'g'ri variant

```rust
let job = receiver.lock().unwrap().recv().unwrap();
job();
```

Bu yerda `let` statement tugashi bilan temporary `MutexGuard` drop bo'ladi.

### Noto'g'ri variant: `while let`

```rust
while let Ok(job) = receiver.lock().unwrap().recv() {
    job();
}
```

Bu kod compile bo'ladi, lekin `MutexGuard` loop body davomida yashab qoladi.

### Shu muammo `if let`da ham bo'ladi

```rust
if let Some(item) = list.lock().unwrap().pop() {
    process(item);
}
```

Bu yerda guard `process(item)` tugaguncha yashaydi.

## Common Mistakes

- **`while let` yoki `if let` temporary'lari darhol drop bo'ladi deb o'ylash**.
- **Lock scope'ini faqat ko'z bilan taxmin qilish**.
- **Sekin ishni lock ichida bajarib yuborish**.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[while-let]]
- [[if-let]]
- [[match]]
- [[worker-thread]]
- [[thread-pool]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-multithreading]]
