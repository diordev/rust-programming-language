---
title: "minigrep Iterator Refactor"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, iterators, closures, refactoring, minigrep, clone, impl-trait]
source_count: 1
---

# minigrep Iterator Refactor

12-bobdagi `minigrep` loyihasini 13.3-bobda closures va iterators bilan yaxshilash — ikkita o'zgarish.

## 1. `Config::build`: iterator qabul qilish, `clone` olib tashlash

**Oldin** (`&[String]` slice, clone zarur):

```rust
// main.rs
let args: Vec<String> = env::args().collect();
let config = Config::build(&args).unwrap_or_else(|err| { ... });

// lib.rs
impl Config {
    fn build(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 {
            return Err("not enough arguments");
        }
        let query = args[1].clone();       // clone — heap alloc
        let file_path = args[2].clone();   // clone — heap alloc
        let ignore_case = env::var("IGNORE_CASE").is_ok();
        Ok(Config { query, file_path, ignore_case })
    }
}
```

**Keyin** (`impl Iterator<Item = String>`, clone yo'q):

```rust
// main.rs
let config = Config::build(env::args()).unwrap_or_else(|err| { ... });

// lib.rs
impl Config {
    fn build(
        mut args: impl Iterator<Item = String>,
    ) -> Result<Config, &'static str> {
        args.next(); // dastur nomini o'tkazib yuborish

        let query = match args.next() {
            Some(arg) => arg,
            None => return Err("Didn't get a query string"),
        };

        let file_path = match args.next() {
            Some(arg) => arg,
            None => return Err("Didn't get a file path"),
        };

        let ignore_case = env::var("IGNORE_CASE").is_ok();
        Ok(Config { query, file_path, ignore_case })
    }
}
```

**Nima yaxshilandi:**
- `Vec<String>` yaratish va `clone` alloclari yo'q
- Error xabarlari aniqroq
- `env::args()` iterator to'g'ridan-to'g'ri uzatiladi

## 2. `search`: mutable Vec → iterator chain

**Oldin** (mutable oraliq holat):

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

**Keyin** (declarative, immutable):

```rust
pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    contents
        .lines()
        .filter(|line| line.contains(query))
        .collect()
}
```

**Nima yaxshilandi:**
- Mutable `results` Vec yo'q
- Kod niyatini bir qatorda ifodalaydi
- Kelajakda parallel qilish osonroq (mutable shared state yo'q)

## Bonus: to'liq lazy `search`

```rust
pub fn search<'a>(
    query: &str,
    contents: &'a str,
) -> impl Iterator<Item = &'a str> {
    contents
        .lines()
        .filter(move |line| line.contains(query))
}
```

`run()` ichidagi `for line in results` har qator topilishi bilan chop etadi — barcha natijalar yig'ilishini kutmaydi.

## Related Pages

- [[13-3-improving-our-io-project|Source: 13.3]]
- [[iterators]]
- [[iterator-adapters]]
- [[consuming-adapters]]
- [[impl-trait]]
- [[closures]]
- [[clone]]
- [[lifetimes]]
