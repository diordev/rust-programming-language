---
title: "21.1. Building a Single-Threaded Web Server"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, web-server, tcp, http]
source_count: 1
---

# 21.1. Building a Single-Threaded Web Server

## Source

- File: `raw/books/the_rust_programming_language/21.1. Building a Single-Threaded Web Server.md`
- URL: <https://doc.rust-lang.org/stable/book/ch21-01-single-threaded.html>
- Book: *The Rust Programming Language*

## Detailed Summary

21.1 bobi birinchi ishlaydigan, lekin hali single-threaded bo'lgan Rust web serverini bosqichma-bosqich quradi. Maqsad framework ishlatish emas, balki browser yuborayotgan bytes va server qaytarayotgan bytes orasidagi aloqani ochiq ko'rish.

### TCP va HTTP rollari

Bo'lim avval ikki protokolni ajratadi:

- [[tcp]] - past darajadagi transport; bytes'larni ishonchli va tartibli yetkazadi.
- [[http]] - shu bytes ichida request va response tarkibini belgilaydigan application protocol.

Ikkalasi ham bu kontekstda [[request-response]] modelida ishlaydi: client so'rov yuboradi, server tinglaydi va javob qaytaradi.

### `TcpListener` bilan ulanishlarni tinglash

Server `TcpListener::bind("127.0.0.1:7878")` bilan lokal manzil va portga ulanadi. `incoming()` iteratori orqali kiruvchi ulanish urinishlari olinadi. Har item `TcpStream` bo'lib, ochilgan connection'ni ifodalaydi.

Muhim nuance: loop request'lar emas, connection attempts ustidan yuradi. Browser bir sahifa uchun bir nechta connection ochishi yoki `favicon.ico` kabi qo'shimcha resurs so'rashi mumkin.

### Request'ni o'qish

Keyingi qadamda `handle_connection` funksiyasi ajratiladi. `BufReader::new(&stream)` `TcpStream` ustiga buffered layer qo'yadi va `lines()` orqali line-based o'qish imkonini beradi.

Book avval butun request'ni quyidagi pipeline bilan yig'adi:

- `lines()` - newline bo'yicha bo'ladi;
- `map(|result| result.unwrap())` - `Result<String, io::Error>` ichidan `String`ni oladi;
- `take_while(|line| !line.is_empty())` - HTTP request'ning bo'sh satr bilan tugashidan foydalanadi;
- `collect::<Vec<_>>()` - debugging uchun hamma satrlarni yig'adi.

Shu yerda request line formati tushuntiriladi:

```text
GET / HTTP/1.1
```

Bu satr method, URI va version'dan iborat. Qolgan satrlar headers; `GET` request'da body bo'lmasligi mumkin.

### Minimal HTTP response

Serverning eng sodda javobi:

```text
HTTP/1.1 200 OK\r\n\r\n
```

Bu yerda status line va undan keyin bo'sh qator bor. `stream.write_all(response.as_bytes())` shu bytes'larni browserga uzatadi. Natijada browser error o'rniga blank page ko'ra boshlaydi.

### HTML file qaytarish

Server keyin `hello.html` faylini `fs::read_to_string` bilan o'qib, response body sifatida yuboradi. Bu bosqichda [[content-length-header|Content-Length]] header muhim bo'ladi:

```rust
let response =
    format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
```

Browser body uzunligini bilishi uchun `length` body byte length bo'lishi kerak.

### Request'ni tekshirish va 404 qaytarish

Avvalgi versiya hamma request'ga bir xil HTML qaytarardi. Keyin kod faqat request line `GET / HTTP/1.1` bo'lsa `hello.html`, aks holda `404.html` yuboradi. Bu juda sodda routing bo'lsa ham, real serverning asosiy g'oyasini beradi: request ma'lumotiga qarab response tanlash.

### Refactoring

`if/else` ichidagi takrorlanuvchi kod tuple destructuring bilan qisqartiriladi:

```rust
let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
    ("HTTP/1.1 200 OK", "hello.html")
} else {
    ("HTTP/1.1 404 NOT FOUND", "404.html")
};
```

Shundan keyin file o'qish va response yozish bitta umumiy yo'l orqali bajariladi.

### Asosiy cheklov

Bu server hali [[threads]] ishlatmaydi. Shuning uchun u bir vaqtda bitta requestni ishlaydi. Keyingi bo'limlar aynan shu bottleneck'ni hal qilishga o'tadi.

## Key Concepts

- [[web-server]]
- [[tcp]]
- [[http]]
- [[request-response]]
- [[tcp-listener]]
- [[tcp-stream]]
- [[bufreader|BufReader]]
- [[content-length-header|Content-Length header]]
- [[fs-read-to-string|fs::read_to_string]]
- [[format-macro|format! macro]]

## Code Examples

- [[tcp-listener-connection-loop|Listing 21-1: TcpListener connection loop]]
- [[http-request-reader|Listing 21-2: HTTP request reader]]
- [[hello-html-response|Listings 21-3 and 21-5: minimal and HTML response]]
- [[request-line-404-routing|Listings 21-6..21-9: request-line based routing and refactor]]

## Exercises or Practice Ideas

1. `127.0.0.1:7878/foo` va `127.0.0.1:7878/` request'lari uchun `request_line` qanday farq qilishini tekshiring.
2. `hello.html` body uzunligini noto'g'ri yuborsangiz browser qanday xulq ko'rsatishini kuzating.
3. `GET /sleep HTTP/1.1` kabi alohida path qo'shib, keyingi multi-threaded optimizatsiya nega kerak bo'lishini oldindan sinab ko'ring.
4. `BufReader::lines()` pipeline'dagi har bir adapter nima qilayotganini yozib chiqing.

## Questions Raised

- Nima uchun browser bitta page load uchun bir necha marta ulanadi?
- Nega `incoming()` connection attempts qaytaradi, tayyor request emas?
- Nima uchun `Content-Length` bo'lmasa browser response body'ni noto'g'ri talqin qilishi mumkin?
- Single-threaded server qaysi holatda real foydalanuvchilar uchun yomon bottleneck bo'ladi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[wiki/chapters/21-1-building-a-single-threaded-web-server|Chapter 21.1]]
- [[web-server]]
- [[http]]
- [[tcp]]
