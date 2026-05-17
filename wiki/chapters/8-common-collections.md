---
title: "8. Common Collections"
type: chapter
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, chapter, collections]
source_count: 4
---

# 8. Common Collections

## Learning Goal

Rust standard librarydagi common [[collections|collections]]ni tushunish: runtime'da grow/shrink qiladigan heap-backed data structures, [[vector|Vec<T>]] bilan ishlash, [[string-type|String]] va [[string-slice|String slice]] orasidagi farq, UTF-8 text processing, indexing/slicing tradeofflari, iteration, borrowing constraints, enum orqali bir vector ichida bir nechta value shape'ini model qilish, hamda [[hash-map|HashMap]] orqali key-value lookup, ownership, overwrite, va [[entry-api|entry API]] patternlarini qo'llash.

## Main Ideas

- Collections bir nechta value'ni bitta data structure ichida saqlaydi.
- [[array]] va [[tuple]]dan farqli ravishda common collections data'ni heapda saqlab, runtime'da grow/shrink qila oladi.
- Chapter 8 uch collectionga fokus qiladi: [[vector|Vec<T>]], [[string-type|String]], va [[hash-map|HashMap]].
- [[hash-map|HashMap]] key-value association saqlaydi; vector index orqali, hash map esa meaningful key orqali lookup qiladi.
- `HashMap` prelude'da emas, shuning uchun odatda `use std::collections::HashMap;` kerak bo'ladi.
- `HashMap<K, V>`dagi barcha keylar bir xil type, barcha valuelar bir xil type bo'ladi.
- `get` key bo'yicha `Option<&V>` qaytaradi; missing key `None`.
- Hash map iteration order arbitrary; output tartibiga tayanmaslik kerak.
- Owned `String` key/value `insert` orqali mapga move bo'ladi; `i32` kabi [[copy-trait|Copy trait]] values copied bo'ladi.
- Existing key bilan `insert` eski value'ni overwrite qiladi.
- [[entry-api|Entry API]] va `or_insert` key mavjud bo'lmasa value qo'shish, mavjud bo'lsa old value'ga mutable reference olish uchun ishlatiladi.
- Word count pattern `split_whitespace`, `entry`, `or_insert`, va `*count += 1`ni birlashtiradi.
- Default hasher SipHash bo'lib, security uchun yaxshi default; performance-sensitive code custom hasherni [[hashing-function|hashing function]] sifatida tanlashi mumkin.
- Chapter 8 yakunidagi practice ideas: vector bilan median/mode, string bilan Pig Latin, hash map bilan employee department manager.
- [[vector|Vec<T>]] elementlarni memoryda yonma-yon saqlaydigan growable list.
- `Vec<T>` faqat bitta concrete element type saqlaydi.
- Empty `Vec::new()` uchun type annotation kerak bo'lishi mumkin, chunki compiler `T`ni infer qila olmaydi.
- [[vec-macro|vec! macro]] initial valuesdan vector yaratadi va type inference uchun context beradi.
- Vectorni o'zgartirish uchun binding `mut` bo'lishi kerak; `push` vector oxiriga element qo'shadi.
- Vector elementiga `&v[index]` bilan access qilish out-of-bounds bo'lsa [[panic|panic]] qiladi.
- `v.get(index)` `Option<&T>` qaytaradi; out-of-bounds bo'lsa `None`.
- `push` vector reallocation qilishi mumkin; shu sabab elementga reference ushlab turib vectorni mutate qilish [[e0502-mutable-and-immutable-borrow-conflict|E0502]]ga olib keladi.
- Vector bo'ylab `for i in &v` immutable references bilan, `for i in &mut v` mutable references bilan yuradi.
- Mutable reference orqali element qiymatini o'zgartirish uchun [[dereference-operator|dereference operator]] `*` ishlatiladi.
- Bir vector ichida turli data shape'larini saqlash kerak bo'lsa, variants payloadli [[enums|enum]] ishlatish mumkin.
- Vector scope'dan chiqqanda vector ham, uning elementlari ham [[drop|dropped]] bo'ladi.
- Rustda "string" deganda `String` yoki `&str` nazarda tutilishi mumkin; ikkalasi ham [[utf-8|UTF-8]] encoded.
- `String` growable, mutable, owned text type; `&str` esa borrowed string slice.
- `String` `Vec<u8>` ustidagi wrapper bo'lib, valid UTF-8 guarantees va text-specific methods beradi.
- `String::new`, `String::from`, va `to_string()` string yaratishning asosiy yo'llari.
- `push_str` string slice append qiladi va ownershipni olmaydi; `push` bitta `char` qo'shadi.
- [[string-concatenation|String concatenation]]da `+` left `String`ni move qiladi va right side'ni `&str` sifatida oladi.
- [[format-macro|format! macro]] formatted `String` qaytaradi va murakkab concatenationda readableroq.
- Rust [[string-indexing|string indexing]]ni integer bilan qo'llab-quvvatlamaydi, chunki byte, `char`, va [[grapheme-clusters|grapheme cluster]] bir xil emas.
- `String::len()` bytes sonini qaytaradi, user-facing character count emas.
- String range slicing byte boundariesga tayanadi; UTF-8 character boundary buzilsa runtime [[panic|panic]] bo'ladi.
- `.chars()` Unicode scalar values bo'ylab, `.bytes()` raw bytes bo'ylab yuradi.

## Concepts

