---
title: "22.1. A - Keywords"
type: chapter
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, keywords, syntax, editions]
source_count: 1
---

# 22.1. A - Keywords

## Learning Goal

Rust keywordlari qaysi rolda ishlashini, future reserved so'zlar nimani anglatishini, va [[raw-identifiers]] bilan keyword conflict hamda [[edition]] compatibility muammolarini hal qilishni tushunish.

## Main Ideas

- Keyword Rust grammar'ning reserved vocabulary'si; oddiy [[identifiers]] sifatida ishlatilmaydi.
- Appendix A ikki toifani beradi: hozir ishlatilayotgan keywordlar va kelajak uchun reserved keywordlar.
- Amaliy jihatdan eng muhim qoida: `r#keyword` syntax'i keyword'ni identifier sifatida ishlatishga ruxsat beradi.
- Bu nafaqat naming escape hatch, balki cross-edition compatibility vositasi hamdir.

## Keyword Groups

- Control flow va branching: `if`, `else`, `match`, `loop`, `while`, `for`, `in`, `break`, `continue`, `return`.
- Item, module, va type syntax'i: `fn`, `struct`, `enum`, `mod`, `crate`, `trait`, `impl`, `type`, `use`, `pub`, `where`, `extern`, `unsafe`, `union`, `self`, `Self`, `super`.
- Binding, mutability, va storage: `let`, `mut`, `ref`, `move`, `const`, `static`.
- Async, dispatch, va literal syntax'i: `async`, `await`, `dyn`, `true`, `false`, `as`.

## Raw Identifier Rule

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

- `fn match(...)` parse error beradi.
- `fn r#match(...)` to'g'ri.
- Call site ham `r#match(...)` bo'lishi kerak.

## Edition Compatibility

- `try` 2015 edition'da keyword emas.
- `try` 2018, 2021, va 2024 edition'larda reserved.
- Shu sabab keyingi edition crate eski API'ni `r#try(...)` deb chaqiradi.

## Concepts

- [[keywords]]
- [[identifiers]]
- [[raw-identifiers]]
- [[edition]]
- [[async-await]]
- [[future]]
- [[extern-block]]
- [[unsafe-rust]]
- [[use-declarations]]
- [[pub-keyword]]

## Examples

- [[raw-identifier-match-function|raw identifier `match` function]]

## Exercises

1. `dyn`, `extern`, `unsafe`, `pub`, `where`, va `crate` keywordlarini tegishli concept sahifalari bilan bog'lang.
2. `r#match` example'ini `r#try` edition compatibility example'iga aylantirib yozing.
3. Future reserved keywordlardan uchtasini tanlab, nega bugun ham identifier sifatida tanlamaslik kerakligini izohlang.
4. `Self` va `self` keywordlari orasidagi farqni existing wiki sahifalari yordamida tushuntiring.

## Review Questions

1. Keyword nima?
2. Future reserved keyword nimani anglatadi?
3. Raw identifier syntax'i qanday yoziladi?
4. `r#` prefiksi qachon kerak bo'ladi?
5. `try` misolida edition compatibility qanday ishlaydi?
6. Identifier qaysi narsalarning nomi bo'lishi mumkin?

## Related Pages

- [[wiki/sources/22-1-a-keywords|Source: 22.1]]
- [[wiki/chapters/22-appendix|Chapter 22]]
- [[keywords]]
- [[raw-identifiers]]
