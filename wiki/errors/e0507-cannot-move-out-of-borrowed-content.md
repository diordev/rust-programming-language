---
title: "E0507: cannot move out of borrowed content"
type: error
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, compiler-error, ownership, move]
source_count: 1
---

# E0507: cannot move out of borrowed content

## Symptom

Borrow qilingan value ichidan owned field'ni move qilishga urinilganda compiler `E0507` beradi.

```text
error[E0507]: cannot move out of `worker.thread` which is behind a mutable reference
```

## Cause

Sizda butun struct ownership'i yo'q, faqat borrow'i bor (`&mut worker`). Lekin field methodi owned `self` talab qiladi. 21.3 misolida `JoinHandle::join(self)` handle'ni consume qiladi, shuning uchun `worker.thread.join()` borrowed `worker` orqali ishlamaydi.

## Fix Pattern

Owned qiymatni borrow ichidan xavfsiz chiqarish kerak:

- `Option::take()` bilan;
- `Vec::drain(..)` bilan;
- yoki umumiy holatda `mem::replace`/`mem::take` bilan.

## Minimal Example

Failing:

```rust
for worker in &mut self.workers {
    worker.thread.join().unwrap(); // E0507
}
```

Fixed with `Option::take()`:

```rust
if let Some(thread) = worker.thread.take() {
    thread.join().unwrap();
}
```

Fixed with `Vec::drain(..)`:

```rust
for worker in self.workers.drain(..) {
    worker.thread.join().unwrap();
}
```

## Related Concepts

- [[ownership]]
- [[move-semantics]]
- [[join-handle]]
- [[option]]
- [[vector|Vec<T>]]
- [[thread-pool]]
- [[graceful-shutdown]]

## Sources

- [[wiki/sources/21-3-graceful-shutdown-and-cleanup|21.3]]
