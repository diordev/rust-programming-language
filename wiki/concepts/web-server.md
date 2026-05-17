---
title: "Web Server"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, web-server]
source_count: 3
---

# Web Server

## Short Definition

Client'lardan kelgan tarmoq request'larini qabul qilib, ularga response qaytaradigan dastur. Chapter 21 kontekstida bu server raw [[tcp]] connection ustida [[http]] request/response bytes bilan ishlaydi.

## Why It Matters

Web server Rust'dagi bir nechta yirik mavzularni bitta joyga bog'laydi: I/O, ownership, strings, parsing, branching, file serving, va concurrency. Bu systems-level fikrlashni ham, application-level protocol fikrlashni ham talab qiladi.

## Mental Model

Browser serverga ulanadi, request yuboradi, server uni talqin qiladi va response yozadi. Serverning vazifasi faqat "HTML ko'rsatish" emas; u protocol qoidalariga mos bytes yuborishi kerak.

Single-threaded server modeli:

1. Tingla
2. Ulanishni qabul qil
3. Request'ni o'qib tushun
4. Response yoz
5. Keyingi request'ga o't

Multithreaded variant esa tinglash bilan request processing orasiga [[thread-pool]] qo'yadi:

1. Tingla
2. Request'ni qabul qil
3. Job sifatida pool'ga yubor
4. Worker thread bajarsin
5. Navbatdagi requestni olishda davom et

## Syntax and Examples

### Minimal server skeleton

```rust
use std::net::TcpListener;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        handle_connection(stream);
    }
}
```

### HTML response yuborish

```rust
let contents = std::fs::read_to_string("hello.html").unwrap();
let length = contents.len();
let response =
    format!("HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}");
```

## Common Mistakes

- **TCP va HTTP'ni aralashtirish**: TCP faqat bytes tashiydi; HTTP esa shu bytes ma'nosini belgilaydi.
- **Response formatini soddalashtirib yuborish**: body yuborib, `Content-Length` yoki to'g'ri CRLF'larni unutish browser xulqini buzadi.
- **Connection va request'ni bir deb o'ylash**: bir browser session bir nechta connection yoki bir connection ichida bir nechta request patternlarini yaratishi mumkin.
- **Concurrency'ni faqat `thread::spawn` deb tasavvur qilish**: amaliy serverlar fixed-size pool yoki async runtime bilan resursni boshqaradi.

## Related Concepts

- [[tcp]]
- [[http]]
- [[request-response]]
- [[tcp-listener]]
- [[tcp-stream]]
- [[content-length-header|Content-Length header]]
- [[threads]]
- [[thread-pool]]

## Sources

- [[wiki/sources/21-final-project-building-a-multithreaded-web-server|21]]
- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
