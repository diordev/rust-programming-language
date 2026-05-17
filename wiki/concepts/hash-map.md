---
title: "HashMap"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, collections]
source_count: 2
---

# HashMap

## Short Definition

`HashMap<K, V>` key bilan value'ni bog'laydigan standard library collection.

## Why It Matters

Hash map lookupni "index number" bilan emas, meaningful key bilan qilishga imkon beradi: masalan user ID -> user data, word -> count, setting name -> value.

## Mental Model

Hash map umumiy map data structure'ining Rust standard librarydagi implementationlaridan biri. `HashMap<K, V>` ikki type parameterga ega: `K` key type, `V` value type. Bitta map ichidagi barcha keylar bir xil concrete type, barcha valuelar ham bir xil concrete type bo'ladi.

Vector index orqali lookup qiladi; `HashMap` esa meaningful key orqali lookup qiladi. Internally key hashing qilinadi va shu hash value storage location topishga yordam beradi. Data heapda saqlanadi va map runtime'da o'sishi mumkin.

Ownership rule muhim: owned `String` key yoki value `insert` orqali mapga berilsa, ownership mapga move bo'ladi. `i32` kabi [[copy-trait|Copy trait]] typelar esa copied bo'ladi. Duplicate key bilan `insert` eski value'ni overwrite qiladi.

[[entry-api|Entry API]] key bor-yo'qligiga qarab ishlash uchun ishlatiladi. `or_insert` key mavjud bo'lmasa default value kiritadi va value'ga mutable reference qaytaradi.

## Syntax and Examples

```rust
use std::collections::HashMap;
```

Create and insert:

```rust
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

Lookup:

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

Overwrite:

```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
```

Insert only if missing:

```rust
scores.entry(String::from("Yellow")).or_insert(50);
```

Update based on old value:

```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
```

## Common Mistakes

- Ordered list kerak bo'lgan joyda hash map tanlash.
- Key-value lookup kerak bo'lgan joyda vector indexlariga ortiqcha tayanish.
- `HashMap` iteration order stable bo'ladi deb o'ylash; order arbitrary.
- `use std::collections::HashMap;` importini unutish, chunki `HashMap` prelude'da emas.
- Owned `String` key/value insert qilingandan keyin eski binding valid qoladi deb o'ylash.
- Existing key bilan `insert` eski value'ni saqlab qoladi deb kutish; u overwrite qiladi.

## Related Concepts

- [[collections]]
- [[standard-library|standard library]]
- [[use-declarations|use declarations]]
- [[entry-api|entry API]]
- [[hashing-function|hashing function]]
- [[ownership]]
- [[copy-trait|Copy trait]]
- [[option|Option]]
- [[vector|Vec<T>]]
- [[string-type|String]]
- [[team-scores-hashmap|team scores hashmap]]
- [[word-count-hashmap|word count hashmap]]

## Sources

- [[wiki/sources/8-common-collections]]
- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
