---
title: "4.3. The Slice Type - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, slices]
source_count: 1
source_path: "raw/books/the_rust_programming_language/4.3. The Slice Type.md"
source_url: "https://doc.rust-lang.org/stable/book/ch04-03-slices.html"
---

# 4.3. The Slice Type - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/4.3. The Slice Type.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch04-03-slices.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[slices]]ni tushuntiradi. Slice collection ichidagi contiguous sequence'ga reference bo'lib, ownershipga ega emas.

Problem: stringdagi birinchi wordni topadigan function yozish. Slice ishlatmasdan `first_word(s: &String) -> usize` byte index qaytarishi mumkin. Bu ishlaydi, lekin index `String` state'iga bog'lanmagan alohida value bo'lib qoladi. Agar `word = 5` olingandan keyin `s.clear()` qilinsa, `word` endi meaningful emas, lekin compiler buni ushlamaydi. Bu API fragile.

String slice `&str` shu muammoni hal qiladi. Slice `&s[start..end]` syntax bilan yaratiladi; start inclusive, end exclusive. Slice internally pointer va length saqlaydi. Range shorthand:

- `&s[0..2]` == `&s[..2]`
- `&s[3..len]` == `&s[3..]`
- `&s[0..len]` == `&s[..]`

String slice indices valid UTF-8 character boundariesda bo'lishi kerak. Multibyte character o'rtasidan slice olish runtime error bilan tugashi mumkin.

`first_word`ni `&str` qaytaradigan qilib yozish:

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

Endi return value underlying data bilan reference orqali bog'langan. Agar `word` slice active bo'lgan paytda `s.clear()` chaqirilsa, compiler `E0502` beradi, chunki `word` immutable borrow sifatida active, `clear` esa mutable borrow talab qiladi. Shunday qilib slices index out-of-sync bug classini compile time'da yo'q qiladi.

String literal aslida `&str`: binary ichidagi textga immutable slice. Shu sababli string literals immutable.

API design uchun experienced Rustacean `fn first_word(s: &String) -> &str` o'rniga `fn first_word(s: &str) -> &str` yozadi. Bu function `String` slice, whole `String` reference, partial `&str`, va string literal bilan ishlaydi. Bu flexibility deref coercions orqali keladi; details Chapter 15da.

Other slices: array yoki boshqa collection qismiga reference ham slice bo'lishi mumkin. Masalan `&a[1..3]` type `&[i32]` bo'ladi va pointer + length mental modeli string slicesga o'xshaydi.

Chapter 4 summary ownership, borrowing, va slices compile time memory safety beradi deb yakunlaydi. Rust systems programmingdagi memory controlni beradi, lekin owner scope'dan chiqqanda cleanup avtomatik bo'lgani uchun manual free/debug code kerak bo'lmaydi.

## Key Concepts

- [[slices]]
- [[string-slice|String slice]]
- [[string-type|String]]
- [[string-literal-vs-string]]
- [[reference]]
- [[borrowing]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[deref-coercions|deref coercions]]
- [[api-design|API design]]

## Code Examples

Idiomatic `first_word`:

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

String slices:

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
let whole = &s[..];
```

Array slice:

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

## Exercises or Practice Ideas

- `first_word`ni avval `usize`, keyin `&str` qaytaradigan qilib yozib API farqini solishtirish.
- `word` active paytda `s.clear()` qilib `E0502`ni ko'rish.
- `first_word(s: &str)` signature bilan `String`, `&String`, `&str`, va string literalni sinab ko'rish.
- Array slice `&[i32]` bilan kichik example yozish.

## Questions Raised

- Slice pointer + length mental modeli ownership bilan qanday bog'lanadi?
- `&str` parameter nima uchun `&String`dan flexible?
- UTF-8 character boundary restriction amaliy string processingda qanday ehtiyotkorlik talab qiladi?

## Links To Update

- [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]]
- [[slices]]
- [[string-slice|String slice]]
- [[string-literal-vs-string]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
