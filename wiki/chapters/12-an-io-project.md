---
title: "12. An I/O Project: Building a Command Line Program"
type: chapter
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, io, project, minigrep]
source_count: 7
---

# 12. An I/O Project: Building a Command Line Program

## Learning Goal

Ilgari o'rganilgan bilimlarni — kod tashkiloti, collections, error handling, traits, lifetimes, testing — bitta real loyiha orqali mustahkamlash. `minigrep` nomli `grep` versiyasini qurish.

## Main Ideas

### Loyiha: `minigrep`

`minigrep` — `grep`ning soddalashtrilgan versiyasi. U quyidagilarni bajaradi:

1. Command line argumentlarini qabul qiladi: `query` va `file_path`
2. Berilgan faylni o'qiydi
3. Fayl ichida `query`ni qidiradi
4. Mos satrlarni chiqaradi

```shell
$ cargo run -- the poem.txt
```

### 12.1: Command Line Argumentlar

`std::env::args()` orqali argumentlar o'qiladi:

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    let query = &args[1];
    let file_path = &args[2];
}
```

- `args[0]` — binary nomi
- `args[1]` — birinchi argument (`query`)
- `args[2]` — ikkinchi argument (`file_path`)
- `collect()` uchun `Vec<String>` type annotation kerak

### 12.2: Faylni O'qish

`std::fs::read_to_string` orqali fayl to'liq o'qiladi:

```rust
use std::fs;

let contents = fs::read_to_string(file_path)
    .expect("Should have been able to read the file");
```

`Result<String>` qaytaradi — `.expect()` bilan hozircha xatolarni boshqaradi.

### 12.3: Refactoring — Modularity va Error Handling

To'rt muammo hal qilindi: `main`da ko'p responsibility, tarqoq konfiguratsiya, noaniq xato xabarlari, xato kodi tarqoqlik.

**Refactoring qadamlari:**

1. `parse_config()` funksiyasi ajratildi
2. `Config` struct — `query` va `file_path` birlashtirildi
3. `Config::new` → `Config::build` (muvaffaqiyatsiz bo'lishi mumkin bo'lganda `build` nomi)
4. `Config::build` `Result<Config, &'static str>` qaytaradi
5. `main` da `unwrap_or_else` + `process::exit(1)`
6. `run(config)` funksiyasi — barcha logika
7. `run` → `Result<(), Box<dyn Error>>` + `?` operatori
8. `main` da `if let Err(e) = run(config)`
9. `src/lib.rs` + `src/main.rs` ajratish

**Yakuniy arxitektura:**

```
src/main.rs → args o'qish, Config::build, run chaqirish, xato ishlash
src/lib.rs  → Config, Config::build, run, search (keyingi bo'limlarda)
```

### 12.4: TDD bilan `search` funksiyasini qurish

TDD jarayoni: muvaffaqiyatsiz test → minimal kod → test o'tadi → refactor.

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    let mut results = Vec::new();
    for line in contents.lines() {
        if line.contains(query) {
            results.push(line);
        }
    }
    results
}
```

`'a` — `contents` va return value bog'liq, `query` emas. Sabab: qaytariladigan slices fayl mazmunidan keladi.

### 12.5: Environment Variable bilan Case-Insensitive Qidiruv

`IGNORE_CASE` environment variable orqali case-insensitive rejim yoqiladi. Foydalanuvchi uni bir marta o'rnatadi — terminal sessiyasida barcha qidiruvlar katta-kichik harfni hisobga olmaydi.

```rust
pub fn search_case_insensitive<'a>(
    query: &str,
    contents: &'a str,
) -> Vec<&'a str> {
    let query = query.to_lowercase();   // String, &str emas
    let mut results = Vec::new();

    for line in contents.lines() {
        if line.to_lowercase().contains(&query) {
            results.push(line);
        }
    }
    results
}
```

`Config` structiga `ignore_case: bool` maydoni qo'shildi:

```rust
let ignore_case = env::var("IGNORE_CASE").is_ok();
```

`is_ok()` — qiymatning o'zi emas, shunchaki o'rnatilganmi-yo'qmi muhim.

```shell
$ IGNORE_CASE=1 cargo run -- to poem.txt
```

### 12.6: Xatolarni stderr ga Yo'naltirish

`println!` faqat `stdout`ga yozadi. Xato xabarlari `stderr`ga yuborilishi kerak — Unix konventsiyasi.

