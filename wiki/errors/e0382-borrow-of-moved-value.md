---
title: "E0382 borrow of moved value"
type: error
status: active
created: 2026-05-06
updated: 2026-05-16
tags: [rust, compiler-error, ownership]
source_count: 6
---

# E0382 borrow of moved value

## Symptom

Value move bo'lgandan keyin eski bindingni ishlatishga uringanda compiler `error[E0382]: borrow of moved value` chiqaradi.

## Cause

`String` kabi type `Copy` emas. Assignment yoki function call ownershipni yangi binding/parameterga move qiladi. Oldingi binding invalid bo'ladi, chunki aks holda ikkita owner bitta heap allocationni free qilishga urinar edi.

Backend beginner ownership source buni eng sodda shaklda ko'rsatadi: `let s2 = s1;`dan keyin `println!("{s1}")` mumkin emas.

[[hash-map|HashMap]] inserti ham shu xatoga olib kelishi mumkin: owned `String` key yoki value `insert` orqali mapga berilganda ownership mapga move bo'ladi. Keyin eski binding ishlatilsa E0382 chiqadi.

## Fix Pattern

Agar independent copy kerak bo'lsa, explicit `clone()` ishlating:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("{s1}, {s2}");
```

Agar ownership kerak bo'lmasa, keyingi borrowing sectiondan keyin reference ishlatiladi:

```rust
// Borrowing details are covered in Chapter 4.2.
```

## Minimal Example

Failing:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{s1}, world!");
}
```

Fixed with clone:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {s1}, s2 = {s2}");
}
```

Hash map insert move:

```rust
use std::collections::HashMap;

fn main() {
    let field_name = String::from("Favorite color");
    let field_value = String::from("Blue");

    let mut map = HashMap::new();
    map.insert(field_name, field_value);

    println!("{field_name}");
}
```

## Thread kontekstida E0382

`move` closure bilan qiymat spawned thread'ga ko'chsa, asl scope'da ishlatib bo'lmaydi:

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];
    let handle = thread::spawn(move || println!("{v:?}")); // v thread'ga o'tdi
    drop(v); // XATO: E0382 — v allaqachon moved
    handle.join().unwrap();
}
```

`move` E0373 ni to'g'irlaydi, lekin agar asl scope ham qiymatni ishlatmoqchi bo'lsa, E0382 chiqadi. Yechim: `clone()` qiling, clone'ni thread'ga bering.

## Channel `send` kontekstida E0382

`tx.send(val)` qiymat ownership'ini channel'ga ko'chiradi. Yuborilgandan keyin asl scope'da ishlatib bo'lmaydi:

```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();
thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();
    println!("val is {val}"); // XATO: E0382 — send val'ni move qildi
});
```

Bu **xususiyat**, xato emas: yuborilgan qiymat boshqa thread tomonidan modify yoki drop qilinishi mumkin, shuning uchun ownership saqlanishi kerak.

## `Receiver`ni worker loop'da move qilish

Thread pool qurayotganda quyidagi pattern ham E0382 beradi:

```rust
let (sender, receiver) = mpsc::channel();

for id in 0..size {
    workers.push(Worker::new(id, receiver)); // XATO
}
```

`receiver` `Copy` emas. U loopning birinchi iteratsiyasida `Worker::new` ga move bo'ladi, keyingi iteratsiyada esa eski binding endi yo'q. Bundan tashqari std `mpsc` modeli single-consumer bo'lgani uchun `Receiver`ni oddiy clone qilish ham mumkin emas.

To'g'ri fix:

```rust
let receiver = Arc::new(Mutex::new(receiver));

for id in 0..size {
    workers.push(Worker::new(id, Arc::clone(&receiver)));
}
```

Bu yerda ownership bitta `Arc` orqali bo'linadi, actual `recv()` access esa `Mutex` bilan serialized qilinadi.

## Related Concepts

- [[4-1-what-is-ownership]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[hash-map|HashMap]]
- [[move-vs-clone]]
- [[move-closures-threads]]
- [[e0373-closure-may-outlive-current-function]]
- [[channels]]
- [[arc-t|Arc<T>]]
- [[mutex-t|Mutex<T>]]
- [[thread-pool]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[16-1-using-threads-to-run-code-simultaneously]]
- [[16-2-transfer-data-between-threads-with-message-passing]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
- [[wiki/sources/rust-for-backend-developers-ownership]]
