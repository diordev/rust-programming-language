---
title: "Qisqa savol-javob: variable, ownership, borrow, reference, lifetimes"
type: question
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, question, variables, ownership, borrowing, reference, lifetimes]
source_count: 7
---

# Qisqa savol-javob: variable, ownership, borrow, reference, lifetimes

## Answer

### 1. Variable nima?

Variable — qiymatga berilgan nom. Rustda odatda `let` bilan yaratiladi.

```rust
let x = 5;
```

### 2. Ownership nima?

Ownership — qiymat kimniki ekanini va qachon `drop` bo'lishini belgilaydigan qoida.

```rust
let s1 = String::from("hi");
let s2 = s1;
// s1 endi ishlamaydi
```

### 3. Borrow nima?

Borrow — qiymatni ownershipsiz vaqtincha ishlatish.

```rust
let s = String::from("hi");
let len = uzunlik(&s);
```

### 4. Reference nima?

Reference — borrow qilish uchun ishlatiladigan ko'rinish: `&T` yoki `&mut T`.

```rust
let s = String::from("hi");
let r = &s;
```

### 5. Lifetimes nima?

Lifetime — reference qachongacha valid bo'lishini compiler tekshiradigan tushuncha.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

### Juda qisqa bog'lanish

- variable = nom
- ownership = egalik
- borrow = vaqtincha foydalanish
- reference = shu foydalanishning `&` ko'rinishi
- lifetime = reference xavfsiz yashash muddati

## Evidence

- [[variables-and-mutability|variables and mutability]]
- [[ownership]]
- [[borrowing]]
- [[reference]]
- [[lifetimes]]
- [[3-1-variables-and-mutability]]
- [[4-1-what-is-ownership]]
- [[4-2-references-and-borrowing]]
- [[10-3-validating-references-with-lifetimes]]

## Follow-up

- Ownership va borrow farqini kodda ajratib ko'rish.
- `&String` va `&str` farqini alohida mashq qilish.
- `E0382`, `E0499`, `E0502`, `E0597` xatolarini amalda ko'rish.

## Practice Check

2026-05-08 foydalanuvchi yechimlari bo'yicha qisqa review:

### 1. Borrow bilan uzunlik olish

To'g'ri.

```rust
fn len_data(v: &str) -> usize {
    v.len()
}
```

Bu yerda `&str` tanlangani yaxshi, chunki `&String`dan ko'ra flexible.

### 2. `&mut String` bilan o'zgartirish

To'g'ri.

```rust
fn add_char(v: &mut String) {
    v.push_str("!");
}
```

`-> ()` yozish shart emas. Lekin yechim mantiqan to'g'ri: owner `main`da qolyapti, function esa mutable borrow bilan value'ni o'zgartiryapti.

### 3. `longest<'a>` funksiyasi

To'g'ri.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

Bu yerda lifetime annotation return reference inputlardan biriga bog'langanini compilerga aniq ko'rsatadi.

### 4. `s1` nega ishlamay qoladi?

Mazmun to'g'ri.

Qisqa, aniq ifoda:

- `s1` avval owner
- `let s2 = s1;` dan keyin ownership `s2`ga move bo'ladi
- `s1` invalid bo'ladi
- keyin `println!("{s1}")` qilinsa [[e0382-borrow-of-moved-value|E0382]] chiqadi

## Related Pages

- [[ownership-borrowing-reference-va-lifetimes-boglanishi]]
- [[ozgaruvchilar-haqida]]
- [[ownership-haqida]]
- [[borrowing-nima-va-qanday-ishlaydi]]
- [[lifetimes-nima-va-nima-uchun-kerak]]
