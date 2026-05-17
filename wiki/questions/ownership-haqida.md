---
title: "Ownership nima?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, ownership]
source_count: 2
---

# Ownership nima?

## Answer

Ownership Rustning memory management modeli. Har bir value'ning bitta owner'i bo'ladi, owner scope'dan chiqqanda value avtomatik `drop` qilinadi.

Asosiy qoidalar:

- har bir value'ning owner'i bor
- bir vaqtda faqat bitta owner bo'ladi
- owner scope'dan chiqqanda value drop qilinadi

```rust
let s1 = String::from("hello");
let s2 = s1; // ownership moves to s2
```

Bu yerda `s1` endi valid emas, chunki `String` ownership'i `s2`ga move bo'ldi. Agar eski bindingni ishlatsangiz, compiler `E0382 borrow of moved value` beradi.

Ownershipning amaliy foydasi shuki, Rust garbage collector ishlatmasdan memory safety beradi. Kim heap data'ni tozalashini compile time'da aniqlaydi, shuning uchun double free va use-after-free kabi xatolarni erta ushlaydi.

Function argumentlari ham ownershipni move qilishi mumkin:

```rust
fn takes_ownership(some_string: String) {
    println!("{some_string}");
}
```

`String` kabi heap-backed type functionga berilganda ownership transfer bo'lishi odatiy hol. `i32` kabi `Copy` type'lar esa copy bo'ladi.

Ownershipni to'g'ri tushunsa, keyingi mavzular ancha osonlashadi: [[borrowing]] ownershipni transfer qilmasdan foydalanish yo'lini beradi, [[reference]] esa shu kirish mexanizmini ifodalaydi.

## Evidence

- [[ownership]]: Rust memoryni boshqaradigan compile-time rules sistemi.
- [[4-1-what-is-ownership]]: ownershipning uchta asosiy qoidasi va move semantics.
- [[move-semantics]]: assignment va function call paytida move qanday ishlashi.
- [[copy-trait]]: `Copy` type'lar move emas, copy bo'lishi.
- [[drop]]: owner scope'dan chiqqanda automatic cleanup.

## Follow-up

- `ownership` va `borrowing` orasidagi farqni misollar bilan ko'rish.
- `String` va `&str` ni ownership nuqtayi nazaridan solishtirish.
- `clone()` qachon kerak bo'lishini alohida ajratish.

## Related Pages

- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[move-semantics]]
- [[copy-trait]]
- [[drop]]
- [[e0382-borrow-of-moved-value]]
