---
title: "21.1. Building a Single-Threaded Web Server"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, web-server, tcp, http]
source_count: 1
---

# 21.1. Building a Single-Threaded Web Server

## Learning Goal

`std::net` yordamida minimal ishlaydigan HTTP server yozish: connection'ni tinglash, request'ni o'qish, `200 OK` yoki `404 NOT FOUND` response qaytarish, va bularning hammasi single-threaded bo'lganda qayerda limit paydo bo'lishini ko'rish.

## Main Ideas

- Browser bilan gaplashish uchun serverga ikki qatlam kerak: [[tcp]] bytes tashiydi, [[http]] esa shu bytes ma'nosini belgilaydi.
- `TcpListener` server tomonidagi tinglovchi socket; `TcpStream` esa alohida ochilgan connection.
- HTTP request'ning eng muhim qismi avval request line: `GET / HTTP/1.1`.
- Response kamida status line va CRLF bilan tugashi kerak; body bo'lsa [[content-length-header|Content-Length]] ham kerak.
- File serving va request-based branching minimal web serverning yadrosini beradi.
- Kod ishlaydi, lekin hali bir vaqtda bitta requestni ko'radi.

## TCP va HTTP qisqa farqi

| Tushuncha | Vazifa |
|-----------|--------|
| [[tcp]] | bytes'larni ishonchli yetkazish |
| [[http]] | request/response formatini belgilash |
| [[request-response]] | client yuboradi, server javob beradi |

## Bosqichlar

### 1. Connection loop

```rust
let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

for stream in listener.incoming() {
    let stream = stream.unwrap();
    handle_connection(stream);
}
```

Batafsil: [[tcp-listener-connection-loop]].

### 2. Request'ni o'qish

```rust
let buf_reader = BufReader::new(&stream);
let request_line = buf_reader.lines().next().unwrap().unwrap();
```

Bu yerda `BufReader` va `lines()` browser yuborgan text request bilan ishlashni soddalashtiradi. Batafsil: [[http-request-reader]].

### 3. Minimal response

```rust
let response = "HTTP/1.1 200 OK\r\n\r\n";
stream.write_all(response.as_bytes()).unwrap();
```

Bu blank page uchun yetadi, lekin HTML body hali yo'q.

### 4. HTML body va `Content-Length`

```rust
let contents = fs::read_to_string("hello.html").unwrap();
let length = contents.len();
let response =
    format!("HTTP/1.1 200 OK\r\nContent-Length: {length}\r\n\r\n{contents}");
```

Batafsil: [[hello-html-response]].

### 5. Routing va refactor

`GET / HTTP/1.1` bo'lsa `hello.html`, aks holda `404.html` qaytariladi. Keyin `(status_line, filename)` tuple'i bilan duplicated code qisqartiriladi. Batafsil: [[request-line-404-routing]].

## Concepts

- [[web-server]]
- [[tcp]]
- [[http]]
- [[request-response]]
- [[tcp-listener]]
- [[tcp-stream]]
- [[bufreader|BufReader]]
- [[content-length-header|Content-Length header]]
- [[fs-read-to-string|fs::read_to_string]]
- [[pattern-destructuring|pattern destructuring]]

## Examples

- [[tcp-listener-connection-loop|Listing 21-1]]
- [[http-request-reader|Listing 21-2]]
- [[hello-html-response|Listings 21-3 and 21-5]]
- [[request-line-404-routing|Listings 21-6..21-9]]

## Exercises

1. `GET /foo HTTP/1.1` request'iga 404 response qaytishini terminal va browserdan tekshiring.
2. `Content-Length`ni olib tashlab response yuboring va browser xulqini kuzating.
3. `if`/`else`dan oldingi va keyingi versiyani taqqoslab, refactor nimani yutganini yozing.
4. `request_line` o'rniga butun request'ni yig'ish va faqat birinchi satrni o'qish orasidagi farqni tushuntiring.
5. Nima uchun bu server single-threaded bo'lgani uchun keyingi chapter improvement'ga muhtojligini misol bilan ko'rsating.

## Review Questions

1. `TcpListener` va `TcpStream` qanday farq qiladi?
2. Nima uchun `incoming()` connection attempts ustidan iteratsiya qiladi deb aytiladi?
3. HTTP request line qaysi uch bo'lakdan iborat?
4. Nima uchun `\r\n\r\n` HTTP response uchun muhim?
5. `Content-Length` nimani bildiradi?
6. Nega `GET / HTTP/1.1` bilan oddiy string comparison bu misolda yetarli?
7. Bu serverning single-threaded bo'lishi qaysi real muammoni keltirib chiqaradi?

## Related Pages

- [[wiki/sources/21-1-building-a-single-threaded-web-server|Source: 21.1]]
- [[wiki/chapters/21-final-project-building-a-multithreaded-web-server|Chapter 21]]
- [[wiki/chapters/21-2-from-single-threaded-to-multithreaded-server|Chapter 21.2]]
- [[12-an-io-project|Chapter 12: I/O Project]]
- [[threads]]
