---
title: "Borrowing nima va qanday ishlaydi?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, ownership, borrowing]
source_count: 2
---

# Borrowing nima va qanday ishlaydi?

## Answer

Borrowing — bu value'dan ownershipni olmasdan, faqat vaqtincha foydalanish usuli. Rustda bu odatda `&` orqali reference yaratish bilan qilinadi.

Ownership bilan farqi shunda:

- `ownership` value kimga tegishli ekanini belgilaydi
- `borrowing` esa shu value'dan kim vaqtincha foydalanishi mumkinligini belgilaydi

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
}

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}");
}
```

Bu misolda `calculate_length` `s1`ni borrow qiladi, lekin ownershipni olmaydi. Shuning uchun `main` ichida `s1`ni keyin ham ishlatish mumkin.

Borrowingning ikki asosiy turi bor:

- immutable borrow: `&T`
- mutable borrow: `&mut T`

Immutable borrow faqat o‘qish uchun. Bir value'ga bir vaqtda bir nechta immutable reference bo‘lishi mumkin:

```rust
let s = String::from("hello");
let r1 = &s;
let r2 = &s;
```

Mutable borrow esa o‘zgartirish uchun. Rust bu yerda qat’iy qoida qo‘yadi: bir vaqtda yoki bitta `&mut`, yoki ko‘p `&` bo‘ladi, lekin ikkalasi birga overlap qilmaydi.

```rust
fn change(s: &mut String) {
    s.push_str(", world");
}
```

Bu qoidaning maqsadi — data race va noto‘g‘ri aliasing’ni compile time’da to‘xtatish. Ya’ni bir data’ni bir paytda o‘zgartirish va boshqa joydan o‘qish xavfsiz bo‘lmagan holatlarni compiler yo‘q qiladi.

Praktik jihatdan borrowing ownership ceremonyini kamaytiradi:

- functionga `String` berib keyin qaytarib olish shart emas
- methodlarda `&self` bilan faqat o‘qish qilinadi
- `&mut self` bilan ichki state o‘zgartiriladi

Borrowingni yaxshi tushunsangiz, ownership qachon kerakligini, qachon reference yetarliligini tez ajratasiz.

## Evidence

- [[borrowing]]: reference yaratish orqali ownershipsiz foydalanish.
- [[reference]]: `&value` syntax va ownership transfer qilmaslik.
- [[mutable-reference|mutable reference]]: `&mut` va exclusive access qoidasi.
- [[ownership]]: borrowing ownershipni almashtirmaydi, faqat vaqtincha foydalanish beradi.
- [[4-2-references-and-borrowing-the-rust-programming-language]]: Rust Book’dagi asosiy izohlar va borrow checker qoidalari.

## Follow-up

- `&String` va `&str` farqini alohida ko‘rish.
- `&mut` nima uchun `mut` bo‘lmagan bindingda ishlamasligini tekshirish.
- Borrowing va lifetime bog‘lanishini keyingi bosqichda ochish.

## Related Pages

- [[borrowing]]
- [[reference]]
- [[mutable-reference|mutable reference]]
- [[ownership]]
- [[lifetimes]]
- [[e0596-cannot-borrow-as-mutable]]
- [[e0499-multiple-mutable-borrows]]
- [[e0502-mutable-and-immutable-borrow-conflict]]
