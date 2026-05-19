---
title: "serde JSON Employee"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, serde, json, serialization]
source_count: 1
---

# serde JSON Employee

Typed structni JSON stringga serialize qilish va JSON stringdan qayta deserialize qilish.

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Serialize, Deserialize)]
struct Employee {
    id: u64,
    name: String,
}

fn main() {
    let emp1 = Employee {
        id: 1,
        name: "John Doe".to_string(),
    };

    let json = serde_json::to_string(&emp1).unwrap();
    println!("{json}");

    let emp2: Employee = serde_json::from_str(
        r#"{"id":2,"name":"Ivan Ivanov"}"#,
    )
    .unwrap();
    println!("{emp2:?}");
}
```

## Related

- [[wiki/crates/serde]]
- [[wiki/crates/serde-json]]
- [[serialization]]
- [[deserialization]]

