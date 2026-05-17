---
title: "Файловая система - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, filesystem, advance]
source_count: 1
---

# Файловая система - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/49. Файловая система.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/file-system.html

## Detailed Summary

Bu source `std::fs` va `std::io` kesishgan joyini, ya'ni filesystem boundary'ni ko'rsatadi. `File`, `OpenOptions`, `read_dir`, `io::Result`, `OsString`, `OsStr`, `Path` birgalikda ishlaydi.

Birinchi example file lifecycle'ni ko'rsatadi: `File::create`, `write_all`, `flush`, scope exit, keyin append mode bilan `OpenOptions`, keyin `File::open` va `read_to_string`. Muhim semantic takeaway: file handle'ni ko'pincha qo'lda `close()` qilinmaydi; `[[drop|Drop]]` scope tugaganda resource cleanup qiladi. Lekin explicit `flush()` yozuvni aynan shu nuqtada underlying target'ga tushirish kerak bo'lganda baribir foydali.

`io::Result<T>` alohida qayd qilinadi. Bu yangi error algebra emas, balki `Result<T, std::io::Error>` alias'i. Demak filesystem API "special exception" emas, Rustning umumiy `Result` modeli bilan ishlaydi.

`read_dir` example directory entry, `file_name`, `file_type`ni ko'rsatadi. Bu yerda muhim boundary string modeliga o'tadi: filesystem nomlari Rust `String` emas, ko'pincha `[[osstring|OsString]]` va `[[osstr|OsStr]]` bilan ifodalanadi. Sabab platforma-level encoding Unix va Windows'da farq qiladi.

`Path` bo'limi shu string boundary'ni mustahkamlaydi. `Path` text emas, filesystem path semantics uchun wrapper. Durable mental model: `Path` amalda `OsStr` ustidagi path-aware view, shuning uchun file API'lari ko'pincha `P: AsRef<Path>` qabul qiladi.

`AsRef<Path>` ergonomics'ining foydasi aniq: caller `&str`, `String`, `PathBuf`, yoki `&Path` berishi mumkin; API esa bittagina generic imzo bilan qoladi. Bu Rust filesystem API design'ining standart patterni.

Bu source backend ishlarida filesystem markaziy mavzu emasligini aytadi, lekin amaliy chegarani aniq ochadi: byte I/O bir tarafda, path/os-string platform boundary boshqa tarafda.

## Key Concepts

- [[std-fs|std::fs]]
- [[file-io]]
- [[io-error]]
- [[open-options|OpenOptions]]
- [[osstring|OsString]]
- [[osstr|OsStr]]
- [[path|Path]]
- [[drop]]

## Code Examples

```rust
use std::fs::{File, OpenOptions};
use std::io::{self, Read, Write};

fn main() -> io::Result<()> {
    let mut file = File::create("file.txt")?;
    file.write_all(b"hello")?;
    Ok(())
}
```

```rust
for entry in std::fs::read_dir(".")? {
    let entry = entry?;
    println!("{:?}", entry.file_name());
}
```

```rust
pub fn create<P: AsRef<Path>>(path: P) -> io::Result<File>
```

## Exercises or Practice Ideas

- `OpenOptions` bilan append-only file example yozing.
- `read_dir` orqali current katalogdagi faqat file nomlarini chiqaring.
- `AsRef<Path>` qabul qiladigan helper function yozib, unga `&str` va `PathBuf` yuboring.

## Questions Raised

- API imzosida `&str` qabul qilish kifoyami yoki `AsRef<Path>` kerakmi?
- `flush()` qaysi holatda semantik jihatdan muhim, qaysi holatda scope exit yetarli?
- `OsString`/`OsStr` bilan ishlashni qachon `String`ga majburan aylantirmaslik kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-file-system]]
- [[std-fs|std::fs]]
- [[open-options|OpenOptions]]
- [[osstring|OsString]]
- [[osstr|OsStr]]
- [[path|Path]]
- [[file-io]]
- [[io-error]]