```rust
// Oldin
println!("Problem parsing arguments: {err}");

// Keyin
eprintln!("Problem parsing arguments: {err}");
eprintln!("Application error: {e}");
```

Natija: `> output.txt` bilan ishlaganda xato xabarlari ekranda ko'rinadi, muvaffaqiyatli chiqish faylga tushadi.

## Concepts

- [[env-args|std::env::args]] — command line argumentlarni o'qish
- [[collect|collect()]] — iteratordan collection yasash
- [[type-annotations|type annotations]] — `collect()` uchun kerak
- [[fs-read-to-string|fs::read_to_string]] — faylni o'qish
- [[result|Result]] — xato qaytaruvchi turlar
- [[expect|expect()]] — tez xato ishlash
- [[separation-of-concerns|separation of concerns]] — `main`ni refactor qilish
- [[constructor-functions|Config::build naming convention]] — fallible constructors
- [[unwrap-or-else|unwrap_or_else]] — xatoni closure bilan ishlash
- [[process-exit|process::exit]] — toza dastur to'xtatish
- [[box-dyn-error|Box<dyn Error>]] — dinamik xato turi
- [[question-mark-operator|? operatori]] — xatoni yuqoriga uzatish
- [[if-let|if let Err(e)]] — xato bo'lsa ishlash
- [[static-lifetime|'static lifetime]] — string literal xato xabarlari
- [[library-crate|library crate]] — `src/lib.rs` ajratish
- [[tdd|Test-Driven Development]] — search funksiyasini qurish
- [[env-var|env::var]] — environment variable o'qish
- [[eprintln-macro|eprintln!]] — stderr ga chiqarish
- [[stderr]] — xato chiqish oqimi
- [[stdout]] — oddiy chiqish oqimi
- [[shadowing]] — `let query = query.to_lowercase()`

## Examples

- [[minigrep-args|minigrep args example]] (12.1)
- [[minigrep-read-file|minigrep read file example]] (12.2)

## Exercises

- `Config::build` ni o'zingiz yozing: kamida 3 xil xato holati uchun aniq xabar bering
- `poem.txt` yaratib `minigrep`ni sinab ko'ring
- `main` funksiyasini refactor qilib `src/lib.rs` ga ajrating
- `process::exit(1)` o'rniga `main -> Result<(), Box<dyn Error>>` bilan yozing
- CLI argument VA environment variable orqali case sensitivity boshqaring; qaysi biri ustuvor bo'lishini hal qiling
- Barcha xato xabarlarini `eprintln!`ga o'tkazing va `> out.txt 2> err.txt` bilan farqni kuzating

## Review Questions

1. `env::args()` nima qaytaradi va `collect()` bilan qanday ishlaydi?
2. Nima uchun `collect()` uchun type annotation kerak?
3. `args[0]` nima va nima uchun arguments `args[1]`dan boshlanadi?
4. `fs::read_to_string` qanday xatolarni yuzaga keltirishi mumkin?
5. Nima uchun `Config::new` emas, `Config::build` ishlatildi?
6. `unwrap_or_else` va `if let Err(e)` qanday farq qiladi?
7. `Box<dyn Error>` nima va nima uchun ishlatiladi?
8. `src/lib.rs` ga ajratish nima afzallik beradi?
9. `search_case_insensitive`da nima uchun `query` `&str` emas, `String` bo'ladi?
10. `env::var("X").is_ok()` va `env::var("X").is_err()` qanday farq qiladi?
11. `println!` va `eprintln!` qanday farq qiladi va qayerda qaysinisini ishlatish kerak?

## Related Pages

- [[11-writing-automated-tests-the-rust-programming-language|11. Writing Automated Tests]] — oldingi bob
- [[env-args]]
- [[fs-read-to-string]]
- [[separation-of-concerns]]
- [[unwrap-or-else]]
- [[process-exit]]
- [[constructor-functions]]
- [[12-an-io-project-the-rust-programming-language]] (source)
- [[12-1-accepting-command-line-arguments-the-rust-programming-language]] (source)
- [[12-2-reading-a-file-the-rust-programming-language]] (source)
- [[12-3-refactoring-to-improve-modularity-and-error-handling-the-rust-programming-language]] (source)
- [[12-4-adding-functionality-with-test-driven-development-the-rust-programming-language]] (source)
- [[12-5-working-with-environment-variables-the-rust-programming-language]] (source)
- [[12-6-redirecting-errors-to-standard-error-the-rust-programming-language]] (source)
