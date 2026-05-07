---
title: "Test Filtering"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, cargo-test, filtering]
source_count: 1
---

# Test Filtering

## Short Definition

Test filtrlash — `cargo test <pattern>` buyrug'i orqali faqat nomi berilgan pattern bilan mos keladigan testlarni ishlatish. Modul nomi ham qidiriladi.

## Why It Matters

Test to'plami katta bo'lganda barcha testlarni ishlatish vaqt oladi. Muayyan bir funksiyaga tegishli testlarni tezda qayta ishlatish uchun filtrlash ishlatiladi.

## Syntax and Examples

### Bitta test nomi

```shell
cargo test one_hundred
```

```
running 1 test
test tests::one_hundred ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

`2 filtered out` — ikki test mos kelmadi.

### Substring bilan bir nechta test

```shell
cargo test add
```

Nomi `add` substringini o'z ichiga olgan barcha testlar ishlaydi:

```
running 2 tests
test tests::add_three_and_two ... ok
test tests::add_two_and_two ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out
```

### Modul nomi bo'yicha filtrlash

Har bir test `module::function_name` formatida to'liq nomga ega. Shuning uchun modul nomini filtrlash shu modul ichidagi barcha testlarni ishlatadi:

```shell
cargo test tests::   # tests module'dagi barcha testlar
cargo test unit_     # nomi "unit_" bilan boshlangan barcha testlar
```

## Common Mistakes

- Bir vaqtda bir nechta aniq nom berish mumkin emas — faqat bitta argument yoki substring qabul qilinadi.
- Substringlar case-sensitive — `Add` va `add` boshqa narsalar.

## Related Concepts

- [[testing]]
- [[cfg-test]]

## Sources

- [[11-2-controlling-how-tests-are-run-the-rust-programming-language]]
