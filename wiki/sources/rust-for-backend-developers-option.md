---
title: "Option - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, option]
source_count: 1
---

# Option - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/34. Option.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/option.html

## Detailed Summary

Bu source `Option<T>`ni shunchaki standard enum sifatida emas, absence modelining Rustcha javobi sifatida quradi. Kirish qismidagi uchta eski yondashuv muhim: sentinel value, qo'shimcha boolean flag, va null pointer. Source bu uchalasini birma-bir rad etib, `Option<T>`ning amaliy yutug'ini ko'rsatadi: absence type system ichiga ko'tariladi.

`Option<T>` definition'i source'da juda to'g'ri joyda keladi: `Some(T)` va `None` generic enum modeli oldingi generics bobi bilan ulanadi. Muhim durable takeaway: `Option` tashqi flag emas, enum bo'lgani uchun discriminantni o'zi bilan olib yuradi. Shu sabab alohida `is_empty` field yoki maxsus reserved value kerak emas.

Source `FullName { middle_name: Option<String> }` va `get_record_by_id(id) -> Option<Record>` misollari bilan Option'ning ikki klassik use case'ini beradi: optional field va optional lookup result. Bu ikkalasi keyingi Rust code'ning juda katta qismini tashkil qiladi.

Value extraction bo'limi to'rt qatlamga bo'linadi: `unwrap`, `unwrap_or`, `match`, va `if let`. Bu yerda muhim caveat saqlanishi kerak: `unwrap` "unsafe" emas, Rust `unsafe` semanticsiga aloqasi yo'q; u `None` holatda panic qilishi mumkin bo'lgan convenience shortcut, xolos. Shuning uchun beginner code'da ham uni error handling deb yozish noto'g'ri.

Source'ning eng foydali qismi Option combinators. `map` bo'limi Option ustidagi transformation modelini ko'rsatadi: `Some(v)` bo'lsa closure ishlaydi, `None` bo'lsa hech narsa bo'lmaydi. `get_user_name_by_id(id).map(|user| user.name)` misoli optional objectdan optional field extractionni juda amaliy ko'rinishda beradi.

`flatten` bo'limi source'da bejiz emas. `map` ba'zan `Option<Option<T>>` hosil qiladi; `flatten` shuni bitta qatlamga tushiradi. `get_user_middle_name_by_id(id)` misoli `Option<User>` ichidan `Option<String>` olishda nega nested option paydo bo'lishini ochiq ko'rsatadi.

`and_then` esa `map + flatten` sifatida beriladi. Bu yerda saqlanadigan signal: `and_then` optional chaining uchun asosiy combinator. Ya'ni closure'ning o'zi Option qaytarganda, nested wrapping yaratmasdan keyingi qadamga o'tish mumkin.

Source bo'ylab yana bitta nozik nuqta bor: `map` va `and_then` closure yoki function pointer qabul qilishi mumkin. Bu oldingi anonymous functions materialini Option bilan amaliy bog'laydi.

## Key Concepts

- [[option|Option]]
- [[generic-enums|generic enums]]
- [[enums]]
- [[unwrap]]
- [[match]]
- [[if-let|if let]]
- [[closures]]

## Code Examples

```rust
struct FullName {
    first_name: String,
    last_name: String,
    middle_name: Option<String>,
}
```

```rust
let o: Option<i32> = None;
let i: i32 = o.unwrap_or(1);
```

```rust
fn get_user_name_by_id(id: u64) -> Option<String> {
    get_user_by_id(id).map(|user| user.name)
}
```

```rust
fn get_user_middle_name_by_id(id: u64) -> Option<String> {
    get_user_by_id(id)
        .map(|user| user.middle_name)
        .flatten()
}
```

## Exercises or Practice Ideas

- Sentinel value, boolean flag, va `Option<T>` bilan bir xil domain muammoni uch xil model qilib solishtiring.
- `map`, `flatten`, va `and_then`ni bir xil data-flow ustida yozib, qaysi joyda nested `Option` paydo bo'lishini ko'ring.
- `unwrap`, `unwrap_or`, `match`, va `if let` bilan bir xil `Option<String>`ni to'rt xil usulda handle qiling.
- `Option<User>`dan `Option<&str>` yoki `Option<String>` field extraction helper yozing.

## Questions Raised

- Qachon `unwrap_or` yetarli, qachon explicit `match` o'qilishi yaxshiroq?
- Qachon `map(...).flatten()`dan ko'ra `and_then(...)` to'g'ridan-to'g'ri signal beradi?
- Optional field bilan empty stringni bir xil modelga tushirish nega yomon API signali?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-option]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[option|Option]]
- [[generic-enums|generic enums]]
- [[unwrap]]
- [[match]]
- [[if-let|if let]]
- [[closures]]
