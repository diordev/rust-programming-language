---
title: "eprintln! makrosi"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, stderr, macros, cli]
source_count: 1
---

# eprintln! makrosi

## Short Definition

`eprintln!` — `println!` singari ishlaydi, lekin chiqishni `stdout` emas, `stderr` (standard error) ga yuboradi.

## Why It Matters

CLI dasturlarda xato xabarlari va oddiy ma'lumotlar alohida oqimlarda bo'lishi lozim. Bu foydalanuvchiga muvaffaqiyatli chiqishni faylga yo'naltirib, xato xabarlarini ekranda ko'rish imkonini beradi — bu keng tarqalgan Unix konventsiyasi.

## Mental Model

```
println!("natija")   →  stdout (fayl/pipe yo'naltirilishi mumkin)
eprintln!("xato!")   →  stderr (ekranda qoladi)

$ cargo run > output.txt
   # stdout → output.txt
   # stderr → terminal ekranida ko'rinadi
```

## Syntax and Examples

```rust
// Xato xabarlari — eprintln!
eprintln!("Problem parsing arguments: {err}");
eprintln!("Application error: {e}");

// Oddiy chiqish — println!
println!("{line}");
println!("Natija: {}", value);
```

**Amaliy misol — `minigrep` `main.rs`:**

```rust
let config = Config::build(&args).unwrap_or_else(|err| {
    eprintln!("Problem parsing arguments: {err}");
    process::exit(1);
});

if let Err(e) = run(config) {
    eprintln!("Application error: {e}");
    process::exit(1);
}
```

**Shell test:**

```shell
# Xato — stderr ekranda, stdout bo'sh fayl
$ cargo run > output.txt
Problem parsing arguments: not enough arguments

# Muvaffaqiyat — stdout faylga, ekran toza
$ cargo run -- to poem.txt > output.txt
```

## Common Mistakes

- Xato xabarlarini `println!` bilan yozish — faylga redirect qilinganida xabar yo'qoladi yoki noto'g'ri joyga tushadi.
- `eprint!` (yangi qator yo'q) ham mavjud — `print!` ning `stderr` analogiyasi.

## Related Concepts

- [[println-macro|println!]] — `stdout`ga chiqarish
- [[stderr]] — standard error oqimi
- [[stdout]] — standard output oqimi
- [[process-exit|process::exit]] — xato holatida dasturni to'xtatish

## Sources

- [[12-6-redirecting-errors-to-standard-error|12.6. Redirecting Errors to Standard Error]]
