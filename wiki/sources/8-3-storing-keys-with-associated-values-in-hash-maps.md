---
title: "8.3. Storing Keys with Associated Values in Hash Maps - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, collections, hash-maps]
source_count: 1
source_path: "raw/books/the_rust_programming_language/8.3. Storing Keys with Associated Values in Hash Maps.md"
source_url: "https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html"
---

# 8.3. Storing Keys with Associated Values in Hash Maps - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/8.3. Storing Keys with Associated Values in Hash Maps.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch08-03-hash-maps.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section Chapter 8dagi oxirgi common collection — [[hash-map|HashMap]]ni tushuntiradi. `HashMap<K, V>` `K` typedagi keysni `V` typedagi values bilan bog'laydi. U keys va valuesni memoryga joylashtirish uchun [[hashing-function|hashing function]]dan foydalanadi. Boshqa tillarda shunga o'xshash data structure hash, map, object, hash table, dictionary, yoki associative array deb atalishi mumkin.

Hash map index number bilan emas, meaningful key bilan lookup qilish kerak bo'lganda foydali. Masalan, game score table'da team name key, score esa value bo'lishi mumkin. Team nomini bilsangiz, score'ni topasiz.

Hash map yaratish uchun standard librarydan `HashMap` scope'ga kiritiladi:

```rust
use std::collections::HashMap;
```

Bu zarur, chunki uch common collection ichida `HashMap` prelude'ga kirmaydi. Standard libraryda `HashMap` yaratish uchun built-in macro ham yo'q. Empty map `HashMap::new()` bilan yaratiladi va key-value pairs `insert` bilan qo'shiladi:

```rust
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

[[vector|Vec<T>]] kabi hash maps ham heapda data saqlaydi va homogeneous: hamma keys bir xil type, hamma values bir xil type bo'lishi kerak. Bu example'da keys `String`, values `i32`.

Value olish uchun `get` methodiga key reference beriladi. `get` `Option<&V>` qaytaradi: key bo'lsa `Some(&value)`, bo'lmasa `None`. Example:

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

Bu yerda `copied()` `Option<&i32>`ni `Option<i32>`ga aylantiradi, chunki `i32` [[copy-trait|Copy]] type. `unwrap_or(0)` key topilmasa default score sifatida 0 beradi. Hash map bo'ylab `for (key, value) in &scores` bilan iterate qilish mumkin, lekin order arbitrary. Hash map iteration orderiga tayanmaslik kerak.

Ownership qoidalari hash mapda juda muhim. `i32` kabi `Copy` values map ichiga copied bo'ladi. `String` kabi owned values esa `insert` qilinganda moved bo'ladi va map ularning owner'iga aylanadi:

```rust
let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);
```

Shundan keyin `field_name` va `field_value` invalid. Ularni ishlatishga urinish [[e0382-borrow-of-moved-value|E0382]]ga olib keladi. Agar references mapga insert qilinsa, original values move bo'lmaydi, lekin references ko'rsatayotgan values hash mapdan kam yashamasligi kerak; bu [[lifetimes]] mavzusiga bog'lanadi.

Hash map update qilishda unique key qoidasini tushunish kerak: bitta key bir vaqtda faqat bitta value bilan associated bo'ladi. Lekin bir xil value bir nechta keyda bo'lishi mumkin. Existing keyga `insert` yana chaqirilsa, old value overwrite qilinadi:

```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
```

Natija `{"Blue": 25}`.

Ko'p hollarda "agar key yo'q bo'lsa qo'sh, bor bo'lsa old value'ni saqla" kerak bo'ladi. Buning uchun [[entry-api|entry API]] ishlatiladi. `entry(key)` `Entry` enum qaytaradi; `or_insert(value)` key mavjud bo'lsa existing value'ga mutable reference, mavjud bo'lmasa value insert qilib yangi value'ga mutable reference qaytaradi:

```rust
scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);
```

Bu borrowing rules bilan yaxshi ishlaydi va manual contains/insert logicdan cleaner.

Hash mapning yana keng patterni: old value asosida update qilish. Word count example'da textdagi har bir word count qilinadi:

```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
```

`split_whitespace` whitespace bilan ajratilgan subslices iteratorini beradi. `or_insert(0)` `&mut V` qaytaradi, bu yerda `&mut i32`. `*count += 1` ishlashi uchun [[dereference-operator|dereference operator]] kerak: mutable reference ortidagi actual count value increment qilinadi. Mutable reference loop iteration oxirida scope'dan chiqadi, shuning uchun keyingi iterationlar borrowing rulesga mos.

By default `HashMap` SipHash deb ataladigan hashing functiondan foydalanadi. SipHash hash table'ga qarshi denial-of-service attacklarga resistance berishi mumkin. U eng tez hashing algorithm emas, lekin security/performance tradeoffi standard library defaulti uchun ma'qul. Agar profiling default hasher sekinligini ko'rsatsa, boshqa hasher tanlash mumkin; hasher `BuildHasher` trait implement qiladigan type. Common hashers uchun crates.io'da crates bor.

Chapter summary uchta collectionni mustahkamlaydi: vectors, strings, va hash maps store/access/modify data uchun katta functionality beradi. Practice ideas: integers list uchun median va mode topish (`Vec` + `HashMap`), stringsni Pig Latin'ga o'tkazish (UTF-8ni hisobga olib), va employee/department text interface yaratish (`HashMap` + vectors).

## Key Concepts

- [[hash-map|HashMap]]
- [[collections]]
- [[hashing-function|hashing function]]
- [[entry-api|entry API]]
- [[ownership]]
- [[copy-trait|Copy trait]]
- [[move-semantics|move semantics]]
- [[borrowing]]
- [[option|Option]]
- [[dereference-operator|dereference operator]]
- [[lifetimes]]
- [[crates-io|crates.io]]
- [[chapter-8-collections-practice|Chapter 8 collections practice]]

## Code Examples

Creating and inserting:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

Accessing:

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

Iterating:

```rust
for (key, value) in &scores {
    println!("{key}: {value}");
}
```

Move into map:

```rust
let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);
```

Overwrite:

```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
```

Insert only if missing:

```rust
scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);
```

Word count:

```rust
let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
```

## Exercises or Practice Ideas

- Team scores hash mapini yarating, `get` bilan existing va missing team score'larini oling.
- `insert` duplicate keyni overwrite qilishini kichik example bilan ko'rsating.
- `entry(...).or_insert(...)` bilan "agar yo'q bo'lsa qo'sh" patternini yozing.
- Word count example'ida `*count += 1` nima uchun `*` talab qilishini tushuntiring.
- `String` key/value insert qilingandan keyin eski variablesni ishlatib [[e0382-borrow-of-moved-value|E0382]]ni kuzating.
- Chapter summarydagi median/mode, Pig Latin, va employee departments mashqlarini alohida practice project sifatida bajaring.

## Questions Raised

- Hash map iteration orderiga qachon tayanish xavfli?
- `get` nima uchun `Option<&V>` qaytaradi?
- `HashMap`ga owned values insert qilish ownershipni qanday o'zgartiradi?
- `entry` API manual `contains_key` + `insert` logicdan nimasi bilan yaxshi?
- Default SipHash security/performance tradeoffi qachon muhim bo'ladi?
- Custom hasher qachon kerak bo'lishi mumkin?

## Links To Update

- [[wiki/chapters/8-common-collections|8. Common Collections]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[hashing-function|hashing function]]
- [[word-count-hashmap|word count hashmap]]
- [[team-scores-hashmap|team scores hashmap]]
- [[chapter-8-collections-practice|Chapter 8 collections practice]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
