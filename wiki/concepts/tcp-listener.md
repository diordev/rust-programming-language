---
title: "TcpListener"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, tcp, std]
source_count: 1
---

# TcpListener

## Short Definition

`std::net::TcpListener` - server tomonida ma'lum address va port'ga bind bo'lib, incoming TCP ulanishlarni qabul qiladigan type.

## Why It Matters

Web serverning kirish eshigi aynan `TcpListener`. Server foydalanuvchidan request olishni boshlashi uchun avval socket ochib, shu manzilda kutishi kerak.

## Mental Model

`TcpListener` - "eshik oldida navbat kutayotgan qo'riqchi". U request'ni parse qilmaydi; faqat yangi ulanishlarni qabul qilib, har biri uchun alohida [[tcp-stream]] beradi.

## Syntax and Examples

### Bind va incoming loop

```rust
use std::net::TcpListener;

let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

for stream in listener.incoming() {
    let stream = stream.unwrap();
    handle_connection(stream);
}
```

### Nima qaytadi?

- `bind(...)` -> `Result<TcpListener, io::Error>`
- `incoming()` -> `Iterator<Item = Result<TcpStream, io::Error>>`

## Common Mistakes

- **`bind` xatosini unutish**: port band bo'lsa server ishga tushmaydi.
- **`incoming()` tayyor request qaytaradi deb o'ylash**: u connection attempts qaytaradi.
- **Port va address ma'nosini aralashtirish**: `127.0.0.1` host, `7878` port.

## Related Concepts

- [[tcp]]
- [[tcp-stream]]
- [[web-server]]
- [[request-response]]

## Sources

- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
