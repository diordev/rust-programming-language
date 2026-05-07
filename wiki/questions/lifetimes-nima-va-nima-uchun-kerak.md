---
title: "Lifetimes nima va nima uchun kerak?"
type: question
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, question, borrowing, lifetimes]
source_count: 2
---

# Lifetimes nima va nima uchun kerak?

## Answer

Lifetimes — bu reference qancha vaqt yashashini bildiradigan tushuncha.

Maktabcha misol bilan:

- `ownership` = bu daftar kimniki?
- `borrowing` = do‘stingdan daftarning bir sahifasini vaqtincha olish
- `reference` = qo‘lingdagi o‘sha sahifa
- `lifetime` = o‘sha sahifani ushlab turish uchun ajratilgan vaqt

Rustda reference owner emas. Reference faqat mavjud value'ga qaraydi. Shuning uchun Rust juda muhim savolni tekshiradi:

“Bu reference qarayotgan value reference yo‘qolguncha hali tirik bo‘ladimi?”

Agar javob yo‘q bo‘lsa, compiler code'ni rad qiladi. Sababi: dangling reference bo‘lishi mumkin.

Oddiy misol:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

Bu code xavfsiz, chunki function owned `String` qaytaradi. Ammo mana bu xavfli bo‘ladi:

```rust
fn bad() -> &String {
    let s = String::from("hello");
    &s
}
```

Bu ishlamaydi, chunki `s` function tugashi bilan yo‘q bo‘ladi, lekin qaytgan reference hali unga qarab turadi. Bu dangling reference bo‘ladi.

Shuning uchun lifetime’larning asosiy vazifasi shuki:

- reference qachongacha valid ekanini ko‘rsatish
- compilerga “bu reference qaysi owner’ga bog‘langan”ligini tushuntirish
- dangling reference’ni compile time’da to‘xtatish

Hozir boshlovchi bosqichda lifetime annotationlarning o‘zidan ko‘ra, g‘oyasini tushunish muhimroq:

1. Har bir value’ning owner’i bor.
2. Borrowing ownershipni olmaydi.
3. Reference faqat owner tirik bo‘lsa yashaydi.
4. Lifetime shu “tirik turish muddati”ni ifodalaydi.

Bu bog‘lanish ayniqsa function return va struct field reference’larda muhim. Masalan, struct ichida `&str` saqlamoqchi bo‘lsangiz, compiler reference qaysi data’ga tegishli ekanini lifetime orqali bilishi kerak.

Qisqa xulosa:

- `ownership` qiymatni kim saqlashini belgilaydi
- `borrowing` vaqtincha foydalanishni beradi
- `reference` shu borrowing’ning o‘zi
- `lifetime` reference qancha vaqt xavfsiz bo‘lishini bildiradi

## Evidence

- [[lifetimes]]: reference qancha vaqt valid bo'lishini ifodalaydi.
- [[borrowing]]: ownershipsiz foydalanish.
- [[reference]]: `&value` syntax va owner emasligi.
- [[ownership]]: owner scope'dan chiqganda value dropped bo'ladi.
- [[4-2-references-and-borrowing-the-rust-programming-language]]: dangling reference va valid reference qoidalari.
- [[e0106-missing-lifetime-specifier]]: return reference ortida valid owner bo'lmagan holat.

## Follow-up

- Lifetime annotation (`'a`) ni alohida, juda sodda misollar bilan ko'rish.
- `&str` bilan `String` orasidagi lifetime farqini ko'rish.
- Struct ichida reference field nega lifetime talab qilishini tekshirish.

## Related Pages

- [[lifetimes]]
- [[borrowing]]
- [[reference]]
- [[ownership]]
- [[dangling-reference|dangling reference]]
- [[e0106-missing-lifetime-specifier]]
- [[string-slice|String slice]]
- [[structs]]

