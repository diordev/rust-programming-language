---
title: "Borrowing"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership, borrowing]
source_count: 5
---

# Borrowing

## Short Definition

Borrowing reference yaratish orqali value'dan ownershipni olmasdan foydalanish.

## Why It Matters

Ownershipni functionga berib keyin qaytarish ceremonyini kamaytiradi. Borrowing Rustga memory safety va aliasing/mutation rulesni compile time'da enforce qilish imkonini beradi.

## Mental Model

```rust
let s1 = String::from("hello");
let len = calculate_length(&s1);
println!("The length of '{s1}' is {len}.");
```

`calculate_length` `s1`ni borrow qiladi, owner bo'lmaydi.

Struct functions va methodsda borrowing ko'p uchraydi: `area(rectangle: &Rectangle)` va `fn area(&self)` instance ownershipini olmasdan fieldsni o'qiydi. Method receiver uchun Rust [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]]ni qo'llashi mumkin.

Collectionsda borrowing element references valid qolishini ham himoya qiladi. Masalan, vector elementiga reference bor paytda `push` qilish rad etiladi, chunki `push` reallocation qilib elementlarni boshqa heap joyiga ko'chirishi mumkin.

[[hash-map|HashMap]]da [[entry-api|entry API]] ham borrowingni ko'rsatadi: `entry(...).or_insert(...)` value'ga mutable reference qaytaradi. Shu reference orqali value update qilinadi, lekin reference active turgan paytda mapni boshqa yo'l bilan borrow/mutate qilishga urinish borrow checker qoidalariga tushadi.

## Syntax and Examples

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Mutable borrow:

```rust
fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Hash map entry pattern:

```rust
let count = map.entry(word).or_insert(0);
*count += 1;
```

## Common Mistakes

- Immutable borrow orqali data mutate qilish.
- Bir vaqtda ikki mutable borrow yaratish.
- Immutable borrow hali active bo'lgan paytda mutable borrow olish.
- Method receiver `&self` bo'lsa ownership ko'chadi deb o'ylash.
- Collection elementiga reference bor paytda collectionni mutate qilish safe bo'ladi deb o'ylash.
- `or_insert` qaytargan mutable reference active paytda mapni yana alohida mutable borrow qilishga urinish.

## Related Concepts

- [[reference]]
- [[mutable-reference|mutable reference]]
- [[ownership]]
- [[data-race|data race]]
- [[methods]]
- [[self-parameter|self parameter]]
- [[automatic-referencing-and-dereferencing|automatic referencing and dereferencing]]
- [[vector|Vec<T>]]
- [[vector-indexing|vector indexing]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[5-2-an-example-program-using-structs-the-rust-programming-language]]
- [[5-3-methods-the-rust-programming-language]]
- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps-the-rust-programming-language]]
