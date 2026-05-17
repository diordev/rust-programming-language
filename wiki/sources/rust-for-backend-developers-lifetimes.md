---
title: "Лайфтаймы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, lifetimes]
source_count: 1
---

# Лайфтаймы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/22. Лайфтаймы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/lifetimes.html

## Detailed Summary

Bu source lifetime'larni ownership va borrowingning keyingi qatlami sifatida beradi: object bitta ownerga ega bo'lishi mumkin, lekin references owner umridan uzun yashay olmaydi.

Markaziy misol `take_longest(x: &str, y: &str) -> &str`. Lifetime annotation yo'qligida compiler `E0106 missing lifetime specifier` beradi, chunki return qaysi input borrow'iga bog'langanini bilmaydi. Source bu muammoni real foydalanish scenariysi bilan ko'rsatadi: ichki scope'dagi `s2`ga qaragan natija outer scope'dagi `longest`ga yozilsa, dangling reference xavfi paydo bo'ladi.

Solution sifatida `fn take_longest<'a>(x: &'a str, y: &'a str) -> &'a str` beriladi. Muhim to'g'ri takeaway: annotation referencening umrini cho'zmaydi; u faqat relationshipni compilerga ko'rsatadi. Source matnida "bir scope'ga tegishli" degan soddalashtirish bor, lekin wiki buni relation modeli sifatida ehtiyotkorroq yozadi.

`'static` bo'limi ixcham: string literalga reference `'static` lifetimega ega, chunki literal program tugaguncha mavjud bo'ladi. Bu beginner stage uchun yetarli. Oxirida source practical note beradi: early stage'da murakkab lifetime bog'lanishlar bilan kurashish o'rniga ba'zan `.clone()` bilan owned copy olish osonroq bo'lishi mumkin.

## Key Concepts

- [[lifetimes]]
- [[static-lifetime]]
- [[dangling-reference|dangling reference]]
- [[borrow-checker]]
- [[string-slice|String slice]]
- [[ownership]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]

## Code Examples

```rust
fn take_longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

```rust
let s1 = String::from("aaa");
let longest;
{
    let s2 = String::from("bbbb");
    longest = take_longest(s1.as_str(), s2.as_str());
}
```

```rust
const TEXT: &'static str = "some string";
```

## Exercises or Practice Ideas

- `take_longest` misolini annotation bilan va बिना annotation compile natijasi bilan solishtiring.
- Return reference qaysi inputga bog'langanini qo'lda izohlaydigan 2 ta function signature yozing.
- Lifetime muammosini `clone()` bilan chetlab o'tish va proper annotation bilan hal qilishni solishtiring.

## Questions Raised

- Lifetime relationni grafik yoki interval mental model bilan tushuntirish yaxshiroqmi?
- Qachon `&str` return qilishdan ko'ra owned `String` qaytarish dizaynni soddalashtiradi?
- Compiler `'static` tavsiya qilganda bu haqiqiy yechim yoki symptom ekanini qanday ajratish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-lifetimes]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[lifetimes]]
- [[static-lifetime]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]
