---
title: "8. Common Collections - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, collections]
source_count: 1
source_path: "raw/books/the_rust_programming_language/8. Common Collections.md"
source_url: "https://doc.rust-lang.org/stable/book/ch08-00-common-collections.html"
---

# 8. Common Collections - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/8. Common Collections.md`
- Type: book chapter opening
- URL: https://doc.rust-lang.org/stable/book/ch08-00-common-collections.html
- Date processed: 2026-05-06

## Detailed Summary

Chapter 8 [[collections|collections]] mavzusini ochadi. Oldingi chapterlarda ko'p typelar bitta aniq value'ni ifodalagan edi; collections esa bir nechta value'ni bitta data structure ichida saqlaydi. Rust standard library bir nechta collection types beradi, lekin bu chapter uchta eng ko'p ishlatiladigan collectionga fokus qiladi: [[vector|Vec<T>]], [[string-type|String]], va [[hash-map|HashMap]].

Built-in [[array]] va [[tuple]] typelaridan farqli ravishda bu collections odatda heap-backed data'ga tayanadi. Ya'ni collection saqlayotgan data hajmi compile time'da oldindan aniq bo'lishi shart emas; program ishlayotgan paytda grow yoki shrink qilishi mumkin. Bu [[stack-and-heap|stack and heap]] mental modeliga bog'lanadi: collection value stackda metadata saqlashi mumkin, lekin elementlar yoki dynamic payload heapda turadi.

Har bir collection turining o'z capability va cost'lari bor. Chapter collection tanlashni skill sifatida frame qiladi: `Vec<T>` ordered, contiguous list uchun; `String` growable UTF-8 text uchun; `HashMap<K, V>` key-value association uchun mos. To'g'ri collection tanlash performance, API clarity, va ownership/borrowing behavioriga ta'sir qiladi.

Chapter 8 keyingi sectionsda collectionlarni yaratish va update qilishni, shuningdek har bir collectionni alohida qiladigan xususiyatlarni ko'rsatishini aytadi. 8.1 birinchi collection sifatida vectorsni ochadi; keyingi sections String va hash mapsga o'tadi.

## Key Concepts

- [[collections]]
- [[vector|Vec<T>]]
- [[string-type|String]]
- [[hash-map|HashMap]]
- [[standard-library|standard library]]
- [[stack-and-heap|stack and heap]]
- [[array]]
- [[tuple]]

## Code Examples

Bu opening section concrete Rust code bermaydi; u collection turlarining chapter mapini beradi:

```text
Vec<T>      -> variable number of same-type values
String      -> growable UTF-8 text collection
HashMap<K,V> -> key-value association
```

## Exercises or Practice Ideas

- `array`, `Vec<T>`, `String`, va `HashMap<K, V>`ni "compile-time size", "heap usage", va "use case" bo'yicha taqqoslang.
- Shopping cart, text file lines, user cache, va command history uchun qaysi collection mosligini yozing.
- Heap-backed collection o'sishi yoki qisqarishi ownership va borrowing rules bilan qanday bog'lanishini oldindan taxmin qiling.

## Questions Raised

- Collection tanlashda capability va cost'larni qanday solishtirish kerak?
- `Vec<T>` va array orasidagi eng muhim amaliy farq nima?
- `String` nega collection sifatida qaraladi?
- `HashMap<K, V>` key lookup uchun qanday mental model beradi?

## Links To Update

- [[wiki/chapters/8-common-collections|8. Common Collections]]
- [[collections]]
- [[vector|Vec<T>]]
- [[string-type|String]]
- [[hash-map|HashMap]]
- [[standard-library|standard library]]
- [[stack-and-heap|stack and heap]]
