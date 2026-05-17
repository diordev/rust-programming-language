---
title: "8.2. Storing UTF-8 Encoded Text with Strings - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, collections, strings, utf-8]
source_count: 1
source_path: "raw/books/the_rust_programming_language/8.2. Storing UTF-8 Encoded Text with Strings.md"
source_url: "https://doc.rust-lang.org/stable/book/ch08-02-strings.html"
---

# 8.2. Storing UTF-8 Encoded Text with Strings - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/8.2. Storing UTF-8 Encoded Text with Strings.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch08-02-strings.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[string-type|String]]ni [[collections|collections]] kontekstida chuqurlashtiradi. Rustda strings new Rustaceans uchun ko'pincha qiyin ko'rinadi, chunki Rust possible errorsni yashirmaydi, string data structure ko'p programmerlar o'ylaganidan murakkabroq, va hammasi [[utf-8|UTF-8]] bilan bog'langan.

Rustda "string" deganda ikki asosiy type nazarda tutilishi mumkin: core language ichidagi `str`, odatda borrowed form `&str` sifatida ishlatiladi, va standard librarydagi owned [[string-type|String]]. [[string-slice|String slice]] `&str` boshqa joyda saqlangan UTF-8 encoded string data'ga reference beradi; string literals program binary'sida saqlanadi va `&str` hisoblanadi. `String` esa growable, mutable, owned, UTF-8 encoded text type.

`String` collection sifatida qaraladi, chunki u bytes collectioni ustidagi wrapper: amalda `Vec<u8>` ustiga qo'shimcha guarantees, restrictions, va text-oriented methods qo'yilgan. Shu sababli `String` va [[vector|Vec<T>]]da o'xshash operations bor, masalan `new` bilan bo'sh value yaratish.

Bo'sh `String`:

```rust
let mut s = String::new();
```

Initial data bilan `String` yaratish uchun `to_string()` methodi yoki `String::from()` ishlatiladi. `to_string()` `Display` implement qilgan typelarda mavjud; string literals `Display` implement qilgani uchun `"initial contents".to_string()` ishlaydi. `String::from("initial contents")` ham xuddi shu natijani beradi. Bu ikki style bir-biriga yaqin; tanlov ko'pincha style va readability masalasi.

UTF-8 sababli Rust `String` ko'p tillardagi valid textni saqlay oladi: Arabic, Czech, Hebrew, Hindi, Japanese, Korean, Chinese, Portuguese, Russian, Spanish va boshqalar. Bu Rust stringlari ASCII-only emasligini eslatadi.

`String` mutable/growable collection bo'lgani uchun update qilinadi. `push_str` methodi string slice (`&str`) append qiladi. U ownershipni olmaydi, shuning uchun append qilingan slice keyin ham ishlatilishi mumkin:

```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s2 is {s2}");
```

`push` methodi esa bitta `char` qo'shadi:

```rust
let mut s = String::from("lo");
s.push('l');
```

String concatenation uchun `+` operator ishlatilishi mumkin, lekin ownership semantics muhim. `let s3 = s1 + &s2;` yozilganda `s1` moved bo'ladi va keyin invalid; `s2` reference sifatida berilgani uchun valid qoladi. Bu `+` aslida `add(self, s: &str) -> String`ga yaqin signature bilan ishlagani sababli: left side ownership olinadi, right side string slice sifatida qo'shiladi. `&String`dan `&str`ga o'tish [[deref-coercions|deref coercion]] orqali bo'ladi.

Bir nechta stringni `+` bilan qo'shish o'qilishi qiyinlashadi. [[format-macro|format! macro]] `println!`ga o'xshaydi, lekin stdoutga yozmaydi; `String` qaytaradi. `format!("{s1}-{s2}-{s3}")` ownershipni olmaydi, chunki macro referencesdan foydalanadi. Murakkab concatenation uchun `format!` ko'pincha readable va ergonomikroq.

Rust `String`ni integer index bilan access qilishga ruxsat bermaydi. `let h = s1[0];` compile bo'lmaydi va [[e0277-trait-bound-not-satisfied|E0277]] chiqishi mumkin, chunki `str` integer index bilan indexed bo'lmaydi. Sabab: `String` UTF-8 bytes sifatida saqlanadi; byte index har doim valid character yoki user kutgan "letter"ga mos kelmaydi.

`String::len()` bytes sonini qaytaradi, human-perceived characters sonini emas. `"Hola"` 4 bytes; `"Здравствуйте"` esa 24 bytes, chunki Cyrillic scalar values UTF-8da 2 bytedan joy oladi. Agar `&"hi"[0]` valid bo'lganida ham, u `h` emas, byte value `104` bo'lardi. Rust bunday noaniq va ko'pincha noto'g'ri expectationni compile time'da rad qiladi.

