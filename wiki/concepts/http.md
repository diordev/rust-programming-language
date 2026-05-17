---
title: "HTTP"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, http]
source_count: 2
---

# HTTP

## Short Definition

Hypertext Transfer Protocol - client va server o'rtasidagi request/response tarkibini belgilaydigan application protocol. Chapter 21 misolida u [[tcp]] ustida text-based request va response satrlaridan iborat.

## Why It Matters

Browser server bilan "just bytes" emas, ma'noli request yuboradi: method, path, headers, va ba'zan body. Server ham status code, headers, va body bilan javob beradi. Shu qoidalarni bilmasdan web server to'g'ri ishlamaydi.

## Mental Model

TCP quvur bo'lsa, HTTP shu quvur ichidan o'tadigan hujjat formati. Server HTTP'ni parse qilib, qaysi resource so'ralganini aniqlaydi va protocolga mos response yasaydi.

## Syntax and Examples

### Request line

```text
GET / HTTP/1.1
```

### Minimal response

```text
HTTP/1.1 200 OK\r\n\r\n
```

### Body bilan response

```rust
let response =
    format!("HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}");
```

## Common Mistakes

- **CRLF separator'larni unutish**: HTTP satrlari `\r\n` bilan ajraladi.
- **Header va body orasidagi bo'sh qatorni unutish**: browser response'ni noto'g'ri parse qilishi mumkin.
- **Body uzunligini ko'rsatmaslik**: ayniqsa body bor response'larda `Content-Length` muhim.

## Related Concepts

- [[tcp]]
- [[request-response]]
- [[content-length-header|Content-Length header]]
- [[web-server]]
- [[tcp-stream]]

## Sources

- [[wiki/sources/21-final-project-building-a-multithreaded-web-server|21]]
- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
