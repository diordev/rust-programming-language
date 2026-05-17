---
title: "Generic lifetime qanday ishlatiladi?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, borrowing, lifetimes, generics]
source_count: 2
---

# Generic lifetime qanday ishlatiladi?

## Answer

`Generic lifetime` deganda odatda `<'a>` kabi lifetime parameter nazarda tutiladi. Bu type genericlariga o‘xshaydi, lekin type emas, **reference qancha vaqt valid bo‘lishini** belgilaydi.

Oddiy model:

- `generics` - `T`, `U`, `K`, `V` kabi type parameterlar
- lifetimes - `\'a`, `\'b` kabi reference validity parameterlar

Masalan, function ichida ikki reference bor va return qilinayotgan reference ulardan biriga bog‘liq bo‘lsa, lifetime yoziladi:

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

Bu yerda `<'a>` compilerga shuni aytadi:

- `x` reference’ning lifetime’i `a`
- `y` reference’ning lifetime’i ham `a`
- return qilinadigan reference ham shu `a` ichida valid bo‘lishi kerak

Bu “ikkalasi ham bir xil muddat yashashi kerak” degani emas. Yaxshi intuitiv tushuncha shuki: return reference qaysi input reference’ga bog‘liq bo‘lsa, compiler shu bog‘lanishni lifetime orqali ko‘radi.

Struct ichida reference field saqlashda ham lifetime kerak:

```rust
struct User<'a> {
    username: &'a str,
}
```

Bu `User` ichidagi `username` field borrowed data ekanini bildiradi. `User<'a>` shuni anglatadi: bu struct ichidagi reference, `a` lifetime davomida valid bo‘lishi kerak.

Methodlarda ham lifetime ishlatilishi mumkin:

```rust
impl<'a> User<'a> {
    fn username(&self) -> &'a str {
        self.username
    }
}
```

Hamma joyda lifetime yozish shart emas. Rust ko‘p oddiy holatlarda lifetime elision qoidalari bilan `&self`, `&str` kabi signallarni o‘zi tushunadi. Lekin qayerda reference qaytayotgani yoki struct ichida reference saqlanayotgani noaniq bo‘lsa, `<'a>` kerak bo‘ladi.

Yana bir muhim narsa: lifetime genericga o‘xshasa ham, u value yaratmaydi va memory allocation qilmaydi. U faqat relationship belgilaydi:

- bu reference qaysi owner’ga bog‘langan
- bu borrow qachongacha yashashi mumkin

Qisqa qoida:

- `T` - type uchun
- `\'a` - reference muddati uchun

## Evidence

- [[lifetimes]]: reference qancha vaqt valid bo‘lishini ifodalaydi.
- [[generics]]: type parameterlardan foydalanish.
- [[borrowing]]: ownershipsiz foydalanish.
- [[reference]]: `&value` syntax.
- [[4-2-references-and-borrowing]]: lifetime annotationlar va valid reference qoidalari.

## Follow-up

- `longest<'a>` misolini bir nechta variant bilan mashq qilish.
- Struct ichida `&str` field nega `String`dan murakkabroq ekanini ko‘rish.
- Lifetime elision qoidalarini alohida o‘rganish.

## Related Pages

- [[generics]]
- [[lifetimes]]
- [[borrowing]]
- [[reference]]
- [[ownership]]
- [[string-slice|String slice]]
- [[structs]]

