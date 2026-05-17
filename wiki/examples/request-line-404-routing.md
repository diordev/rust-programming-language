---
title: "Request-line based 200/404 routing"
type: example
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, http, routing]
source_count: 1
---

# Request-line based 200/404 routing

Listings 21-6 dan 21-9 gacha server faqat `GET / HTTP/1.1` uchun `hello.html`, qolgan request'lar uchun `404.html` qaytaradigan eng sodda routing'ni quradi.

```rust
let buf_reader = BufReader::new(&stream);
let request_line = buf_reader.lines().next().unwrap().unwrap();

let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
    ("HTTP/1.1 200 OK", "hello.html")
} else {
    ("HTTP/1.1 404 NOT FOUND", "404.html")
};

let contents = fs::read_to_string(filename).unwrap();
let length = contents.len();

let response =
    format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");

stream.write_all(response.as_bytes()).unwrap();
```

## Why It Matters

Bu routing hali real router emas, lekin request ma'lumotiga qarab response tanlash g'oyasini aniq ko'rsatadi. Shu bilan birga tuple destructuring takroriy code'ni kamaytiradi.

## Related

- [[http]]
- [[request-response]]
- [[pattern-destructuring|pattern destructuring]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
