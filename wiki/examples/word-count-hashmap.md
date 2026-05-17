---
title: "Word count hashmap"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, collections, hash-maps]
source_count: 1
---

# Word count hashmap

## Goal

[[entry-api|Entry API]] orqali textdagi word occurrencesni count qilish.

## Code

```rust
use std::collections::HashMap;

fn main() {
    let text = "hello world wonderful world";

    let mut counts = HashMap::new();

    for word in text.split_whitespace() {
        let count = counts.entry(word).or_insert(0);
        *count += 1;
    }

    println!("{counts:?}");
}
```

## Explanation

`split_whitespace` textni whitespace bo'yicha `&str` subslicesga ajratadi. `entry(word).or_insert(0)` word birinchi marta uchrasa `0` insert qiladi; har holda value'ga `&mut i32` qaytaradi.

`*count += 1` mutable reference ortidagi actual count value'ni increment qiladi. `count` loop iteration oxirida scope'dan chiqadi, shuning uchun keyingi iterationda map yana safely borrowed bo'lishi mumkin.

## Related Pages

- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[hash-map|HashMap]]
- [[entry-api|entry API]]
- [[dereference-operator|dereference operator]]
- [[borrowing]]
- [[string-slice|String slice]]
