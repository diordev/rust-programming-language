---
title: "Spreadsheet cell vector"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, collections, enums]
source_count: 1
---

# Spreadsheet cell vector

## Goal

Bir [[vector|Vec<T>]] ichida har xil data shape'larini [[enums|enum]] orqali bitta concrete type sifatida saqlash.

## Code

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

fn main() {
    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];

    for cell in &row {
        match cell {
            SpreadsheetCell::Int(value) => println!("int: {value}"),
            SpreadsheetCell::Float(value) => println!("float: {value}"),
            SpreadsheetCell::Text(value) => println!("text: {value}"),
        }
    }
}
```

## Explanation

`Vec<T>` bitta concrete type saqlaydi. Bu example'da concrete type `SpreadsheetCell`; variants esa `i32`, `f64`, va `String` payloadlarni bitta enum ostida birlashtiradi.

`match` har bir variantni handle qiladi. Yangi variant qo'shilsa va catch-all ishlatilmasa, compiler exhaustive matching orqali missing case'larni ko'rsatadi.

## Related Pages

- [[8-1-storing-lists-of-values-with-vectors-the-rust-programming-language]]
- [[vector|Vec<T>]]
- [[enums]]
- [[enum-variants|enum variants]]
- [[match]]
- [[exhaustive-matching|exhaustive matching]]
