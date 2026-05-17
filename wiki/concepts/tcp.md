---
title: "TCP"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, tcp]
source_count: 2
---

# TCP

## Short Definition

Transmission Control Protocol - ishonchli, tartibli, connection-oriented byte stream transport protokoli. Rust'da Chapter 21 uchun `TcpListener` va `TcpStream` shu qatlam bilan ishlaydi.

## Why It Matters

[[http]] odatda TCP ustida yuradi. Web server yozayotganda browser bilan almashilayotgan ma'lumotlarni yakunda aynan TCP bytes tashiydi. Demak HTTP semantics'ni tushunishdan oldin transport qatlamini minimal darajada bilish kerak.

## Mental Model

TCP "message" emas, **oqim** beradi. Dasturga "mana bitta tayyor request" demaydi; faqat kelgan bytes'larni tartibli yetkazadi. Request chegaralarini application protocol - bu yerda [[http]] - belgilaydi.

## Syntax and Examples

### Listener yaratish

```rust
use std::net::TcpListener;

let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
```

### Ochiq connection

```rust
use std::net::TcpStream;

fn handle_connection(mut stream: TcpStream) {
    // stream ichidan o'qish va yozish mumkin
}
```

## Common Mistakes

- **TCP'ni message boundary beradi deb o'ylash**: application o'zi request tugaganini aniqlashi kerak.
- **Connection va request'ni bir xil deb olish**: bir ulanish ichida bir nechta request bo'lishi mumkin.
- **HTTP'ni TCP bilan tenglashtirish**: HTTP TCP ustidagi application protocol, TCPning o'zi emas.

## Related Concepts

- [[http]]
- [[request-response]]
- [[tcp-listener]]
- [[tcp-stream]]
- [[web-server]]

## Sources

- [[wiki/sources/21-final-project-building-a-multithreaded-web-server|21]]
- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
