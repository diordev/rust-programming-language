---
title: "TcpStream"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, tcp, std]
source_count: 1
---

# TcpStream

## Short Definition

`std::net::TcpStream` - ochilgan TCP connection'ni ifodalovchi type. U orqali server client'dan o'qiydi va client'ga yozadi.

## Why It Matters

Web server request'ni ham, response'ni ham aynan `TcpStream` orqali boshqaradi. `TcpListener` ulanishni qabul qiladi, `TcpStream` esa haqiqiy bytes almashinuvini olib boradi.

## Mental Model

`TcpStream` - ikki tomonlama quvur. Bir tomondan browser yuborgan request bytes keladi, ikkinchi tomondan server response bytes yozadi. Scope tugaganda stream drop bo'lib connection yopilishi mumkin.

## Syntax and Examples

### O'qish

```rust
use std::io::{BufRead, BufReader};
use std::net::TcpStream;

fn handle_connection(stream: TcpStream) {
    let buf_reader = BufReader::new(&stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();
}
```

### Yozish

```rust
use std::io::Write;

stream.write_all(response.as_bytes()).unwrap();
```

## Common Mistakes

- **UTF-8 doim bor deb o'ylash**: `lines()` text parsing uchun qulay, lekin binary yoki noto'g'ri encoding bo'lsa xato beradi.
- **Connection nega yopilib qoldi deb hayron bo'lish**: scope tugaganda stream drop bo'ladi.
- **Bitta read = bitta request deb o'ylash**: request chegaralari protocol darajasida aniqlanadi.

## Related Concepts

- [[tcp]]
- [[tcp-listener]]
- [[bufreader|BufReader]]
- [[web-server]]

## Sources

- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
