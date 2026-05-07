---
title: "13.3. Improving Our I/O Project"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, closures, iterators, minigrep, refactoring, clone, impl-trait]
source_count: 1
---

# 13.3. Improving Our I/O Project

## Source

- URL: https://doc.rust-lang.org/stable/book/ch13-03-improving-our-io-project.html
- Kitob: The Rust Programming Language
- Bob: 13.3

## Detailed Summary

Bu bo'lim 12-bobdagi `minigrep` loyihasiga qaytib, closures va iterators yordamida ikki joyni yaxshilaydi: `Config::build` va `search` funksiyasi.

### 1. `Config::build`: `clone()` dan xalos bo'lish

**Muammo:** 12-bobda `build(args: &[String])` faqat slice borrow qilgani uchun `String` qiymatlarni `clone()` qilishga majbur edi. Bu qo'shimcha heap allocation demak.

**Yechim:** Signature'ni iterator qabul qiladigan qilib o'zgartirish:

```rust
// ESKI — clone talab qiladi
fn build(args: &[String]) -> Result<Config, &'static str> {
    let query = args[1].clone();
    let file_path = args[2].clone();
    // ...
}

// YANGI — ownership orqali clone shart emas
fn build(
    mut args: impl Iterator<Item = String>,
) -> Result<Config, &'static str> {
    args.next(); // dastur nomini o'tkazib yubor

    let query = match args.next() {
        Some(arg) => arg,
        None => return Err("Didn't get a query string"),
    };

    let file_path = match args.next() {
        Some(arg) => arg,
        None => return Err("Didn't get a file path"),
    };
    // ...
}
```

**`main` dagi o'zgarish:**

```rust
// ESKI — iterator'ni Vec'ga yig'ib, keyin slice uzatish
let args: Vec<String> = env::args().collect();
let config = Config::build(&args)...;

// YANGI — iterator'ni to'g'ridan-to'g'ri uzatish
let config = Config::build(env::args())...;
```

`env::args()` o'zi iterator qaytaradi. Endi `Vec` alloc va `clone` ikkalasi ham yo'q.

**`impl Iterator<Item = String>`** — `impl Trait` sintaksisi (10-bob): `args` `Iterator<Item = String>` implement qilgan istalgan tur bo'lishi mumkin. `mut` kerak chunki `next()` ichki holatni o'zgartiradi.

**Error xabarlari** ham aniqroq bo'ldi: `"not enough arguments"` o'rniga `"Didn't get a query string"` / `"Didn't get a file path"` — nima yetishmayotgani ko'rinadi.

### 2. `search` funksiyasi: mutable Vec → iterator chain

```rust
// ESKI — mutable holatli
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();
    for line in contents.lines() {
        if line.contains(query) {
            results.push(line);
        }
    }
    results
}

// YANGI — functional style
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    contents
        .lines()
        .filter(|line| line.contains(query))
        .collect()
}
```

Afzalliklar:
- Mutable `results` vektori yo'q — kamroq mutable state
- Kod niyatini to'g'ridan-to'g'ri ifodalaydi: "filter qilib yig'ib ol"
- Kelajakda parallellashtirish osonroq (mutable shared state yo'q)

**Bonus pattern (mashq uchun):** `collect()` ni olib tashlab `impl Iterator<Item = &'a str>` qaytarish → `run()` ichidagi `for` loop har qator topilishi bilan chop etadi — to'liq lazy pipeline.

### Iterators vs Loops: qaysi biri yaxshi?

Ko'pchilik Rust dasturchilari iterator stilini afzal ko'radi: loop ichidagi mayda-chuyda o'rniga kodni yuqori darajadagi maqsadga qaratadi. Avval o'rganish qiyin, lekin `filter`, `map`, `collect` kabi adapter'lar o'rganilgach, kod aniqroq o'qiladi.

## Key Concepts

- [[iterators]] — `env::args()` iterator qaytaradi, to'g'ridan-to'g'ri uzatish mumkin
- [[consuming-adapters]] — `collect()` search natijasini yig'adi
- [[iterator-adapters]] — `filter()`, `lines()` search'ni ifodalaydi
- [[impl-trait]] — `impl Iterator<Item = String>` generic parameter sifatida
- [[clone]] — iterator ownership bilan `clone` kerak emas bo'ladi
- [[closures]] — `filter(|line| line.contains(query))` closure environment'dan capture qiladi
- [[lifetimes]] — `search<'a>` return tipi lifetimeni saqlab qoladi

## Code Examples

- [[minigrep-iterator-refactor]] — `Config::build` va `search` to'liq refactor

## Exercises or Practice Ideas

- `search_case_insensitive` funksiyasini ham iterator stiliga o'tkazing
- `search` dan `collect()` olib tashlab `impl Iterator<Item = &'a str>` qaytaring va testlarni yangilang
- `Config::build` uchun `args.next()` o'rniga destructuring pattern sinab ko'ring

## Questions Raised

- `impl Iterator<Item = T>` va `Box<dyn Iterator<Item = T>>` qaysi holda farq qiladi?
- `search` funksiyasida `lines()` qanday ishlaydi — u ham iterator-mi?

## Links To Update

- [[iterators]]
- [[impl-trait]]
- [[13-functional-language-features]] chapter
- [[minigrep-iterator-refactor]] example