- [[collections]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[vector-indexing|vector indexing]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[hashing-function|hashing function]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[string-concatenation|string concatenation]]
- [[format-macro|format! macro]]
- [[string-indexing|string indexing]]
- [[grapheme-clusters|grapheme clusters]]
- [[standard-library|standard library]]
- [[stack-and-heap|stack and heap]]
- [[generics]]
- [[type-inference|type inference]]
- [[option|Option]]
- [[copy-trait|Copy trait]]
- [[ownership]]
- [[panic]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[dereference-operator|dereference operator]]
- [[enums]]
- [[drop]]

## Examples

Creating vectors:

```rust
let empty: Vec<i32> = Vec::new();
let values = vec![1, 2, 3];
```

Updating a vector:

```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
```

Indexing vs `get`:

```rust
let v = vec![1, 2, 3, 4, 5];

let third = &v[2];

match v.get(2) {
    Some(value) => println!("third: {value}"),
    None => println!("missing"),
}
```

Mutable iteration:

```rust
let mut v = vec![100, 32, 57];

for i in &mut v {
    *i += 50;
}
```

Enum-backed heterogeneous row:

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

String creation and update:

```rust
let mut s = String::from("foo");
s.push_str("bar");
s.push('!');
```

String concatenation:

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
```

UTF-8 iteration:

```rust
for c in "Зд".chars() {
    println!("{c}");
}

for b in "Зд".bytes() {
    println!("{b}");
}
```

Team scores hash map:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

Word count hash map:

```rust
use std::collections::HashMap;

let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
```

## Exercises

- `Vec::new()` va `vec![]` orasidagi type inference farqini tushuntiring.
- Indexing panic va `get` returning `None` farqini user input example'ida solishtiring.
- Vector elementiga immutable reference olgandan keyin `push` qilish nima uchun rad etilishini reallocation bilan tushuntiring.
- `for i in &mut v { *i += 50; }` code'ini `*`siz sinab, compiler nimadan shikoyat qilishini yozing.
- Spreadsheet row enumiga yangi variant qo'shib, `match`ni exhaustive qilib yangilang.
- `push_str` va `push` orasidagi parameter type farqini tushuntiring.
- `let s3 = s1 + &s2;`dan keyin `s1` va `s2` validity holatini yozing.
- `"Здравствуйте".len()` nima uchun 24 bo'lishini bytes/scalar values bilan tushuntiring.
- `.chars()` va `.bytes()` outputini `"Зд"` uchun solishtiring.
- `&hello[0..1]` nima uchun panic qilishini UTF-8 boundary bilan izohlang.
- `HashMap::new()` uchun nima uchun `use std::collections::HashMap;` kerakligini tushuntiring.
- `get` qaytaradigan `Option<&V>`ni `match` yoki `unwrap_or` bilan handle qiling.
- `insert` existing key bilan eski value'ni overwrite qilishini kichik example bilan ko'rsating.
- `entry(...).or_insert(...)` yordamida key missing bo'lsa default value qo'shing.
- Word count example'dagi `*count += 1` nima uchun kerakligini izohlang.
- Chapter 8 practice: integer list median/mode, string Pig Latin, employee departments manager mashqlarini rejalashtiring.

## Review Questions

- Collectionlar array va tuple'dan nimasi bilan farq qiladi?
- `Vec<T>`dagi `T` nimani bildiradi?
- Empty vector yaratishda qachon type annotation kerak?
- `&v[i]` va `v.get(i)` behaviori qanday farq qiladi?
- `push` nima uchun oldingi element reference'larini xavf ostiga qo'yishi mumkin?
- Vector bo'ylab immutable va mutable iteration syntaxlari qanday?
- Bir vector ichida har xil value shape'larini saqlash uchun enum qanday yordam beradi?
- Vector dropped bo'lganda elementlarga nima bo'ladi?
- `String` va `&str` farqi nimada?
- `String` nima uchun collection sifatida qaraladi?
- `+` operatori string concatenationda ownershipga qanday ta'sir qiladi?
- Nima uchun Rust `s[0]` string indexingga ruxsat bermaydi?
- Bytes, `char`, va grapheme cluster farqi nimada?
- String range slicing qachon panic qiladi?
- `HashMap<K, V>`dagi `K` va `V` type parameterlari nimani bildiradi?
- `HashMap` nima uchun prelude'da emas?
- `HashMap::get` missing key uchun qanday signal beradi?
- Owned `String` key/value `insert` qilinganda ownership qayerga o'tadi?
- Existing keyga `insert` qilish va `entry(...).or_insert(...)` behaviorlari qanday farq qiladi?
- Hash map word count patternida mutable reference va dereference qanday ishlaydi?
- Default SipHash qanday tradeoff beradi?

## Related Pages

- [[wiki/sources/8-common-collections]]
- [[8-1-storing-lists-of-values-with-vectors]]
- [[8-2-storing-utf-8-encoded-text-with-strings]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[collections]]
- [[vector|Vec<T>]]
- [[vec-macro|vec! macro]]
- [[vector-indexing|vector indexing]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[hashing-function|hashing function]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[string-concatenation|string concatenation]]
- [[format-macro|format! macro]]
- [[string-indexing|string indexing]]
- [[grapheme-clusters|grapheme clusters]]
- [[array]]
- [[tuple]]
- [[generics]]
- [[option|Option]]
- [[copy-trait|Copy trait]]
- [[ownership]]
- [[panic]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
- [[team-scores-hashmap|team scores hashmap]]
- [[word-count-hashmap|word count hashmap]]
- [[chapter-8-collections-practice|Chapter 8 collections practice]]
- [[drop]]
