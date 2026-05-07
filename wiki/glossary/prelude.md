---
title: "prelude"
type: glossary
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, glossary, standard-library]
source_count: 1
---

# prelude

Rust standard library *prelude*'i — har bir Rust faylga avtomatik kiritiladigan eng keng ishlatiladigan typelar, traits, va functionlar to'plami. `Option`, `Some`, `None`, `Result`, `Ok`, `Err`, `String`, `Vec`, `Box`, `Drop`, `Clone`, `Copy`, va boshqalar prelude'da, shu sabab `use` qilish shart emas.

Prelude scope'ga kiritish (Chapter 7) qaramligi bilan bog'liq: agar custom `Option` define qilmoqchi bo'lsangiz, std prelude version'i bilan qarama-qarshi kelmaydi, chunki sizning version'ingiz local scope'da soya qiladi. Lekin bunday qilish ko'pincha noto'g'ri tanlov.

Related: [[standard-library|standard library]], [[option|Option]], [[result|Result]]

Sources: [[6-1-defining-an-enum-the-rust-programming-language]]
