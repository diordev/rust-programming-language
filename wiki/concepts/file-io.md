---
title: "File I/O"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, io, filesystem]
source_count: 3
---

# File I/O

## Short Definition

File I/O Rust'da `std::fs` handle'lari va `std::io` traitlari orqali file'ni o'qish-yozish modeli.

## Why It Matters

CLI, tooling, batch job, va ba'zi backend utility qatlamlarida fayl bilan ishlash muqarrar.

## Mental Model

`File` concrete handle. `[[read-trait|Read]]` va `[[write-trait|Write]]` universal byte-stream interface. `[[path|Path]]` esa file location boundary'si.

Oddiy workflow:

1. `File::open` yoki `File::create`
2. `read_to_string`, `read`, `write_all`, yoki `write`
3. xatolar `Result` orqali chiqadi
4. handle scope'dan chiqqanda `[[drop]]` cleanup qiladi

## Syntax and Examples

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_file() -> io::Result<String> {
    let mut file = File::open("poem.txt")?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}
```

```rust
use std::fs::OpenOptions;
use std::io::Write;

let mut file = OpenOptions::new().append(true).open("app.log")?;
file.write_all(b"hello\n")?;
```

## Common Mistakes

- Katta input uchun `read_to_string`ni ehtiyotsiz ishlatish.
- `write()` partial write bo'lishi mumkinligini unutish.
- Path'ni faqat UTF-8 `String` deb ko'rish.
- Error xabarida qaysi file bilan ishlanayotganini yozmaslik.

## Related Concepts

- [[std-fs|std::fs]]
- [[std-io|std::io]]
- [[read-trait|Read]]
- [[write-trait|Write]]
- [[open-options|OpenOptions]]
- [[path|Path]]
- [[io-error]]
- [[drop]]

## Sources

- [[12-2-reading-a-file]]
- [[wiki/sources/rust-for-backend-developers-io]]
- [[wiki/sources/rust-for-backend-developers-file-system]]

