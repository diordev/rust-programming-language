---
title: "E0382 borrow of moved value"
type: error
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, compiler-error, ownership]
source_count: 2
---

# E0382 borrow of moved value

## Symptom

Value move bo'lgandan keyin eski bindingni ishlatishga uringanda compiler `error[E0382]: borrow of moved value` chiqaradi.

## Cause

`String` kabi type `Copy` emas. Assignment yoki function call ownershipni yangi binding/parameterga move qiladi. Oldingi binding invalid bo'ladi, chunki aks holda ikkita owner bitta heap allocationni free qilishga urinar edi.

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

## Related Concepts

- [[4-1-what-is-ownership-the-rust-programming-language]]
- [[ownership]]
- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[hash-map|HashMap]]
- [[move-vs-clone]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