UTF-8 stringni uch darajada ko'rish mumkin: raw bytes (`u8` values), Unicode scalar values (`char`), va [[grapheme-clusters|grapheme clusters]] (odam "harf" deb qabul qiladigan birlikka eng yaqin). Hindi "नमस्ते" example'ida bytes 18 ta, `char` values 6 ta, grapheme clusters esa 4 ta. Diacritics alohida `char` bo'lishi mumkin, lekin yakka holda "letter" sifatida ma'noli emas.

String slice yaratishda single integer indexing emas, range ishlatiladi: `&hello[0..4]`. Bu byte range bo'yicha slice oladi va valid UTF-8 boundarylarda bo'lishi shart. `"Здравствуйте"`da `&hello[0..4]` valid va `Зд` beradi, chunki har bir Cyrillic character 2 byte. Lekin `&hello[0..1]` runtime [[panic|panic]] qiladi, chunki byte index 1 character boundary emas.

String bo'laklari ustida ishlashning eng yaxshi yo'li nima kerakligini explicit qilish: Unicode scalar values uchun `.chars()`, raw bytes uchun `.bytes()`. `"Зд".chars()` `З` va `д` chars beradi; `"Зд".bytes()` esa `208`, `151`, `208`, `180` raw bytes beradi. Grapheme clusters standard libraryda yo'q; kerak bo'lsa crates.io'dagi crates ishlatiladi.

Section xulosasi: Rust strings murakkabligini yashirmaydi. Bu upfront fikrlashni talab qiladi, lekin non-ASCII text bilan keyin paydo bo'ladigan yashirin bugsni oldini oladi. Standard library `String` va `&str` ustida `contains`, `replace` kabi ko'p helper methods beradi.

## Key Concepts

- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[collections]]
- [[vector|Vec<T>]]
- [[string-concatenation|string concatenation]]
- [[format-macro|format! macro]]
- [[string-indexing|string indexing]]
- [[grapheme-clusters|grapheme clusters]]
- [[char-type|char type]]
- [[deref-coercions|deref coercions]]
- [[panic]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]

## Code Examples

Creating strings:

```rust
let mut s = String::new();

let data = "initial contents";
let s = data.to_string();
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

UTF-8 strings:

```rust
let hello = String::from("Здравствуйте");
let hello = String::from("こんにちは");
let hello = String::from("你好");
```

Updating:

```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s2 is {s2}");

let mut s = String::from("lo");
s.push('l');
```

Concatenation:

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2;
```

Formatting:

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{s1}-{s2}-{s3}");
```

Invalid string indexing:

```rust
let s1 = String::from("hi");
let h = s1[0];
```

String slicing with ranges:

```rust
let hello = "Здравствуйте";
let s = &hello[0..4]; // "Зд"
```

Iterating chars vs bytes:

```rust
for c in "Зд".chars() {
    println!("{c}");
}

for b in "Зд".bytes() {
    println!("{b}");
}
```

## Exercises or Practice Ideas

- `String::from("initial contents")` va `"initial contents".to_string()`ni bir xil natija, lekin turli style sifatida tushuntiring.
- `push_str` nima uchun `&str` olishini ownership nuqtayi nazaridan izohlang.
- `let s3 = s1 + &s2;`dan keyin qaysi variable valid qolishini yozing.
- `format!("{s1}-{s2}-{s3}")` va `s1 + "-" + &s2 + "-" + &s3`ni readability va ownership bo'yicha solishtiring.
- `"Здравствуйте".len()` nima uchun 24 bo'lishini tushuntiring.
- `bytes`, `chars`, va grapheme clusters farqini "नमस्ते" example'i orqali yozing.
- `&hello[0..1]` nima uchun runtime panic qilishini character boundary bilan tushuntiring.

## Questions Raised

- API `String` olishi kerakmi yoki `&str`?
- User-facing "character count" kerak bo'lsa `.len()` yetarlimi?
- Grapheme cluster kerak bo'lgan text processingda qaysi crate ishlatiladi?
- String concatenation uchun qachon `+`, qachon `format!` tanlash kerak?
- String slice range'lari byte boundaryga tayanishi API designni qanday murakkablashtiradi?

## Links To Update

- [[wiki/chapters/8-common-collections|8. Common Collections]]
- [[string-type|String]]
- [[string-slice|String slice]]
- [[utf-8|UTF-8]]
- [[string-concatenation|string concatenation]]
- [[format-macro|format! macro]]
- [[string-indexing|string indexing]]
- [[grapheme-clusters|grapheme clusters]]
- [[e0277-trait-bound-not-satisfied|E0277 trait bound not satisfied]]
