---
title: "4.2. References and Borrowing - The Rust Programming Language"
type: source
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, rust-book, source, borrowing]
source_count: 1
source_path: "raw/books/the_rust_programming_language/4.2. References and Borrowing.md"
source_url: "https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html"
---

# 4.2. References and Borrowing - The Rust Programming Language

## Source

- Path: `raw/books/the_rust_programming_language/4.2. References and Borrowing.md`
- Type: book section
- URL: https://doc.rust-lang.org/stable/book/ch04-02-references-and-borrowing.html
- Date processed: 2026-05-06

## Detailed Summary

Bu section [[reference]] va [[borrowing]]ni ownership transfer muammosiga javob sifatida tushuntiradi. Oldingi `calculate_length` exampleida function `String` ownershipni olgani uchun caller value'dan keyin foydalana olmaydi. Reference orqali function value'dan foydalanadi, lekin ownershipni olmaydi.

Reference pointerga o'xshaydi: u address orqali data'ga boradi. Farqi: Rust reference lifetime davomida valid value'ga ishora qilishi compiler tomonidan kafolatlanadi. `&s1` syntax reference yaratadi, function signatureda `&String` reference parameter bildiradi.

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Reference owner emas, shuning uchun reference scope'dan chiqqanda pointed value dropped bo'lmaydi. Reference yaratish actioni borrowing deb ataladi: value egasi qoladi, boshqa code faqat borrow qiladi.

References default immutable. Borrowed value'ni o'zgartirishga urinish `E0596 cannot borrow as mutable behind & reference` xatosini beradi. O'zgartirish kerak bo'lsa, variable ham mutable bo'lishi, reference ham mutable bo'lishi kerak:

```rust
let mut s = String::from("hello");
change(&mut s);

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Mutable reference restriction: bir value'ga bir vaqtning o'zida faqat bitta mutable reference bo'lishi mumkin. Ikki `&mut s` simultaneous bo'lsa `E0499 cannot borrow as mutable more than once at a time` chiqadi. Bu Rustga data racesni compile time'da oldini olish imkonini beradi.

Mutable va immutable references ham overlap qila olmaydi. Agar immutable references hali ishlatilayotgan bo'lsa, shu value'ga mutable reference olish `E0502 cannot borrow as mutable because it is also borrowed as immutable` beradi. Multiple immutable references ruxsat etiladi, chunki ular read-only.

Reference scope declarationdan boshlanadi va oxirgi ishlatilgan joygacha davom etadi. Shu sababli immutable references oxirgi `println!`da ishlatilgandan keyin mutable reference olish mumkin; scopes overlap qilmaydi.

Dangling references Rustda taqiqlangan. Local `String`ga reference qaytaradigan function invalid, chunki local value function tugaganda dropped bo'ladi. Bunday code `E0106 missing lifetime specifier` chiqaradi, xabarning mazmuni: return type borrowed value saqlaydi, lekin borrow qiladigan valid value yo'q. Fix: owned `String`ni qaytarish.

Section reference rules bilan yakunlanadi:

- Har qanday vaqtda yoki bitta mutable reference, yoki istalgancha immutable references bo'lishi mumkin.
- References har doim valid bo'lishi kerak.

## Key Concepts

- [[reference]]
- [[borrowing]]
- [[mutable-reference|mutable reference]]
- [[dangling-reference|dangling reference]]
- [[data-race|data race]]
- [[ownership]]
- [[e0596-cannot-borrow-as-mutable|E0596 cannot borrow as mutable]]
- [[e0499-multiple-mutable-borrows|E0499 multiple mutable borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict|E0502 mutable and immutable borrow conflict]]
- [[e0106-missing-lifetime-specifier|E0106 missing lifetime specifier]]

## Code Examples

Immutable borrow:

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

Mutable borrow:

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

No dangling reference:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

## Exercises or Practice Ideas

- `calculate_length`ni ownership oladigan va borrow qiladigan versiyalarini solishtirish.
- `&String` reference bilan borrowed value'ni mutate qilib `E0596`ni ko'rish.
- Ikki `&mut` reference yaratib `E0499`ni ko'rish.
- Immutable reference oxirgi ishlatilgandan keyin mutable reference olishga ruxsat berilishini test qilish.
- Dangling reference exampleini yozib `E0106`ni ko'rish.

## Questions Raised

- Reference scope lexical scope emas, oxirgi use joyigacha davom etishi qanday borrow patternsni soddalashtiradi?
- Borrow checker data racesni compile time'da qanday oldini oladi?
- Lifetime annotationlar dangling reference muammosini keyinchalik qanday formal qiladi?

## Links To Update

- [[wiki/chapters/4-understanding-ownership|4. Understanding Ownership]]
- [[borrowing]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[dangling-reference|dangling reference]]
- [[slices]]
