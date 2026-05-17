---
title: "Testing"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-17
tags: [rust, testing, correctness]
source_count: 6
---

# Testing

## Short Definition

Rust testlari — non-test kodning kutilgan tarzda ishlashini tekshiruvchi `#[test]` bilan belgilangan funksiyalar. `cargo test` ularni yig'ib, ishlatib, natijalarni chiqaradi. Rust community testlarni ikki kategoriyaga ajratadi: [[unit-tests]] va [[integration-tests]].

## Why It Matters

Rust type system va borrow checker katta xato sinfini compile time'da ushlaydi, lekin mantiqiy xatolarni ushlay olmaydi. `add_two(3)` funksiyasining `5` qaytarishini compiler tekshira olmaydi — bu ish testlarniki. Dijkstra aytganidek, testlar buglarning borligini ko'rsatadi, lekin yo'qligini isbotlamaydi; shunga qaramay maksimal test yozish kerak.

## Mental Model

Har bir test mustaqil thread'da ishlaydi. Agar test funksiya panic bilan tugasa — FAILED. Agar normal tugasa — ok. Shu sababli Rustda test yozishning asosiy mexanizmi assert macro'lar va `panic!` atrofida qurilgan.

Test body uchta bosqich:
1. Kerakli data yoki holatni tayyorlash.
2. Test qilinadigan kodni ishlatish.
3. Natijani assert qilish.

Back-end service code'ida mock ko'pincha alohida framework bilan emas, trait contract'ni test-specific type orqali implement qilish bilan quriladi.

## Syntax and Examples

### Asosiy test tuzilishi

```rust
#[cfg(test)]
mod tests {
    use super::*;   // tashqi modul itemlarini ichki scopega olib kirish

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

### `cargo test` output

```
running 2 tests
test tests::larger_can_hold_smaller ... ok
test tests::smaller_cannot_hold_larger ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Statistika:
- `passed` / `failed` — o'tgan / o'tmagan
- `ignored` — `#[ignore]` bilan o'tkazib yuborilgan
- `measured` — benchmark testlar (faqat nightly)
- `filtered out` — nom filtridan o'tmagan

### Unit tests va Integration tests farqi

| | Unit tests | Integration tests |
|---|---|---|
| Joyi | `src/` — kod bilan | `tests/` — loyiha ildizida |
| Interface | Private + public | Faqat public API |
| Import | `use super::*` | `use crate_name::fn` |
| `#[cfg(test)]` | Kerak | Kerak emas |

### Parallel va ketma-ket ishlatish

Default: testlar parallel thread'larda ishlaydi. Shared state muammosi bo'lsa:

```shell
cargo test -- --test-threads=1   # bitta thread, o'zaro xalaqitsiz
```

### Output ushlab qolish

O'tgan testlarning `println!` outputi default'da ko'rinmaydi. Ko'rish uchun:

```shell
cargo test -- --show-output
```

### Test filtrlash

```shell
cargo test one_hundred   # faqat shu nomli test
cargo test add           # "add" substringini o'z ichiga olgan barcha testlar
```

### `#[ignore]` — og'ir testlarni o'tkazib yuborish

```rust
#[test]
#[ignore]
fn expensive_test() { /* bir soat ishlaydi */ }
```

```shell
cargo test             # expensive_test ... ignored
cargo test -- --ignored          # faqat ignored testlar
cargo test -- --include-ignored  # barcha testlar, ignored ham
```

### `Result<T, E>` qaytaruvchi test

```rust
#[test]
fn it_works() -> Result<(), String> {
    let result = add(2, 2);
    if result == 4 {
        Ok(())
    } else {
        Err(String::from("two plus two does not equal four"))
    }
}
```

`?` operator ishlatish imkonini beradi. `#[should_panic]` bilan mos kelmaydi.

### Release channels bilan testing

Stable Rust default target bo'lsa ham, ecosystem odatda beta va nightlyni ham kuzatadi. Library yoki tool yaratganda CI'da beta channelni qo'shib qo'yish upcoming stable regressiyalarni oldinroq ko'rishga yordam beradi.

## Common Mistakes

- `use super::*;` yozmaslik — test module ichidan tashqi modul itemlari ko'rinmaydi.
- `#[cfg(test)]` yozmaslik — test kodi release binary'ga ham kirib qoladi.
- `Result<T, E>` returning testda `#[should_panic]` ishlatishga urinish — bu ishlaydi, lekin to'g'ri emas; o'rniga `assert!(value.is_err())` ishlatiladi.
- Test thread'lari parallel ishlashini hisobga olmaslik — shared state (bir xil fayl, env var) testlarda muammo yaratadi; `--test-threads=1` yechim.
- `#[ignore]`li testni faqat `cargo test`da ishlatmoqchi bo'lish — ular o'tkazib yuboriladi; `-- --ignored` kerak.

## Related Concepts

- [[test-macros]] — `assert!`, `assert_eq!`, `assert_ne!`
- [[should-panic]] — `#[should_panic]` attribute
- [[cfg-test]] — `#[cfg(test)]` va `mod tests`
- [[panic]] — test failure mexanizmi
- [[result|Result]] — `Result<T, E>` testlarda
- [[test-filtering]] — test nomini yoki substringni argument sifatida berish
- [[unit-tests]] — `src/` ichida, private access
- [[integration-tests]] — `tests/` papkasida, public API
- [[dev-dependencies]]
- [[cargo-nextest]]
- [[correctness]]
- [[release-channels]]

## Sources

- [[wiki/sources/11-writing-automated-tests]]
- [[11-1-how-to-write-tests]]
- [[11-2-controlling-how-tests-are-run]]
- [[11-3-test-organization]]
- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7. G - How Rust is Made and Nightly Rust]]
- [[wiki/sources/rust-for-backend-developers-testing]]
