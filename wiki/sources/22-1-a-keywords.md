---
title: "22.1. A - Keywords"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, keywords, syntax, editions]
source_count: 1
---

# 22.1. A - Keywords

## Source

- File: `raw/books/the_rust_programming_language/22.1. A - Keywords.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-01-keywords.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix A Rust tilidagi keywordlar uchun compact reference beradi. Asosiy qoida: bu so'zlar til tomonidan current yoki future use uchun reserve qilingan, shu sabab ular oddiy [[identifiers]] sifatida ishlatilmaydi. Bitta muhim istisno bor: [[raw-identifiers]] syntax'i bilan keyword'ni identifier sifatida "escape" qilish mumkin.

Bo'lim avval *identifier* nimalarni nomlashi mumkinligini aniq sanab beradi: function, variable, parameter, struct field, module, crate, constant, macro, static value, attribute, type, trait, yoki lifetime. Bu ta'rif muhim, chunki keyword restriction aynan shu naming positionlarga tegishli.

Keyin reference ikki katta ro'yxat beradi.

### Keywords currently in use

Current keywords parser va language grammar'da hozir faol ma'no beradi. Ular orasida:

- control flow: `if`, `else`, `match`, `loop`, `while`, `for`, `in`, `break`, `continue`, `return`;
- item va type syntax'i: `fn`, `struct`, `enum`, `mod`, `crate`, `trait`, `impl`, `type`, `use`, `pub`, `where`, `extern`, `unsafe`, `union`, `self`, `Self`, `super`;
- bindings va mutability: `let`, `mut`, `ref`, `move`, `const`, `static`;
- async va dispatch bilan bog'liq syntax: `async`, `await`, `dyn`;
- literals va boshqa syntax: `true`, `false`, `as`.

Book har bir keyword uchun qisqa vazifani ham eslatadi. Masalan, `async` current thread'ni block qilmaydigan `Future` qaytaruvchi syntax oilasiga ulanadi; `dyn` trait object va dynamic dispatch bilan bog'liq; `extern` tashqi function yoki variable bilan linklashda ishlatiladi.

### Keywords reserved for future use

Ikkinchi ro'yxat hali functionality olmagan, lekin til kelajak uchun band qilib qo'ygan so'zlardan iborat:

- `abstract`
- `become`
- `box`
- `do`
- `final`
- `gen`
- `macro`
- `override`
- `priv`
- `try`
- `typeof`
- `unsized`
- `virtual`
- `yield`

Bu ro'yxatning amaliy ma'nosi: bugun ham bu so'zlarni oddiy identifier sifatida tanlash xavfsiz emas, chunki til ularni keyinroq semantic ma'no bilan ishlatishi mumkin.

### Raw identifiers

Bo'limning eng amaliy qismi shu. Agar keyword bilan nom conflict bo'lsa, `r#` prefiksi yordam beradi:

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```

Bu yerda `match` keyword, lekin `r#match` identifier sifatida qabul qilinadi. Muhim detail: `r#` faqat definitionda emas, use site'da ham yoziladi:

```rust
assert!(r#match("foo", "foobar"));
```

Book aynan compile error misolini ham beradi: `fn match(...)` yozilsa parser "expected identifier, found keyword" deydi.

### Edition compatibility

Raw identifier faqat naming freedom uchun emas, [[edition]]lar orasidagi compatibility uchun ham kerak. Book `try` misolini beradi:

- 2015 edition'da `try` keyword emas;
- 2018, 2021, va 2024 edition'larda u reserved keyword.

Shuning uchun 2015 edition'da yozilgan library `try` nomli function eksport qilsa, keyingi edition crate undan `r#try(...)` deb foydalanishi kerak. Demak raw identifiers cross-edition bridge vazifasini bajaradi.

### Yakuniy takeaway

Appendix A lexical reference bo'lsa ham, bitta juda practical rule beradi: keyword conflict bo'lganda yoki eski edition API bilan yashash kerak bo'lganda `r#keyword` Rust'ning standart escape hatch'i hisoblanadi.

## Key Concepts

- [[keywords]]
- [[raw-identifiers]]
- [[identifiers]]
- [[edition]]
- [[async-await]]
- [[future]]
- [[match]]

## Code Examples

- [[raw-identifier-match-function|raw identifier `match` function]]

## Exercises or Practice Ideas

1. Current va future reserved keywordlar o'rtasidagi amaliy farqni yozing.
2. `fn match(...)` xatosi nega parse bosqichidayoq chiqishini tushuntiring.
3. Nega `r#` prefiksini definition va call site'ning ikkalasida ham yozish kerakligini izohlang.
4. 2015 edition'dagi `try` API'ni 2024 edition crate'dan qanday chaqirishni yozing.
5. `dyn`, `unsafe`, `where`, `crate`, va `pub` keywordlari qaysi existing concept sahifalarga ulanayotganini toping.

## Questions Raised

- Rust'dagi barcha keywordlar "hard keyword"mi, yoki ba'zilari context-sensitive rol o'ynaydimi?
- Kelajakda `gen` yoki `yield` faol syntax bo'lsa, naming style'ga qanchalik ta'sir qiladi?
- Public API design'da keywordga juda yaqin nomlar ham qochilishi kerakmi?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[keywords]]
- [[raw-identifiers]]
- [[identifiers]]
- [[wiki/chapters/22-1-a-keywords|Chapter 22.1]]
