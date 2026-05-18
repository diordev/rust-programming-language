---
title: "LazyLock RwLock session store"
type: example
status: active
created: 2026-05-18
updated: 2026-05-18
tags: [rust, lazylock, rwlock, arc, mutex, sessions]
source_count: 1
---

# LazyLock RwLock session store

Global session registry `RwLock<HashMap<...>>` bilan, individual session esa `Arc<Mutex<UserSession>>` bilan olinadi.

```rust
use std::{
    collections::HashMap,
    sync::{Arc, LazyLock, Mutex, RwLock},
};

struct UserSession {
    some_field: String,
}

static SESSIONS: LazyLock<RwLock<HashMap<String, Arc<Mutex<UserSession>>>>> =
    LazyLock::new(|| RwLock::new(HashMap::new()));
```

Muhim qoida: registry `RwLock` faqat session pointer'ni olish paytigacha ushlanadi; keyin darhol bo'shatiladi.

## Related

- [[global-state]]
- [[lazylock|LazyLock]]
- [[rwlock|RwLock]]
- [[arc-t|Arc<T>]]
- [[mutex-t|Mutex<T>]]
