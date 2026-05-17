---
title: "HTTP request reader"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, http, web-server]
source_count: 1
---

# HTTP request reader

Listing 21-2 `TcpStream`ni `BufReader` bilan o'rab, browser yuborgan request satrlarini bo'sh qatorgacha yig'adi.

```rust
use std::{
    io::{BufReader, prelude::*},
    net::{TcpListener, TcpStream},
};

fn handle_connection(stream: TcpStream) {
    let buf_reader = BufReader::new(&stream);
    let http_request: Vec<_> = buf_reader
        .lines()
        .map(|result| result.unwrap())
        .take_while(|line| !line.is_empty())
        .collect();

    println!("Request: {http_request:#?}");
}
```

## Why It Matters

Bu example raw TCP bytes'dan application-level HTTP request'ga o'tish nuqtasi. `take_while(|line| !line.is_empty())` request headers tugashini sodda tarzda aniqlaydi.

## Related

- [[bufreader|BufReader]]
- [[http]]
- [[tcp-stream]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
