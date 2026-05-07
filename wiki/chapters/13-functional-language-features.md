---
title: "13. Functional Language Features: Iterators and Closures"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, closures, iterators, functional, fn-traits, performance, minigrep]
source_count: 5
---

# 13. Functional Language Features: Iterators and Closures

## Learning Goal

[[closures]] va [[iterators]] — tez, idiomatic Rust kod yozishning asosi. Bu bobdan keyin anonim funksiyalar bilan muhitdan qiymat capture qilish va ketma-ketliklarni lazy ravishda qayta ishlash usullarini bilish kerak.

## Main Ideas

- Rust functional programming'dan ilhomlangan: closures va iterators shu ta'sirning natijasi
- Closures muhitdan capture qila oladi — oddiy funksiyalar qila olmaydi
- Iteratorlar lazy: consume qilmagunicha hech narsa bajarmaydi
- Ikkalasi birgalikda kuchli, o'qilishi oson, tez kod yozishga imkon beradi
- Iterators for-loop bilan teng tezlikda ishlaydi — [[zero-cost-abstractions]] isbotlangan

## Concepts

### 13.1 Closures

- [[closures]] — `|params| body` sintaksisi bilan anonim funksiya; o'zgaruvchida saqlanadi yoki argument sifatida uzatiladi
- **Capture modlari** — closure body'ga qarab compiler eng kam access tanlab oladi:
  - Immutable borrow: faqat o'qish
  - Mutable borrow: o'zgartirish
  - `move`: ownership o'tkazish (thread'larda zarur)
- **Type inference** — closure type annotatsiyasi shart emas; birinchi chaqiruvda tip qotadi
- [[fn-traits]] — closure'ning qanday chaqirilishi mumkinligini ifodalovchi trait hierarchy:
  - `FnOnce` — bir marta (captured qiymatni move qilsa)
  - `FnMut` — bir necha marta, mutatsiya bo'lishi mumkin
  - `Fn` — bir necha marta, concurrent xavfsiz

### 13.2 Iterators

- [[iterators]] — `Iterator` trait + `next()` metodi; lazy pattern
- Uch xil yaratish: `.iter()` (`&T`), `.into_iter()` (`T`), `.iter_mut()` (`&mut T`)
- [[consuming-adapters]] — `sum()`, `collect()`: iterator'ni tugata ishlatadi
- [[iterator-adapters]] — `map()`, `filter()`: yangi lazy iterator qaytaradi, oxirida consume kerak
- Chaining: `v.iter().filter(...).map(...).collect()`

### 13.3 Improving Our I/O Project

- `Config::build` signature'i `&[String]` → `impl Iterator<Item = String>` ga o'zgartirildi
- `env::args()` iterator to'g'ridan-to'g'ri uzatiladi — oraliq `Vec<String>` va `clone()` yo'q
- `args.next()` + `match Some/None` — index'dan ko'ra xavfsizroq va aniqroq
- `search` funksiyasi: mutable `results` Vec + for loop → `.lines().filter(...).collect()`
- Kamroq mutable state → kelajakda parallellashtirish osonroq

### 13.4 Performance: Loops vs Iterators

- Benchmark: for loop ≈ iterator (19.6ms vs 19.2ms — deyarli teng)
- Compiler iterator chain'ni loop unrolling va bounds check elimination bilan optimizatsiya qiladi
- [[zero-cost-abstractions]]: "What you use, you couldn't hand code any better" — Stroustrup

## Examples

```rust
// Closure — environment'dan capture
let store = Inventory { shirts: vec![Blue, Red, Blue] };
store.giveaway(None); // closure: || self.most_stocked()

// Iterator adapter chain
let v2: Vec<_> = vec![1, 2, 3].iter().map(|x| x + 1).collect();
// v2 == [2, 3, 4]

// filter + closure capture
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}

// 13.3: minigrep search — iterator style
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    contents.lines().filter(|line| line.contains(query)).collect()
}

// 13.3: Config::build — iterator ownership, clone yo'q
fn build(mut args: impl Iterator<Item = String>) -> Result<Config, &'static str> {
    args.next(); // dastur nomini o'tkazib yuborish
    let query = match args.next() {
        Some(arg) => arg,
        None => return Err("Didn't get a query string"),
    };
    // ...
}
```

## Exercises

1. `FnOnce`, `FnMut`, `Fn` qabul qiluvchi uchta alohida funksiya yozing
2. `move` closure bilan thread spawning qiling
3. Custom struct uchun `Iterator` trait implement qiling
4. `filter` + `map` + `fold` chain yozing, for-loop ekvivalenti bilan solishtiring
5. `search_case_insensitive` funksiyasini ham iterator stiliga o'tkazing
6. `search` dan `collect()` olib tashlab `impl Iterator<Item = &'a str>` qaytaring

## Review Questions

1. Closure funksiyadan qaysi jihatdan farq qiladi?
2. `FnOnce` closure nima uchun `sort_by_key`'da ishlatib bo'lmaydi?
3. `iter()`, `into_iter()`, `iter_mut()` orasidagi farq nima?
4. Nima uchun iterator adapters (`map`, `filter`) yolg'iz qo'yilsa warning chiqadi?
5. `move` keyword qachon zarur va nima qiladi?
6. `Config::build` da nima uchun `clone()` kerak edi va iterator bilan qanday olib tashlandi?
7. Iterator va for-loop tezligi o'rtasida qanday farq bor?

## Related Pages

- [[13-functional-language-features-the-rust-programming-language|Source: 13. Intro]]
- [[13-1-closures-the-rust-programming-language|Source: 13.1 Closures]]
- [[13-2-processing-a-series-of-items-with-iterators-the-rust-programming-language|Source: 13.2 Iterators]]
- [[13-3-improving-our-io-project-the-rust-programming-language|Source: 13.3 Improving I/O Project]]
- [[13-4-performance-in-loops-vs-iterators-the-rust-programming-language|Source: 13.4 Performance]]
- [[minigrep-iterator-refactor|Example: minigrep iterator refactor]]
- [[closures]]
- [[fn-traits]]
- [[iterators]]
- [[consuming-adapters]]
- [[iterator-adapters]]
- [[zero-cost-abstractions]]
- [[traits]]
- [[borrowing]]
- [[move-semantics]]
