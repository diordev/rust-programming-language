---
title: "Деструктурирование - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, patterns]
source_count: 1
---

# Деструктурирование - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/29. Деструктурирование.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/destructuring-assignment.html

## Detailed Summary

Bu source Rustdagi murakkab value'ni shape bo'yicha ajratib olish usulini ko'rsatadi. Termin sifatida source "destructuring assignment" deydi, lekin durable model uchun bu ko'proq [[pattern-destructuring|pattern destructuring]]: `let` bindinglarda, function parametrlarda, va boshqa pattern position'larda value ichki qismlarini bindinglarga ajratish.

Kirish tuple misoli bilan boshlanadi va bu to'g'ri: tuple destructuring positional modelni eng sodda ko'rinishda beradi. Muammo yo'q joyda ham source nestingni ko'rsatadi, bu foydali, chunki Rust patternlari flat emas. `(num, c, _, (d1, d2, d3))` ko'rinishidagi pattern bir vaqtning o'zida ignored field, outer tuple, va inner tuple bindinglarini birlashtiradi. Bu pattern syntax keyin `match` va enum variantlarida tabiiy davom etadi.

Array bo'limi tuple'dan keyingi eng muhim farqni beradi: array destructuring compile-time length ma'lum bo'lgani uchun ishlaydi. Source `..` va `rest @ ..` bilan fixed-length arraydan bosh qismini ajratib, qolgan tail'ni ignore qilish yoki ushlab qolishni ko'rsatadi. Bu yerda saqlanadigan asosiy prinsip: array patterni shape-sensitive; `[T; 5]` va `[T; 3]` bir xil pattern bilan o'qilmaydi.

Source slice va vector uchun oddiy destructuring nega ishlamasligini ham to'g'ri ajratadi: compile time'da uzunlik kafolatlanmagan. Shu sabab `let [a, b] = slice;` tipidagi model umumiy slice uchun ishlamaydi. Bu keyingi chapter bilan yaxshi ulanadi, chunki [[slice-patterns|slice patterns]] esa aynan `match` ichida ishlaydi: mismatch bo'lsa keyingi arm tekshiriladi. Demak "array destructuring mumkin, slice destructuring yo'q" degan gapni shartli o'qish kerak; plain bindingda yo'q, `match`da esa pattern sifatida bor.

Struct destructuring positional emas, field name asosida ishlaydi. Source `Person { name, age }` va `Person { name, .. }` orqali record-like data'dan kerakli qismini olishni ko'rsatadi. Bu beginner uchun muhim o'tish nuqtasi: structning foydasi faqat named field emas, pattern tarafida ham shu nomlar semantic selection beradi.

Oxirgi foydali qatlam function parametrlaridagi destructuring. Bu bo'lim patternlar `let` bilan cheklanmasligini ko'rsatadi: parameter list ham pattern position. `fn print_point_2d(Point2D { x, y }: Point2D)` yoki tuple struct parameteri `Rgba(_, _, _, alpha): Rgba` function signature'ni ancha ma'noli qiladi, lekin ortiqcha murakkablashtirib yubormaslik kerak.

## Key Concepts

- [[pattern-destructuring|pattern destructuring]]
- [[tuple]]
- [[array]]
- [[array-destructuring|array destructuring]]
- [[discarded-binding]]
- [[structs]]
- [[struct-fields|struct fields]]
- [[slice-patterns|slice patterns]]
- [[functions]]

## Code Examples

```rust
let tup: (i32, char, bool, (i32, i32, i32)) = (1, 'z', true, (7, 8, 9));
let (num, c, _, (d1, d2, d3)) = tup;
```

```rust
let arr: [i32; 5] = [1, 2, 3, 4, 5];
let [first, _, third, rest @ ..] = arr;
println!("{first} {third} {rest:?}");
```

```rust
struct Point2D {
    x: i32,
    y: i32,
}

fn print_point(Point2D { x, y }: Point2D) {
    println!("({x}, {y})");
}
```

## Exercises or Practice Ideas

- Tuple, array, va struct uchun bittadan nested destructuring misoli yozing.
- `[u8; 6]` arraydan header, body, tail qismlarini pattern bilan ajrating.
- Function parametrida destructuring ishlatib `Point3D` yoki `Rgba` type uchun helper yozing.
- Qaysi holatda destructuring readability'ni oshiradi, qaysi holatda kamaytiradi, ikkita example bilan ko'rsating.

## Questions Raised

- Destructuringni qachon signature'da yozish foydali, qachon function body ichida qoldirish yaxshiroq?
- Patternning o'zi value'ni move qilayotganini qaysi holatlarda alohida ko'rib chiqish kerak?
- `let`dagi destructuring va `match`dagi destructuring orasidagi semantic farq qayerda keskinlashadi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-destructuring]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[pattern-destructuring|pattern destructuring]]
- [[array-destructuring|array destructuring]]
- [[slice-patterns|slice patterns]]
- [[discarded-binding]]
