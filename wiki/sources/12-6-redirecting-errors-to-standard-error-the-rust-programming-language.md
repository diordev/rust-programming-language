---
title: "12.6. Redirecting Errors to Standard Error"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, stderr, stdout, eprintln, minigrep, cli]
source_count: 1
---

# 12.6. Redirecting Errors to Standard Error

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: 12.6 — Redirecting Errors to Standard Error
- URL: https://doc.rust-lang.org/stable/book/ch12-06-writing-to-stderr-instead-of-stdout.html

## Detailed Summary

Bu qisqa lekin muhim bo'lim command line dasturlarining konventsiyasi haqida: xato xabarlari `stderr`ga, oddiy ma'lumotlar `stdout`ga yuborilishi kerak. Bu foydalanuvchiga `stdout`ni faylga yo'naltirib, xato xabarlarni ekranda ko'rish imkonini beradi.

### Muammo: `println!` hamma narsani `stdout`ga yozadi

Dastlab `minigrep` barcha chiqishlarni — jumladan xato xabarlarni — `stdout`ga yozardi:

```shell
$ cargo run > output.txt
```

Bu holda ekranda hech narsa ko'rinmaydi, xato xabari ham `output.txt`ga tushadi. Bu noto'g'ri xulq-atvor — xato xabari ekranda ko'rinishi kerak edi.

### Yechim: `eprintln!` makrosi

`eprintln!` `stderr`ga yozadi, `println!` esa `stdout`ga:

```rust
fn main() {
    let args: Vec<String> = env::args().collect();

    let config = Config::build(&args).unwrap_or_else(|err| {
        eprintln!("Problem parsing arguments: {err}");  // stderr
        process::exit(1);
    });

    if let Err(e) = run(config) {
        eprintln!("Application error: {e}");  // stderr
        process::exit(1);
    }
}
```

Faqat xato xabarlar joylardagi `println!` → `eprintln!` o'zgartirildi. Muvaffaqiyatli natijalar `stdout`da qoladi.

### Natija

```shell
# Xato holati — xato xabari ekranda ko'rinadi, fayl bo'sh
$ cargo run > output.txt
Problem parsing arguments: not enough arguments

# Muvaffaqiyatli holat — natija faylga tushadi, ekranda hech narsa yo'q
$ cargo run -- to poem.txt > output.txt
# (ekranda hech narsa)
# output.txt ichida:
# Are you nobody, too?
# How dreary to be somebody!
```

### Bo'lim xulosasi (Chapter 12 summary)

Bu bo'lim 12-bobni yakunlaydi. Quyidagilar o'zlashtirildi:
- CLI argumentlarni qabul qilish (`env::args`)
- Faylni o'qish (`fs::read_to_string`)
- Environment variable orqali konfiguratsiya (`env::var`)
- Xatolarni `stderr`ga yo'naltirish (`eprintln!`)
- Keyingi bob: closure va iteratorlar (funksional dasturlash elementlari)

## Key Concepts

- [[eprintln-macro|eprintln!]] — `stderr`ga chiqarish
- [[stdout]] — oddiy chiqish oqimi
- [[stderr]] — xato chiqish oqimi
- [[println-macro|println!]] — `stdout`ga chiqarish

## Code Examples

```rust
// Xato xabari stderr ga
eprintln!("Problem parsing arguments: {err}");

// Oddiy chiqish stdout ga (o'zgarishsiz)
println!("{line}");
```

## Exercises or Practice Ideas

1. `minigrep` ni ishga tushirib `stderr`ni ham faylga yo'naltiring (`2> errors.txt`) va ikki oqimning farqini kuzating.
2. O'z loyihangizda barcha xato xabarlarni `eprintln!`ga o'tkazing.

## Questions Raised

- `stderr`ni ham faylga yo'naltirish uchun shell sintaksisi: `2> errors.txt` yoki `2>&1` — buni tushuntiring.
- `log` crate yordamida `stderr`/`stdout` boshqarishni qanday avtomatlashtiriladi?

## Links To Update

- [[12-an-io-project]] — 12.6 bo'limini qo'shish
- [[index]] — manba havolasini qo'shish
- [[eprintln-macro]] — yangi kontseptsiya sahifasi
