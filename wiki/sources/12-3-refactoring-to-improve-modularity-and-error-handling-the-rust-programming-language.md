---
title: "12.3. Refactoring to Improve Modularity and Error Handling"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, refactoring, cli, error-handling, modularity]
source_count: 1
---

# 12.3. Refactoring to Improve Modularity and Error Handling

## Source

- Manba: The Rust Programming Language, 12.3
- URL: https://doc.rust-lang.org/stable/book/ch12-03-improving-error-handling-and-modularity.html

## Detailed Summary

12.2 da yozilgan `minigrep` kodida to'rtta muammo bor edi. Bu bo'lim ularning barchasini bosqichma-bosqich hal qiladi.

### To'rt muammo

1. **`main` da ko'p responsibility** — argument tahlili ham, fayl o'qishi ham bitta funksiyada
2. **Konfiguratsiya o'zgaruvchilari tarqoq** — `query` va `file_path` alohida o'zgaruvchilar, bog'liqligi ko'rinmaydi
3. **`expect` xato xabarlari noaniq** — foydalanuvchi uchun tushunarli emas
4. **Xato ishlovchi kodi bir joyda emas** — kelajakda qiyinlashadi

### Qadamba-qadam refactoring

#### 1-qadam: `parse_config` funksiyasini ajratish

```rust
fn parse_config(args: &[String]) -> (&str, &str) {
    let query = &args[1];
    let file_path = &args[2];
    (query, file_path)
}
```

`main` endi `parse_config(&args)` ni chaqiradi — argument logikasini bilishi shart emas.

#### 2-qadam: `Config` struct — konfiguratsiya qiymatlarini guruhlash

```rust
struct Config {
    query: String,
    file_path: String,
}

fn parse_config(args: &[String]) -> Config {
    let query = args[1].clone();
    let file_path = args[2].clone();
    Config { query, file_path }
}
```

**Nima uchun `.clone()`?** `Config` owned `String` saqlaydi. `args` dan borrow qilish lifetime muammo tug'diradi. `clone` oddiy, biroz samarasiz, lekin `query` va `file_path` kichik string bo'lgani uchun maqbul.

#### 3-qadam: `Config::new` — associated constructor

`parse_config` → `Config::new` ga aylantirildi:

```rust
impl Config {
    fn new(args: &[String]) -> Config {
        let query = args[1].clone();
        let file_path = args[2].clone();
        Config { query, file_path }
    }
}
// main: let config = Config::new(&args);
```

Idiomatik: `String::new()` kabi type'ga bog'liq constructor.

#### 4-qadam: Xatolarni yaxshilash — `panic!` dan `Result`ga

`new` → `build` ga nom o'zgartirildi. Sabab: Rust convention'da `new` hech qachon muvaffaqiyatsiz bo'lmaydi deb kutiladi. `build` — muvaffaqiyatsiz bo'lishi mumkinligini bildiradi.

```rust
impl Config {
    fn build(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 {
            return Err("not enough arguments");
        }
        let query = args[1].clone();
        let file_path = args[2].clone();
        Ok(Config { query, file_path })
    }
}
```

`'static str` — xato xabari string literaldir, u dastur davomida yashaydi.

`main` da `unwrap_or_else` + closure bilan ishlash:

```rust
use std::process;

let config = Config::build(&args).unwrap_or_else(|err| {
    println!("Problem parsing arguments: {err}");
    process::exit(1);
});
```

`process::exit(1)` — dasturni darhol to'xtatadi, nonzero exit kodi bilan (xato bo'lganini bildiradi).

#### 5-qadam: `run` funksiyasini ajratish

Barcha qolgan logika `run` funksiyasiga ko'chirildi:

```rust
fn run(config: Config) -> Result<(), Box<dyn Error>> {
    let contents = fs::read_to_string(config.file_path)?;
    println!("With text:\n{contents}");
    Ok(())
}
```

- `Box<dyn Error>` — turli xato turlarini qaytarish imkonini beradi (dynamic dispatch)
- `?` operatori — `expect` o'rniga; xato bo'lsa `main`ga qaytaradi
- `Ok(())` — success holati, lekin qaytariladigan qiymat yo'q

`main` da `run` xatosini ishlash:

```rust
if let Err(e) = run(config) {
    println!("Application error: {e}");
    process::exit(1);
}
```

`unwrap_or_else` o'rniga `if let Err(e)` — chunki `run` muvaffaqiyatda `()` qaytaradi, uni unwrap qilishning ma'nosi yo'q.

#### 6-qadam: `src/lib.rs` ga ajratish

`main.rs` faqat entry point bo'lishi kerak. Logika `lib.rs`ga ko'chiriladi:

```rust
// src/lib.rs
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    unimplemented!()
}
```

`main.rs` esa:
```rust
use minigrep::search;
```

Afzalliklar:
- `lib.rs` dagi kod `integration tests` orqali test qilinishi mumkin
- `main` funksiyasi to'g'ridan-to'g'ri test qilinmaydi, shuning uchun logika undan chiqariladi

### Umumiy natija

```
src/main.rs   → entry point: args o'qish, Config yasash, run chaqirish, xatolarni ishlash
src/lib.rs    → logika: search, run, Config, Config::build
```

## Key Concepts

- [[separation-of-concerns|separation of concerns]]
- [[structs|Config struct pattern]]
- [[constructor-functions|Config::build naming convention]]
- [[unwrap-or-else|unwrap_or_else]]
- [[process-exit|process::exit]]
- [[box-dyn-error|Box<dyn Error>]]
- [[question-mark-operator|? operatori]]
- [[if-let|if let Err(e)]]
- [[static-lifetime|'static lifetime]]
- [[closures|closures (qisqacha tanishuv)]]
- [[library-crate|library crate]]

## Code Examples

- Listing 12-5: `parse_config` ajratish
- Listing 12-6: `Config` struct
- Listing 12-7: `Config::new`
- Listing 12-8: argument soni tekshiruvi + `panic!`
- Listing 12-9: `Config::build` — `Result` qaytaradi
- Listing 12-10: `unwrap_or_else` + `process::exit(1)`
- Listing 12-11: `run` funksiyasi
- Listing 12-12: `run` — `Result<(), Box<dyn Error>>`
- Listing 12-13: `search` — `src/lib.rs`da
- Listing 12-14: `src/main.rs` — `use minigrep::search`

## Exercises or Practice Ideas

- `Config::build`ni o'zingiz bilan yozing: argumentlar noto'g'ri formatda bo'lsa ham aniq xabar bering.
- `run` funksiyasini `src/lib.rs`ga ko'chiring va `src/main.rs`dan import qiling.
- `process::exit` o'rniga `main -> Result<(), Box<dyn Error>>` bilan yozing.

## Questions Raised

- `unwrap_or_else` va `if let Err` qachon birini, qachon ikkinchisini ishlatish kerak?
- `Box<dyn Error>` dinamik dispatch nima va u qanday ishlaydi? (18-bob)
- Closures qanday ishlaydi? (13-bob)

## Links To Update

- [[separation-of-concerns]]
- [[constructor-functions]]
- [[unwrap-or-else]]
- [[process-exit]]
- [[12-an-io-project]] (chapter)
