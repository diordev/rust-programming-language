---
title: "Паника - Rust для back-end разработчиков"
type: source
status: active
created: 2026-05-17
updated: 2026-05-17
tags: [rust, backend, source, panic, advance]
source_count: 1
---

# Паника - Rust для back-end разработчиков

## Source

- Raw: `raw/books/rust-for-backend-developer/3. advance/51. Паника.md`
- URL: https://rust-for-backend-developers.github.io/dive-deeper/panic.html

## Detailed Summary

Bu source `[[panic|panic]]`ni oddiy "crash" hodisasi sifatida emas, balki runtime invariant break signalining kengroq toolset'i bilan ko'rsatadi: `panic!`, `catch_unwind`, `panic_any`, `set_hook`, `todo!`, `unimplemented!`, `unreachable!`.

Birinchi qism `unwrap()` va explicit `panic!()` misollari bilan boshlanadi. Bu yerda durable rule qat'iy: panic expected error handling emas. User input, file topilmasligi, network failure kabi normal failure path'lar uchun default yechim `[[result|Result]]`.

`catch_unwind` bo'limi panic'ni process level emas, closure boundary darajasida ushlash mumkinligini ko'rsatadi. Muhim tuzatish: imzodagi natija `Result<R, Box<dyn Any + Send + 'static>>`. Bu panic payload'ining `Any` ekaniga ishora qiladi, lekin bu mexanizm exception-style business error transporti uchun emas.

`panic_any` bo'limi custom payload yuborishni ko'rsatadi. `ProcessingErr` enum'ni panic orqali olib chiqib, keyin `downcast_ref::<ProcessingErr>()` bilan ajratish mumkin. Bu capability bor, lekin source o'zi ham to'g'ri ogohlantiradi: buni normal error handlingga aylantirish yomon dizayn.

`set_hook` bo'limi default panic output catch'dan oldin ishlashini ko'rsatadi. Demak `catch_unwind` panicni ushlasa ham, hook bo'lsa oldin log, telemetry, yoki custom formatting qilish mumkin.

`todo!`, `unimplemented!`, `unreachable!` bo'limlari `panic` family'ning yana bir amaliy qatlamini beradi. Ularning barchasi `[[never-type|!]]` qaytaradi, shuning uchun compiler kutilgan har qanday type o'rnida ularni qabul qiladi. `todo!` va `unimplemented!` placeholder, `unreachable!` esa "bu branchga kelinmasligi kerak" degan assertion.

Source ichidagi eng muhim policy takeaway: backend tizimlarda panic ba'zan request boundary ichida izolyatsiya qilinishi mumkin, lekin bu `Result`ni almashtirmaydi. Panic bug, contract break, impossible state, yoki processni davom ettirish xavfli bo'lgan holatlar uchun.

## Key Concepts

- [[panic]]
- [[panic-vs-result|panic! vs Result]]
- [[catch-unwind]]
- [[panic-any]]
- [[panic-hook]]
- [[unwrap]]
- [[todo-macro|todo!]]
- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]
- [[never-type|never type (!)]]
- [[diverging-functions|diverging functions]]
- [[backtrace]]

## Code Examples

```rust
let result = std::panic::catch_unwind(|| {
    panic!("My panic msg");
});
```

```rust
use std::panic::panic_any;

panic_any(MyError::NotAuthorized);
```

```rust
fn my_func() -> String {
    todo!("Don't forget to implement")
}
```

## Exercises or Practice Ideas

- `catch_unwind` bilan bitta closure'ni o'rab, `Err` branchda payload type'ni tekshiring.
- `todo!`, `unimplemented!`, va `unreachable!`ning qaysi development phase'da ishlatilishini yozing.
- `panic!` va `Result` orasidagi boundary'ni uchta real backend scenario bilan ajrating.

## Questions Raised

- Request boundary ichida panic isolation kerakmi yoki process fail-fast bo'lishi kerakmi?
- `catch_unwind` qaysi layerda ishlatilsa o'rinli, qaysi layerda ortiqcha?
- Placeholder macro'larni production branchda qoldirmaslikni qanday nazorat qilish kerak?

## Links To Update

- [[wiki/chapters/rust-for-backend-developers-3-advance]]
- [[wiki/chapters/rust-for-backend-developers-panic]]
- [[panic]]
- [[panic-vs-result|panic! vs Result]]
- [[catch-unwind]]
- [[panic-any]]
- [[panic-hook]]
- [[todo-macro|todo!]]
- [[unimplemented-macro|unimplemented!]]
- [[unreachable-macro|unreachable!]]

