---
title: "MutexGuard Lifetime"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, mutex, lifetimes, concurrency]
source_count: 1
---

# MutexGuard Lifetime

## Short Definition

`Mutex::lock()` qaytaradigan `MutexGuard<T>` qachon drop bo'lishi lock qachon qo'yilishini belgilaydi. Concurrency'ning haqiqiy xulqi ko'pincha aynan shu scope bilan hal bo'ladi.

## Why It Matters

Code compile bo'lishi yetarli emas; lock qancha vaqt ushlanayotganini ham to'g'ri boshqarish kerak. Aks holda technically multithreaded bo'lgan dastur amalda serial ishlashi mumkin.

## Mental Model

`Mutex`ning public `unlock()` metodi yo'q. Lock RAII asosida yashaydi: `MutexGuard` bor ekan, lock bor. `MutexGuard` drop bo'lganda unlock avtomatik bo'ladi.

## Syntax and Examples

### To'g'ri variant: lock statement oxirida qo'yiladi

```rust
let job = receiver.lock().unwrap().recv().unwrap();
job();
```

Bu yerda `let` statement tugashi bilan temporary `MutexGuard` drop bo'ladi, keyin `job()` lock'siz bajariladi.

### Noto'g'ri variant: `while let` lock'ni uzoq ushlab turadi

```rust
while let Ok(job) = receiver.lock().unwrap().recv() {
    job();
}
```

Bu kod compile bo'ladi, lekin `MutexGuard` loop body davomida yashab qoladi. Natijada `job()` ishlayotgan paytda ham boshqa workerlar lock ola olmaydi.

## Common Mistakes

- **`while let` yoki `match` temporary'lari darhol drop bo'ladi deb o'ylash**.
- **Lock scope'ini faqat ko'z bilan taxmin qilish**: actual owner `MutexGuard`.
- **`job()` yoki boshqa sekin ishni lock ichida bajarib yuborish**.

## Related Concepts

- [[mutex-t|Mutex<T>]]
- [[while-let]]
- [[if-let]]
- [[match]]
- [[worker-thread]]
- [[thread-pool]]

## Sources

- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
