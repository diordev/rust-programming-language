---
title: "FFI (Foreign Function Interface)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-09
tags: [rust, unsafe, ffi, interop]
source_count: 2
---

# FFI (Foreign Function Interface)

## Short Definition

Bir programming language'da boshqa tilda yozilgan funksiyalarni chaqirish (yoki o'z funksiyalarini boshqa tildan chaqirilishi uchun ochish) usuli. Rust'da `extern` kalit so'zi orqali amalga oshiriladi.

## Why It Matters

C, C++, Python kabi tillarda yozilgan kutubxonalardan Rust'da foydalanish, yoki Rust kodini boshqa tilga export qilish. Real-world Rust ko'pincha mavjud C ekosistemasi bilan ishlashi shart (libcurl, OpenSSL, OS API).

## Mental Model

Rust kompilyatori boshqa tilda yozilgan funksiyaning aslida nima qilishini bilmaydi va uni tekshira olmaydi — shuning uchun har qanday FFI chaqiriq **unsafe** hisoblanadi (ba'zi istisnolar bilan).

C callback APIda Rust closure emas, plain [[function-pointers|function pointer]] talab qilinishi mumkin. Rust closure capture qilsa, u environment saqlashi kerak; C esa bunday closure representationni bilmaydi.

## Syntax and Examples

### C funksiyasini chaqirish

```rust
unsafe extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("|−3| = {}", abs(-3));
    }
}
```

`unsafe extern "C"` bloki ichida deklaratsiya qilingan har bir item **implicitly unsafe**.

### `safe` keyword — xavfsiz tashqi funksiya

Ba'zi C funksiyalari memory safety bilan bog'liq emas (masalan, `abs(i32)`). Ularni `safe` deb belgilash mumkin:

```rust
unsafe extern "C" {
    safe fn abs(input: i32) -> i32;
}

fn main() {
    println!("|−3| = {}", abs(-3));   // unsafe blok kerak emas!
}
```

`safe` — **dasturchining va'dasi**: "Men bu funksiyani xavfsiz deb tasdiqlayman." Kompilyator ishonadi, lekin agar va'da buzilsa — UB.

### Rust funksiyasini boshqa tilga ochish

```rust
#[unsafe(no_mangle)]
pub extern "C" fn call_from_c() {
    println!("Just called a Rust function from C!");
}
```

- `extern "C"` — C ABI ishlatish.
- `#[unsafe(no_mangle)]` — Rust kompilyatori funksiya nomini o'zgartirmasligi (mangling). C unique nom kutadi.
- `unsafe` attribute ichida — chunki nomli to'qnashuv (collision) xavfi bor.

### ABI (Application Binary Interface)

`"C"` — eng keng tarqalgan ABI. Boshqa qiymatlar:
- `"system"` — platforma-specific (Windows da `stdcall`, Linux'da `C`)
- `"C-unwind"` — C ABI + unwind support
- Boshqalari: [Rust Reference](https://doc.rust-lang.org/stable/reference/items/external-blocks.html#abi)

ABI funksiya argumentlari registrlarda yoki stack'da qanday joylashtirilishini, qaytish qiymati qaerda saqlanishini belgilaydi.

## Common Mistakes

- **C funksiyasi signature'ini noto'g'ri yozish.** `int` ni `i32` o'rniga `i64` deb yozsangiz — UB. C tipini Rust ekvivalenti bilan to'g'ri taqqoslang (ko'pincha `c_int`, `c_char` `libc` cratesidan).
- **C str va Rust `&str` ni adash qilish.** C strings null-terminated; Rust `&str` length+pointer. `CStr`/`CString` orqali konvert qiling.
- **`#[no_mangle]` o'rniga `#[unsafe(no_mangle)]` ni unutish.** Yangi sintaksis (Rust 2024); eski faqat warning beradi.
- **Memory ownership noaniqligi.** C funksiyasi qaytargan pointerni kim free qiladi? Hujjatlarni o'qib aniqlang.
- **Capturing closure'ni C callback sifatida uzatish.** C odatda faqat function pointer va optional context pointer qabul qiladi; safe wrapper kerak bo'lishi mumkin.

## Related Concepts

- [[extern-block|extern block]] — `unsafe extern "C" { ... }`
- [[unsafe-rust|unsafe Rust]] — FFI ko'p hollarda unsafe
- [[raw-pointer|raw pointer]] — C bilan data uzatish
- [[function-pointers|function pointers]] — C callback contexti
- [[safe-abstraction|safe abstraction]] — FFI ustida safe wrapper qurish
- [[union-type|union]] — C union'lar bilan ishlash
- [[systems-programming|systems programming]] — FFI'ning asosiy konteksti

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
