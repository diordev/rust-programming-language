---
title: "hello.html response"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, http, web-server]
source_count: 1
---

# hello.html response

Listings 21-3 va 21-5 server response yozishning ikki bosqichini ko'rsatadi: avval minimal blank response, keyin `hello.html` body bilan haqiqiy HTML response.

## 1. Minimal successful response

```rust
let response = "HTTP/1.1 200 OK\r\n\r\n";
stream.write_all(response.as_bytes()).unwrap();
```

## 2. HTML body bilan response

```rust
use std::fs;

let status_line = "HTTP/1.1 200 OK";
let contents = fs::read_to_string("hello.html").unwrap();
let length = contents.len();

let response =
    format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");

stream.write_all(response.as_bytes()).unwrap();
```

## Why It Matters

Birinchi fragment browser error'ni to'xtatadi, ikkinchisi esa browser ko'rsatishi mumkin bo'lgan to'liq HTML body qaytaradi. `Content-Length` shu bosqichda muhim bo'lib qoladi.

## Related

- [[http]]
- [[content-length-header|Content-Length header]]
- [[fs-read-to-string|fs::read_to_string]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
