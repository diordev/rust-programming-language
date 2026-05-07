---
title: "minigrep: Reading a File"
type: example
status: draft
created: 2026-05-07
updated: 2026-05-07
tags: [rust, cli, minigrep, example, io]
source_count: 1
---

# minigrep: Reading a File

## Maqsad

`std::fs::read_to_string` yordamida faylni to'liq o'qish va mazmunini chiqarish.

## Kod

**Listing 12-4: faylni o'qish**

```rust
use std::env;
use std::fs;

fn main() {
    let args: Vec<String> = env::args().collect();

    let query = &args[1];
    let file_path = &args[2];

    println!("Searching for {query}");
    println!("In file {file_path}");

    let contents = fs::read_to_string(file_path)
        .expect("Should have been able to read the file");

    println!("With text:\n{contents}");
}
```

## Ishga tushirish

```shell
$ cargo run -- the poem.txt
Searching for the
In file poem.txt
With text:
I'm nobody! Who are you?
Are you nobody, too?
...
```

## Tushuntirish

- `fs::read_to_string(path)` — faylni `String`ga o'qiydi, `Result<String, io::Error>` qaytaradi
- `.expect(msg)` — muvaffaqiyatsiz bo'lsa berilgan xabar bilan panic
- Hozircha xato ishlash sodda — 12.3 da yanada yaxshilanadi

## Related Pages

- [[fs-read-to-string|fs::read_to_string]]
- [[file-io|File I/O]]
- [[result|Result]]
- [[expect]]
- [[cli]]
- [[12-2-reading-a-file-the-rust-programming-language]]
