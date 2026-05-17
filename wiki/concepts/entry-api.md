---
title: "Entry API"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, collections, hash-maps]
source_count: 1
---

# Entry API

## Short Definition

`entry` API [[hash-map|HashMap]]da key mavjud yoki mavjud emasligini bitta API orqali handle qilish usuli.

## Why It Matters

Ko'p hash map update patternlari "key bor bo'lsa existing value'ni ishlat, yo'q bo'lsa insert qil" shaklida bo'ladi. `entry(...).or_insert(...)` bu logicni borrowing rules bilan mos va ixcham qiladi.

## Mental Model

`map.entry(key)` key uchun slotni tekshiradi. `or_insert(default)` key mavjud bo'lsa existing value'ga mutable reference qaytaradi; mavjud bo'lmasa default value insert qiladi va unga mutable reference qaytaradi.

## Syntax and Examples

```rust
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);
```

Old value asosida update:

```rust
let count = map.entry(word).or_insert(0);
*count += 1;
```

## Common Mistakes

- Existing key value'ni overwrite qilmaslik kerak bo'lgan joyda `insert` ishlatish.
- `or_insert` mutable reference qaytarishini unutish.
- Mutable reference ortidagi value'ni update qilish uchun `*` kerakligini unutish.

## Related Concepts

- [[hash-map|HashMap]]
- [[dereference-operator|dereference operator]]
- [[borrowing]]
- [[option|Option]]
- [[word-count-hashmap|word count hashmap]]

## Sources

- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
