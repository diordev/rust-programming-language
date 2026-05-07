---
title: "11.1. How to Write Tests"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, testing, assert, should-panic, cfg-test]
source_count: 1
---

# 11.1. How to Write Tests

## Source

- Kitob: *The Rust Programming Language*
- Bo'lim: Chapter 11.1
- URL: https://doc.rust-lang.org/stable/book/ch11-01-writing-tests.html
- Raw fayl: `raw/books/the_rust_programming_language/11.1. How to Write Tests - The Rust Programming Language.md`

## Detailed Summary

### Test funksiya tuzilishi

Test — non-test kodning kutilgan tarzda ishlashini tekshiruvchi Rust funksiya. Test body odatda uchta amalni bajaradi:

1. Kerakli data yoki holatni tayyorlash.
2. Test qilinadigan kodni ishlatish.
3. Natijalar kutilgandek ekanini assert qilish.

### `#[test]` attribute va `cargo test`

`#[test]` annotatsiyasi funksiyani test sifatida belgilaydi. `cargo test` buyrug'i test runner binary yaratib, annotated funksiyalarni ishlatadi va har bir test o'tgan yoki o'tmaganini bildiradi.

`cargo new adder --lib` yangi library project yaratganda `src/lib.rs` ichida avtomatik test module va test funksiya hosil qilinadi:

```rust
pub fn add(left: u64, right: u64) -> u64 {
    left + right
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

### `cargo test` output formati

```
running 1 test
test tests::it_works ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

   Doc-tests adder

running 0 tests
test result: ok. 0 passed; ...
```

Statistika:
- `passed` / `failed` — o'tgan / o'tmagan testlar
- `ignored` — `#[ignore]` bilan belgilangan, ishlatilmagan testlar
- `measured` — benchmark testlar (faqat nightly Rust)
- `filtered out` — nom filtridan o'tmagan testlar
- `Doc-tests` — API dokumentatsiyasidagi kod misollari (Chapter 14da tushuntiriladi)

Test thread'larda ishlaydi: har bir test yangi thread'da ishlaydi; thread panic bilan tugasa — test FAILED.

### `assert!` macro

Boolean ifodani tekshiradi. `true` bo'lsa — hech narsa bo'lmaydi. `false` bo'lsa — `panic!` chaqiriladi, test FAILED.

```rust
assert!(larger.can_hold(&smaller));  // true kutiladi
assert!(!smaller.can_hold(&larger)); // false kutiladi, shuning uchun !
```

`use super::*;` — `tests` ichki module tashqi modul itemlarini ko'rishi uchun kerak (glob import).

### `assert_eq!` va `assert_ne!`

Ikkita qiymatni tenglik yoki tengsizlik bo'yicha tekshiradi. Xato bo'lganda ikkala qiymatni debug formatida chiqaradi — bu `assert!`dan qulay, chunki qiymatlar ko'rinadi.

```rust
assert_eq!(result, 4);   // left == right bo'lsa o'tadi
assert_ne!(result, 10);  // left != right bo'lsa o'tadi
```

Xato output:
```
assertion `left == right` failed
  left: 5
 right: 4
```

**Muhim:** Rusтda argument tartibi `expected` va `actual` emas — `left` va `right`. Tartib farq qilmaydi, lekin konventsiya sifatida ko'pincha `assert_eq!(natija, kutilgan)` yoziladi.

**Talab:** Solishtiriladigan typlar `PartialEq` va `Debug` implement qilishi kerak. Custom struct/enum uchun `#[derive(PartialEq, Debug)]` yetarli.

### Custom failure messages

`assert!`, `assert_eq!`, `assert_ne!` macrolariga qo'shimcha argumentlar berilsa, ular `format!` macrosiga o'xshab formatlangan xabar chiqaradi:

```rust
assert!(
    result.contains("Carol"),
    "Greeting did not contain name, value was `{result}`"
);
```

Xato bo'lganda: `Greeting did not contain name, value was \`Hello!\``

Bu xato kontekstini aniqlashtiradi va debugging osonlashadi.

### `#[should_panic]`

Kod panic qilishi kerak bo'lgan holatlarni testlash uchun. Test funksiyasi panic qilsa — o'tadi. Panic bo'lmasa — FAILED.

```rust
#[test]
#[should_panic]
fn greater_than_100() {
    Guess::new(200);
}
```

`expected` parametri bilan aniqroq qilinadi — panic xabari shu substringni o'z ichiga olishi kerak:

```rust
#[test]
#[should_panic(expected = "less than or equal to 100")]
fn greater_than_100() {
    Guess::new(200);
}
```

Agar panic boshqa xabar bilan chiqsa:
```
note: panic did not contain expected string
      panic message: "Guess value must be greater than or equal to 1, got 200."
 expected substring: "less than or equal to 100"
```

### `Result<T, E>` testlarda

Testlar `Result<T, E>` qaytarishi mumkin. O'tish uchun `Ok(())`, xato uchun `Err(String)` qaytariladi. `?` operator ishlatish imkonini beradi.

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

**Cheklov:** `Result<T, E>` returning testlarda `#[should_panic]` ishlatib bo'lmaydi. `Err` variantni tekshirish uchun `assert!(value.is_err())` ishlatiladi.

## Key Concepts

- [[testing]] — test yozish mexanikasi
- [[cfg-test]] — `#[cfg(test)]` va `mod tests` pattern
- [[test-macros]] — `assert!`, `assert_eq!`, `assert_ne!`
- [[should-panic]] — `#[should_panic]` attribute
- [[result|Result]] testlarda ishlatish

## Code Examples

- **Adder library** — Listing 11-1, 11-2, 11-3, 11-4, 11-7
- **Rectangle can_hold** — Listing 11-5, 11-6
- **Greeting custom message** — custom assert failure message
- **Guess should_panic** — Listing 11-8, 11-9
- **Result-returning test** — `it_works() -> Result<(), String>`

## Exercises or Practice Ideas

- `add_two` funksiyasida qasddan bug kiritib `assert_eq!` xabarini kuzating.
- `can_hold` testini `assert_ne!` bilan qayta yozing.
- `Guess::new` uchun `should_panic(expected = ...)` bilan aniq xabar tekshiring.
- `Result<T, E>` qaytaradigan test yozing va `?` operatorini test ichida ishlatib ko'ring.

## Questions Raised

- `#[ignore]` attribute qanday ishlaydi? (11.2)
- Test filtrlash va nom bo'yicha ishlatish qanday? (11.2)
- Benchmark testlar nightly'da qanday yoziladi?
- Documentation tests qanday yoziladi? (14.2)

## Links To Update

- [[testing]] — to'liq yangilash
- [[index]] — yangi source qo'shish
- [[overview]] — Chapter 11 synthesis
