---
title: "File I/O"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, io, filesystem]
source_count: 1
---

# File I/O

## Short Definition

Rust standart kutubxonasi `std::fs` moduli orqali fayl o'qish va yozish imkonini beradi. Barcha fayl operatsiyalari `Result` qaytaradi — xatolar majburiy boshqariladi.

## Why It Matters

CLI dasturlari ko'pincha fayl o'qiydi yoki yozadi. `std::fs` ergonomik API taqdim etadi; `BufReader` kabi asboblar katta fayllar uchun samaradorlikni oshiradi.

## Mental Model

Fayl operatsiyasi doim muvaffaqiyatsiz bo'lishi mumkin (fayl yo'q, ruxsat yo'q, disk to'la). Shuning uchun Rust `Result` qaytaradi va xatolarni e'tiborsiz qoldirib bo'lmaydi.

## Syntax and Examples

To'liq fayl o'qish:

```rust
use std::fs;

let contents = fs::read_to_string("poem.txt")
    .expect("Should have been able to read the file");
```

Xatoni `?` bilan uzatish:

```rust
use std::fs;
use std::error::Error;

fn read_file(path: &str) -> Result<String, Box<dyn Error>> {
    let contents = fs::read_to_string(path)?;
    Ok(contents)
}
```

## Common Mistakes

- Katta fayllar uchun `read_to_string` — butun faylni xotiraga yuklaydi. Katta fayllar uchun `BufReader` samarali.
- Xato xabarida fayl yo'lini ko'rsatmaslik — debugging qiyinlashadi.

## Related Concepts

- [[fs-read-to-string|fs::read_to_string]]
- [[result|Result]]
- [[question-mark-operator|? operatori]]
- [[error-handling]]

## Sources

- [[12-2-reading-a-file]]
