---
title: "Team scores hashmap"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, collections, hash-maps]
source_count: 1
---

# Team scores hashmap

## Goal

[[hash-map|HashMap]] bilan team name -> score mapping yaratish, lookup qilish, va key-value pairs bo'ylab yurish.

## Code

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);

    println!("{team_name}: {score}");

    for (team, score) in &scores {
        println!("{team}: {score}");
    }
}
```

## Explanation

`HashMap` prelude'da emas, shuning uchun `use std::collections::HashMap;` kerak. `get` `Option<&i32>` qaytaradi; `copied()` uni `Option<i32>`ga aylantiradi, `unwrap_or(0)` missing key uchun default beradi.

Iteration order arbitrary, shuning uchun output orderiga tayanmaslik kerak.

## Related Pages

- [[8-3-storing-keys-with-associated-values-in-hash-maps]]
- [[hash-map|HashMap]]
- [[option|Option]]
- [[copy-trait|Copy trait]]
- [[use-declarations|use declarations]]
