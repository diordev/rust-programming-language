---
title: "serde enum tagging"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, serde, enum, serialization]
source_count: 1
---

# serde enum tagging

Adjacently tagged enum JSON ichida variant tag va payloadni alohida fieldlarda saqlaydi.

```rust
use serde::{Deserialize, Serialize};

#[derive(Debug, Serialize, Deserialize)]
#[serde(tag = "type", content = "obj")]
enum Employee {
    Programmer { name: String, language: String },
    Manager { name: String },
    OfficeCat(String),
}

fn main() {
    let employee = Employee::Programmer {
        name: "John Doe".to_string(),
        language: "Rust".to_string(),
    };

    println!("{}", serde_json::to_string(&employee).unwrap());
}
```

Output shape:

```json
{"type":"Programmer","obj":{"name":"John Doe","language":"Rust"}}
```

## Related

- [[wiki/concepts/serde-enum-tagging|serde enum tagging]]
- [[wiki/crates/serde]]
- [[wiki/crates/serde-json]]
