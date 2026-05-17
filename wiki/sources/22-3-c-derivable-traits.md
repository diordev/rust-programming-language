---
title: "22.3. C - Derivable Traits"
type: source
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, traits, derive, reference]
source_count: 1
---

# 22.3. C - Derivable Traits

## Source

- File: `raw/books/the_rust_programming_language/22.3. C - Derivable Traits.md`
- URL: <https://doc.rust-lang.org/stable/book/appendix-03-derivable-traits.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Appendix C `#[derive(...)]` bilan standard library'dan qaysi traitlar mechanical tarzda implement qilinishi mumkinligini jamlaydi. Bu narrative dars emas, balki "derive qilsam nima olaman, qachon mumkin, qachon yetarli emas?" degan savollarga reference javobidir.

Bo'lim avval `derive` nima qilishini aniq aytadi: compiler type ustiga default behavior'li trait impl generatsiya qiladi. Agar sizga boshqa xulq kerak bo'lsa, trait'ni qo'lda implement qilish kerak. Muhim chegaralardan biri ham shu: standard library'dagi hamma trait derivable emas.

### Nega hamma trait derive bo'lmaydi?

Appendix C aynan `Display` misolini beradi. `Display` end user ko'radigan formatni anglatadi, shuning uchun "to'g'ri default"ni compiler bilolmaydi. Qaysi fieldni ko'rsatish, qaysi format inson uchun qulayligi domenning o'ziga bog'liq. Shu sabab `Display` kabi traitlar mechanical derive uchun mos emas.

### Derivable traitlar ro'yxati

Bo'lim standard library'dan derive qilinadigan traitlarni sanaydi:

- [[debug-trait|Debug]]
- [[partial-eq|PartialEq]]
- [[eq-trait|Eq]]
- [[partial-ord|PartialOrd]]
- [[ord-trait|Ord]]
- [[clone]]
- [[copy-trait|Copy]]
- [[hash-trait|Hash]]
- [[default-trait|Default]]

Appendix yana shuni ham aytadi: bu list faqat standard library traitlari uchun. Kutubxonalar procedural macro orqali o'z traitlari uchun ham derive taqdim qilishi mumkin.

### Debug

`Debug` developer-facing output beradi. `{:?}` va `{:#?}` formatting shunga tayanadi. Appendix C ayniqsa `assert_eq!` bilan bog'liq muhim detailni eslatadi: assertion fail bo'lsa macro qiymatlarni chiqarishi kerak, shu sabab `Debug` talab qilinadi.

### PartialEq va Eq

`PartialEq`:

- `==` va `!=` operatorlarini yoqadi;
- derive bo'lganda struct uchun barcha fieldlar teng bo'lsa value teng, enum uchun variantlar o'z-o'ziga teng;
- `assert_eq!` uchun kerak.

`Eq`:

- method qo'shmaydi;
- type'dagi har qiymat o'ziga teng ekanini signal qiladi;
- `PartialEq` ustiga quriladi;
- float `NaN` kabi holatlar sabab barcha `PartialEq` typelar `Eq` bo'la olmaydi.

Appendix `HashMap<K, V>` keylari uchun `Eq` zarurligini amaliy misol sifatida beradi.

### PartialOrd va Ord

`PartialOrd`:

- `<`, `>`, `<=`, `>=` comparisonlarini beradi;
- `partial_cmp` -> `Option<Ordering>`;
- `NaN` tufayli ba'zan ordering yo'q bo'lishi mumkin;
- derive bo'lganda struct field orderi va enum variant declaration orderiga qarab comparison qiladi.

`Ord`:

- total ordering signal beradi;
- `cmp` -> `Ordering`;
- `PartialOrd` + `Eq` (va shu orqali `PartialEq`) talab qiladi;
- `BTreeSet<T>` kabi tartibga tayanadigan collectionlar uchun kerak.

### Clone va Copy

`Clone`:

- explicit duplication beradi;
- derive bo'lganda har field ustida `clone()` chaqiradi;
- shu sabab barcha fieldlar `Clone` bo'lishi kerak;
- slice'dagi `to_vec()` kabi API'lar uchun kerak bo'lishi mumkin.

`Copy`:

- trivial bitwise duplication signalini beradi;
- methodlari yo'q;
- barcha qismlari `Copy` bo'lgan type'largagina derive qilinadi;
- `Copy` bo'lgan type albatta `Clone` ham bo'lishi kerak.

Appendix C muhim mental modelni takrorlaydi: `Copy` odatda requirement sifatida kam so'raladi, lekin bo'lsa `clone()` yozish zaruratini kamaytiradi.

### Hash

`Hash` arbitrary sized value'ni fixed-size hash state'ga feed qilish imkonini beradi. Derive bo'lganda type qismlarining `hash` natijalari kombinatsiya qilinadi. Demak barcha fieldlar ham `Hash` bo'lishi kerak. Asosiy amaliy use case: `HashMap<K, V>` keylari.

### Default

`Default` type uchun default value beradi. Derive bo'lganda har fieldning `default()`i chaqiriladi, demak fieldlar ham `Default` bo'lishi kerak. Appendix ayniqsa ikki practical joyni ko'rsatadi:

- `..Default::default()` bilan struct update syntax;
- `Option<T>::unwrap_or_default()`.

### Yakuniy takeaway

Appendix C derive'ni "compiler spell" emas, balki aniq contract sifatida ko'rsatadi: faqat meaningful standard default behavior bor traitlargina mechanical derive qilinadi. `Debug`, `PartialEq`, `Default` kabi traitlar uchun bu juda qulay; `Display` kabi user-facing traitlar esa qo'lda qaror talab qiladi.

## Key Concepts

- [[derive-attribute]]
- [[debug-trait]]
- [[partial-eq]]
- [[eq-trait]]
- [[partial-ord]]
- [[ord-trait]]
- [[clone]]
- [[copy-trait]]
- [[hash-trait]]
- [[default-trait]]

## Code Examples

- [[derive-debug-partialeq-assert-eq|derive Debug + PartialEq for assert_eq!]]

## Exercises or Practice Ideas

1. Nega `Display` derive qilinmasligini o'z so'zingiz bilan yozing.
2. `PartialEq` va `Eq` o'rtasidagi farqni `NaN` misoli bilan tushuntiring.
3. `PartialOrd` va `Ord` o'rtasidagi farqni `Option<Ordering>` va `Ordering` nuqtai nazaridan yozing.
4. Nega `Copy` derive bo'lishi uchun type `Clone` ham bo'lishi kerakligini izohlang.
5. `Default` derive va `..Default::default()` patterni bilan kichik struct yarating.

## Questions Raised

- Standard library derivable traitlar nega aynan shu to'plam bilan cheklanadi?
- Qaysi custom traitlar procedural macro derive uchun eng tabiiy nomzod?
- User-facing formatting va programmer-facing formatting orasidagi chegarani qanday tanlash kerak?

## Links To Update

- [[index|Rust Wiki Index]]
- [[overview]]
- [[log]]
- [[wiki/chapters/22-3-c-derivable-traits|Chapter 22.3]]
- [[derive-attribute]]
- [[partial-eq]]
- [[eq-trait]]
- [[ord-trait]]
- [[hash-trait]]
- [[default-trait]]
