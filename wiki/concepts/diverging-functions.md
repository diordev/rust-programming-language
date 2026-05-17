---
title: "Diverging Functions"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, functions, control-flow, types]
source_count: 1
---

# Diverging Functions

## Short Definition

Hech qachon qaytmaydigan funksiya. Return tipi [[never-type|`!`]] bilan belgilanadi: `fn name() -> !`. Funksiya panic, loop forever, yoki dasturni tugatishi mumkin — har holda call site'iga qiymat qaytarmaydi.

## Why It Matters

Diverging funksiya `!` qaytargani uchun, uni har qanday kontekstda ishlatish mumkin (`!` har tipga coerce bo'ladi). Bu juda kuchli abstraktsiya: `panic!()` `match` arm'larida har xil tip kontekstida ishlay oladi.

## Mental Model

```
fn returns_u32() -> u32      →  qaytaradi u32
fn returns_unit() -> ()      →  qaytaradi ()
fn diverges()    -> !        →  hech qachon qaytmaydi
```

`!` — "shu funksiya'ga keyin hech narsa kelmaydi." Kompilyator dead code'ni aniqlaydi.

## Misollar

### `panic!`

```rust
fn explode() -> ! {
    panic!("end of the world");
}
```

`panic!` makrosi o'zi `!` qaytaradi.

### Cheksiz loop

```rust
fn server() -> ! {
    loop {
        accept_connection();
    }
}
```

`break` yo'q → loop hech qachon tugamaydi.

### `process::exit`

```rust
fn fail_fast(code: i32) -> ! {
    eprintln!("fatal");
    std::process::exit(code);   // OS process tugaydi
}
```

### Standart Kutubxonadagi Diverging Funksiya/Makroslar

| Element | Qaytarish | Tavsif |
|---------|-----------|--------|
| `panic!()` | `!` | Stack unwind, abort |
| `unreachable!()` | `!` | "Bu yerga yetmaydi" deb ifodalovchi panic |
| `todo!()` | `!` | "Hali yozilmagan" |
| `unimplemented!()` | `!` | "Implementatsiya talab qilinmaydi" |
| `process::exit(code)` | `!` | OS process tugatish |
| `process::abort()` | `!` | Abort signali |
| `std::thread::panicking()` ichida loop | `!` | rare |

## `!` Coercion'ning Foydasi

```rust
fn parse_or_die(s: &str) -> u32 {
    match s.parse() {
        Ok(n) => n,
        Err(_) => panic!("invalid number"),   // ! → u32 ga coerce
    }
}
```

Yoki `unwrap_or_else` bilan:

```rust
let n: u32 = "abc".parse().unwrap_or_else(|_| panic!("bad input"));
```

`panic!` `!` qaytaradi → closure `u32` qaytarayotgani sababli mos keladi.

## Common Mistakes

- **Diverging funksiyada `return value;` yozish.** Kompilyator xato — `!` qaytarish mumkin emas.
- **`-> !` belgilab, lekin oddiy yo'l bilan tugaydigan funksiya yozish.** Kompilyator unreachable code haqida xato beradi.
- **`if condition { panic!() } else { 5 }` da tip noaniqligini kutish.** Aksincha — `panic!()` `!` qaytarib, `5` (`i32`) tipini qabul qiladi → ifoda `i32`.

## Related Concepts

- [[never-type|never type (!)]] — qaytarish tipi
- [[panic]] — eng keng tarqalgan diverging operatsiya
- [[loop]] — break'siz cheksiz loop
- [[control-flow|control flow]]
- [[match]] — `!` coercion'ning klassik konteksti
- [[process-exit|process::exit]]

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
