---
title: "minigrep: Command Line Arguments"
type: example
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, minigrep, example]
source_count: 1
---

# minigrep: Command Line Arguments

## Maqsad

`std::env::args()` yordamida command line argumentlarni o'qish va `Vec<String>` ga yig'ish.

## Kod

**Listing 12-1: barcha argumentlarni debug chiqarish**

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    dbg!(args);
}
```

Ishga tushirish:

```shell
$ cargo run -- needle haystack
[src/main.rs:4:5] args = [
    "target/debug/minigrep",
    "needle",
    "haystack",
]
```

**Listing 12-2: argumentlarni o'zgaruvchilarga saqlash**

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    let query = &args[1];
    let file_path = &args[2];

    println!("Searching for {query}");
    println!("In file {file_path}");
}
```

## Tushuntirish

- `args[0]` — binary fayl nomi (masalan `target/debug/minigrep`)
- `args[1]` — birinchi foydalanuvchi argumenti (`query`)
- `args[2]` — ikkinchi argument (`file_path`)
- `Vec<String>` type annotation — `collect()` ga qaysi container yasashni ko'rsatadi
- `--` cargo bilan binary argumentlari orasini ajratadi

## Related Pages

- [[env-args|std::env::args]]
- [[collect]]
- [[type-annotations]]
- [[cli]]
- [[12-1-accepting-command-line-arguments]]
