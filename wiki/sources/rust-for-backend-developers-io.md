---
title: "Ввод/вывод - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, io, advance]
source_count: 1
---

# Ввод/вывод - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/48. Вводвывод.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/io.html

## Detailed Summary

Bu source `std::io` ichidagi universal byte-stream modelini ochadi. Asosiy abstraktsiyalar `[[read-trait|Read]]` va `[[write-trait|Write]]`: concrete source file bo'ladimi, socket bo'ladimi, `stdin/stdout` bo'ladimi, API bir xil trait surface bilan ishlaydi.

`Read` bo'limi `read(&mut self, &mut [u8])` primitive operation ekanini ko'rsatadi. Muhim tuzatish: signature `io::Result<usize>` qaytaradi, oddiy `Result` emas. Qaytgan `usize` nechta byte o'qilganini bildiradi; 0 ko'pincha EOF signali bo'ladi.

Source `read_to_end`, `read_to_string`, `read_buf` kabi default helperlarni ham sanaydi. Durable takeaway: `read` partial read qilishi mumkin, `read_to_end` va `read_to_string` esa source'ni butunlay xotiraga yig'adi. Shuning uchun ular kichik yoki chegaralangan input uchun qulay.

`VecDeque<u8>` misoli `Read` traitni collection ustida ko'rsatadi. Bu yerda muhim semantik detail: `VecDeque` front'dan element olishni arzon qilgani uchun uni stream kabi o'qish mumkin. Source outputida oxirgi buffer eski byte'ni qoldiradi (`[9, 8]`), bu partial read'da bufferning qolgan qismi avtomatik tozalanmasligini ko'rsatadi.

`Cursor<T>` bo'limi `Vec<u8>` kabi ownershipdagi buffer'ni read position bilan stream sifatida ko'rishga imkon beradi. Source ichidagi bound matni noto'g'ri soddalashtirilgan; durable correction shuki, `Cursor<T>` bilan `Read` odatda `T: AsRef<[u8]>` kabi byte slice view'ga tayangan holda ishlaydi, `AsRef<u8>` emas.

`Write` bo'limi writing side'ni beradi. `write(&[u8])` ham `io::Result<usize>` qaytaradi va partial write qilishi mumkin; `flush()` buffered targetlar uchun buffer'ni underlying sink'ka tushiradi; `write_all()` esa barcha baytlarni yozishga urinadi. Shu sabab amaliy kodda to'liq yozish kerak bo'lsa `write_all()` ko'proq to'g'ri default.

Source `Vec<u8>` va `stdout` misollari bilan `Write`ni ko'rsatadi. Bu I/O abstractionning amaliy kuchi: testingda memory buffer bilan, productionda file yoki network stream bilan bir xil trait surface ishlatish mumkin.

## Key Concepts

- [[std-io|std::io]]
- [[read-trait|Read]]
- [[write-trait|Write]]
- [[cursor|Cursor]]
- [[file-io]]
- [[io-error]]
- [[byte-buffer]]

## Code Examples

```rust
use std::io::Read;

let mut buf = [0_u8; 2];
let read_bytes: std::io::Result<usize> = reader.read(&mut buf);
```

```rust
use std::io::Cursor;

let mut c = Cursor::new(vec![65, 66, 67]);
```

```rust
use std::io::Write;

let mut out = Vec::new();
out.write_all(b"hello")?;
```

## Exercises or Practice Ideas

- `Cursor<Vec<u8>>` ustida `read_to_string` ishlatib ko'ring.
- `write()` va `write_all()` farqini qisqa izoh bilan yozing.
- Bitta helper functionni `impl Read` qabul qiladigan qilib yozib, uni `Cursor` va `File` bilan ishlating.

## Questions Raised

- Partial read va partial write bilan ishlaydigan API'larda loop qayerda bo'lishi kerak?
- Qachon whole-buffer read qulay, qachon streaming kerak?
- Test doubles uchun `Cursor` qanchalik yetarli, qachon custom `Read`/`Write` mock kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-io]]
- [[read-trait|Read]]
- [[write-trait|Write]]
- [[cursor|Cursor]]
- [[std-io|std::io]]
- [[file-io]]

