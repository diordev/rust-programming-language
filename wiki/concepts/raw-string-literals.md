---
title: "Raw String Literals"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, strings, syntax]
source_count: 1
---

# Raw String Literals

## Short Definition

`r"..."`, `r#"..."#`, `r##"..."##` kabi syntax'lar escape sequence'larni process qilmaydigan string literal familyasi.

## Why It Matters

Backslash va quote ko'p bo'lgan textlar, regexlar, Windows path'lar, yoki embedded snippets yozishda ordinary stringga qaraganda ancha oson.

## Mental Model

Ordinary string `"a\nb"` ichida `\n` newline bo'ladi. Raw stringda esa `r"a\nb"` literal backslash + `n` bo'lib qoladi.

`#` belgilari ichki quote'larni qochirmasdan yozish uchun delimiter'ni kuchaytiradi.

## Syntax and Examples

```rust
let path = r"C:\Users\mkb\notes";
let json = r#"{"name":"rust","safe":true}"#;
let snippet = r##"He said: "hello""##;
```

Raw byte string ham mavjud:

```rust
let bytes = br#"line\nstill raw"#;
```

## Common Mistakes

- Raw string newline va backslashlarni interpret qiladi deb o'ylash.
- Keraklicha `#` qo'ymay ichki `"` sabab delimiter'ni erta yopib qo'yish.
- Raw string va byte stringni bir xil deb o'ylash.

## Related Concepts

- [[string-literal|string literal]]
- [[string-type|String]]
- [[utf-8|UTF-8]]
- [[rust-operators-and-symbols]]

## Sources

- [[wiki/sources/22-2-b-operators-and-symbols|22.2]]
