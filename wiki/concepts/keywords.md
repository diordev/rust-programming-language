---
title: "Keywords"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-13
tags: [rust, syntax]
source_count: 2
---

# Keywords

## Short Definition

Keywords Rust language tomonidan current yoki future use uchun reserved qilingan so'zlar bo'lib, odatiy [[identifiers]] sifatida ishlatilmaydi. Istisno: [[raw-identifiers]].

## Why It Matters

Variable yoki function nomi tanlashda keyword bilan conflict bo'lsa code compile bo'lmaydi. Bundan tashqari, keywordlar [[edition]]lar orasida compatibility masalasini ham keltirib chiqarishi mumkin: bir edition'da oddiy nom bo'lgan so'z keyingi edition'da reserved bo'lib qolishi mumkin.

## Mental Model

Keyword parser uchun maxsus ma'noga ega. Bir qismi grammar'da hozir faol ishlatiladi, bir qismi esa til keyin foydalanishi mumkin bo'lgan reserve joydir. Agar shunday so'zni nom sifatida ishlatish zarur bo'lsa, `r#keyword` syntax'i ishlatiladi.

## Syntax and Examples

```rust
let value = 1;
```

### Current keywords

- Control flow: `if`, `else`, `match`, `loop`, `while`, `for`, `in`, `break`, `continue`, `return`.
- Items va modules: `fn`, `struct`, `enum`, `mod`, `crate`, `use`, `pub`, `trait`, `impl`, `type`, `where`, `extern`, `unsafe`, `union`, `self`, `Self`, `super`.
- Bindings va storage: `let`, `mut`, `ref`, `move`, `const`, `static`.
- Async, dispatch, va literals: `async`, `await`, `dyn`, `true`, `false`, `as`.

Bu keywordlar existing concept sahifalari bilan to'g'ridan-to'g'ri ulanadi: [[async-await]], [[future]], [[break]], [[crate]], [[match]], [[module]], [[move-semantics]], [[pub-keyword]], [[static-items]], [[static-lifetime]], [[traits]], [[type-alias]], [[unsafe-rust]], [[use-declarations]], [[where-clauses]], [[extern-block]], [[impl-block]], [[for-loop]], [[enum-variants]], [[trait-object]], va [[dynamic-dispatch]].

### Future reserved keywords

Quyidagi so'zlar hozir functionality bermaydi, lekin kelajak uchun band:

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

### Raw identifiers

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

Bu yerda `match` keyword bo'lsa ham, `r#match` identifier sifatida o'qiladi. Qarang: [[raw-identifier-match-function]].

### Edition compatibility

Raw identifier'ning amaliy kuchli joyi cross-[[edition]] compatibility:

```rust
let value = legacy_crate::r#try();
```

`try` 2015 edition'da keyword emas, lekin keyingi editionlarda reserved. Shu sabab eski API'ni yangi edition code'dan chaqirishda `r#try` kerak bo'lishi mumkin.

## Common Mistakes

- Reserved keywordni identifier nomi sifatida ishlatish.
- `r#` prefiksini faqat definitionda yoki faqat call site'da yozish.
- Future reserved keywordlarni "hali ishlamayapti-ku" deb nom sifatida tanlab yuborish.
- Edition farqlarini e'tiborsiz qoldirish.

## Related Concepts

- [[variables-and-mutability|variables and mutability]]
- [[functions]]
- [[identifiers]]
- [[raw-identifiers]]
- [[edition]]
- [[async-await]]
- [[future]]
- [[module]]
- [[crate]]
- [[traits]]
- [[static-items]]
- [[static-lifetime]]
- [[unsafe-rust]]
- [[use-declarations]]
- [[where-clauses]]

## Sources

- [[wiki/sources/3-common-programming-concepts]]
- [[wiki/sources/22-1-a-keywords|22.1]]
