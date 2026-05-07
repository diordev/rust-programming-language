---
title: "Lifetimes"
type: concept
status: stable
created: 2026-05-06
updated: 2026-05-07
tags: [rust, borrowing, lifetimes]
source_count: 5
---

# Lifetimes

## Short Definition

Lifetimes — referencelar qancha vaqt valid bo'lishini ifodalovchi genericsning bir turi. Compiler lifetimes orqali barcha borrowlarning valid ekanini compile time'da tekshiradi.

## Why It Matters

Lifetimes Rustga [[dangling-reference|dangling reference]]ni compile time'da rad qilish imkonini beradi. Agar funksiya borrowed value qaytarsa, compiler bu borrow qaysi valid input yoki owner bilan bog'langanini bilishi kerak. Bu runtime xatolari o'rniga compile time xatolarini beradi va memory safety'ni kafolatlaydi.

## Mental Model

Reference owner emas — u faqat mavjud value'ga qaraydi. Owner scope'dan chiqib dropped bo'lsa, unga qaragan reference valid bo'lmaydi; Rust bunday holatni compile qilmaydi.

Lifetimes annotatsiyalar references qancha yashashini **o'zgartirmaydi** — ular faqat bir nechta references o'zaro munosabatini compilerga bildiradi. Borrow checker esa bu munosabatlardan foydalanib hamma borrowlarning valid ekanini tekshiradi.

Struct ichida reference fieldlar saqlansa, lifetime annotatsiya shu reference qaysi owner data'siga bog'langanini type definitionda ko'rsatishi kerak. Shuning uchun beginner stage'da [[structs]] uchun owned [[string-type|String]] fieldlar `&str` fieldlardan soddaroq.

## Syntax and Examples

### Funksiya signaturasida

Annotatsiya `<` bracket ichida e'lon qilinib, `&` dan keyin yoziladi:

```rust
// 'a: ikkala parametr va return value bir xil (minimum) lifetimega ega
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Amalda `'a` = `x` va `y`ning kichik lifetimesi. Return reference shu kichik lifetimegacha valid.

### Struct ichida

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

Bu `ImportantExcerpt` instance o'z `part` fielddagi referencedan uzoqroq yashay olmasligini bildiradi.

### Elision — annotatsiya yozmay ham compile bo'ladigan holat

```rust
// Bitta input, bir xil output — compiler avtomatik infer qiladi
fn first_word(s: &str) -> &str {
    &s[..]
}
```

### Bir parametrni return bilan bog'lash — boshqasini emas

Ikkita `&str` parametr bo'lganda, return faqat bittasiga bog'liq bo'lishi mumkin. Bu holatda faqat o'sha parametrga `'a` beriladi:

```rust
// 'a faqat contents va return'ga — query ga emas
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();
    for line in contents.lines() {
        if line.contains(query) {
            results.push(line);
        }
    }
    results
}
```

Sabab: qaytarilgan string slices `contents` matnidan kelib chiqadi, `query` dan emas. Compiler taklif qiladigan tuzatish (`query` va `contents` ga ham `'a`) texnik jihatdan compile bo'ladi, lekin semantik jihatdan noto'g'ri — ortiqcha cheklov.

Lifetime annotatsiyasiz E0106 xatosi: "signature does not say whether it is borrowed from `query` or `contents`".

### Owned value qaytarish — dangling reference'dan qochish

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s   // owned value, reference emas
}
```

### Generics + Traits + Lifetimes birgalikda

```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {ann}");
    if x.len() > y.len() { x } else { y }
}
```

## Common Mistakes

- Lifetime annotatsiyalar lifetimeni o'zgartiradi deb o'ylash — ular faqat munosabatlarni ifodalaydi.
- Return reference ortida valid owner bo'lishi kerakligini unutish — local valuega reference qaytarish [[e0515-cannot-return-local-reference|E0515]] beradi.
- Struct reference fields ham lifetime relationship talab qilishini unutish.
- Compiler `'static` taklif qilganda darhol qo'llash — ko'pincha asl muammoni yashiradi.
- Ikkita reference parameter va reference return bo'lsa explicit annotatsiya kerakligini bilmaslik.

## Related Concepts

- [[reference]]
- [[borrowing]]
- [[dangling-reference|dangling reference]]
- [[borrow-checker]]
- [[lifetime-elision]]
- [[static-lifetime]]
- [[structs]]
- [[string-slice|String slice]]
- [[generics]]
- [[generic-type-parameters|generic type parameters]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]
- [[e0597-does-not-live-long-enough|E0597 does not live long enough]]
- [[e0515-cannot-return-local-reference|E0515 cannot return local reference]]

## Sources

- [[4-2-references-and-borrowing-the-rust-programming-language]]
- [[5-1-defining-and-instantiating-structs-the-rust-programming-language]]
- [[10-generic-types-traits-and-lifetimes-the-rust-programming-language]]
- [[10-3-validating-references-with-lifetimes-the-rust-programming-language]]
- [[12-4-adding-functionality-with-test-driven-development-the-rust-programming-language]]
