---
title: "Скоупы - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-16
updated: 2026-05-16
tags: [rust, backend, source, scope]
source_count: 1
---

# Скоупы - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/2. base/11. Скоупы.md`
- URL: https://rust-for-backend-developers.github.io/rust-basics/scopes.html

## Detailed Summary

Bu source scope'ni ikkita parallel ma'noda ishlatadi: variable qachongacha yashashi va block expression qaysi value'ni qaytarishi.

Birinchi qism oddiy, lekin fundamental: variable o'zi e'lon qilingan curly-brace block ichida yashaydi va scope tugaganda avtomatik yo'q qilinadi. Bu ownership chapterlaridan oldingi sodda mental model.

Ikkinchi qism Rustning expression-oriented tabiati uchun muhimroq. Har bir block value qaytaradi; bu value block ichidagi oxirgi expression bo'ladi. Shuning uchun blockni variable'ga assign qilish mumkin:

```rust
let a = {
    let x = 1;
    let y = 2;
    x + y
};
```

Source semicolonning real rolini ham ochib beradi. Rustda `;` "expression tugadi" degani emas, balki expressionni statementga aylantirib, keyingi hidden `()` value'ga yo'l ochadi. Natijada block oxirgi expressiondan keyin `;` qo'yilsa, block value'si `()` bo'lib qoladi.

Bu keyingi function return, `if` expressions, `match`, va `let` bilan bog'liq ko'p syntaxni tushunish uchun tayanch bo'ladi.

## Key Concepts

- [[scope]]
- [[statements-and-expressions|statements and expressions]]
- [[unit-type|unit type]]
- [[drop]]
- [[return-values|return values]]

## Code Examples

```rust
{
    let a = 1;
    {
        let b = 2;
    }
}
```

```rust
let a: i32 = {
    let x = 1;
    let y = 2;
    x + y
};
```

```rust
let a: () = {
    let x = 1;
    let y = 2;
    x + y;
};
```

## Exercises or Practice Ideas

- Semicolon bor va yo'q ikki block yozib, types qanday farq qilishini ko'rsating.
- Ichki scope'dagi variable tashqarida ishlatilganda qanday compiler xatti-harakati bo'lishini tekshiring.
- `if` expression ham oxirgi expression modeli bilan ishlashini ko'rsatuvchi misol yozing.

## Questions Raised

- Block return value va function return value orasidagi aloqa qanchalik to'g'ridan-to'g'ri?
- Scope tugaganda cleanup har doim lexical orderning teskarisidami?
- `()`ga aylanishi mumkin bo'lgan blocklar type mismatch'ga qayerlarda olib keladi?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-scopes]]
- [[wiki/chapters/rust-for-backend-developers-2-base]]
- [[scope]]
- [[statements-and-expressions|statements and expressions]]
- [[unit-type|unit type]]
