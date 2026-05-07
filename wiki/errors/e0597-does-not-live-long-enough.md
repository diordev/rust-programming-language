---
title: "E0597 does not live long enough"
type: error
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, compiler-error, lifetimes, borrow-checker]
source_count: 1
---

# E0597 does not live long enough

## Symptom

```
error[E0597]: `x` does not live long enough
```

Reference o'z scope'idan uzoqroq yashaydigan variable'ga ishora qilmoqchi bo'lganda paydo bo'ladi. Yoki funksiya chaqiruvidan qaytgan reference, manba ma'lumotlari scope'dan chiqqandan keyin ishlatilsa paydo bo'ladi.

## Cause

Reference qilingan variable scope'dan chiqadi — dropped bo'ladi — lekin reference hali ishlatilmoqchi.

**Klassik holat 1 — ichki scope:**

```rust
fn main() {
    let r;
    {
        let x = 5;
        r = &x;   // x bu scope'da yaratildi
    }             // x bu yerda dropped
    println!("r: {r}");  // r endi dangling — E0597
}
```

**Klassik holat 2 — funksiya qaytargan reference scope'dan tashqarida:**

```rust
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }             // string2 bu yerda dropped
    println!("{result}"); // result string2 ga ishora qilishi mumkin — E0597
}

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Compiler `'a` = kichik lifetime deb hisoblaydi — bu holda `string2` lifetime. `result` esa `string2` scope'dan tashqarida ishlatilmoqchi.

## Fix Pattern

**Holat 1 — scope mismatchda:** Reference saqlanadigan variable va reference qilinayotgan variable bir scopeda yoki manba uzoqroq yashovchi scopeda bo'lsin.

```rust
fn main() {
    let x = 5;        // x tashqi scopeda — r'dan uzoqroq yashaydi
    let r = &x;
    println!("r: {r}");
}
```

**Holat 2 — funksiya return reference:** Result ni o'sha scopeda ishlating yoki string2'ni tashqi scopega ko'chiring.

```rust
// Yechim 1: result string2 scope'i ichida ishlatilsin
fn main() {
    let string1 = String::from("long string is long");
    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("{result}"); // OK — hamma bir scopeda
    }
}

// Yechim 2: string2'ni tashqariga ko'chiring
fn main() {
    let string1 = String::from("long string is long");
    let string2 = String::from("xyz");
    let result = longest(string1.as_str(), string2.as_str());
    println!("{result}");
}
```

## Minimal Example

```rust
fn main() {
    let r;
    {
        let x = 5;
        r = &x;        // E0597 shu yerda
    }
    println!("{r}");
}
```

## Related Concepts

- [[lifetimes]]
- [[dangling-reference]]
- [[borrow-checker]]
- [[e0106-missing-lifetime-specifier]]
- [[e0515-cannot-return-local-reference]]
- [[reference]]
- [[scope]]

## Sources

- [[10-3-validating-references-with-lifetimes-the-rust-programming-language]]
