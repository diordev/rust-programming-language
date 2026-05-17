---
title: "Extern Block"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, ffi, syntax]
source_count: 1
---

# Extern Block

## Short Definition

`unsafe extern "ABI" { ... }` — boshqa tilda yozilgan funksiyalarni Rust'da deklaratsiya qiluvchi blok. Har bir item implicitly unsafe (agar `safe` deb belgilanmagan bo'lsa).

## Why It Matters

C kutubxonalari va OS API'lari bilan ishlashning standart yo'li. Bitta blokda ko'p tashqi funksiyalarni guruhlab, ABI'ni faqat bir marta belgilash mumkin.

## Mental Model

Extern block — **deklaratsiya** (definition emas). Funksiya tanasi yo'q, faqat signatura. Kompilyator linker'ga "bu nom boshqa kutubxonadan keladi" deb aytadi.

## Syntax and Examples

### Asosiy

```rust
unsafe extern "C" {
    fn abs(input: i32) -> i32;
    fn pow(base: f64, exp: f64) -> f64;
    static errno: i32;
}

fn main() {
    unsafe {
        println!("{}", abs(-5));
        println!("{}", pow(2.0, 10.0));
    }
}
```

### `safe` items

```rust
unsafe extern "C" {
    safe fn abs(input: i32) -> i32;        // safe — unsafe blok kerakmas
    fn dangerous_ptr_op(p: *mut u8);       // unsafe — chaqirishda blok kerak
}
```

### Linker bilan bog'lash

```rust
#[link(name = "m")]   // libm bilan link qilish
unsafe extern "C" {
    fn cos(x: f64) -> f64;
}
```

`#[link(name = "...")]` linker'ga qaysi kutubxonadan funksiyani izlashni aytadi. `Cargo.toml` da `build.rs` orqali ham sozlash mumkin.

### Ichidagi har bir item implicitly unsafe

Eski Rust'da `extern "C" { ... }` (unsafe'siz) ham mumkin edi, lekin har bir chaqiriq baribir `unsafe` blok talab qilardi. Yangi sintaksis aniqroq: blok o'zi unsafe.

## ABI Tanlash

| ABI | Foydalanish |
|-----|-------------|
| `"C"` | Eng keng tarqalgan, deyarli har joyda |
| `"system"` | Platform-specific (Windows: stdcall, Linux: C) |
| `"C-unwind"` | C ABI + Rust unwinding cross-FFI |
| `"Rust"` | Default — Rust ichidagi `extern fn` (kam ishlatiladi) |

## Common Mistakes

- **`extern` blok **definition** o'rinida ko'rish.** Tana yo'q — faqat deklaratsiya.
- **ABI'ni belgilamaslik.** `extern { ... }` default'da `"C"` emas — `"Rust"`. Doim `"C"` yoki kerakli ABI yozing.
- **Linker xatosi.** `undefined reference to abs` — kutubxona linkerda ko'rinmaydi. `#[link(name=...)]` yoki `build.rs` ishlatish.
- **`safe` ni o'ylamasdan ishlatish.** Faqat haqiqatan ham xavfsiz funksiyalarda — aks holda UB.

## Related Concepts

- [[ffi|FFI]] — umumiy kontekst
- [[unsafe-rust|unsafe Rust]]
- [[raw-pointer|raw pointer]] — C bilan data uzatish
- [[abi|ABI]] — calling convention

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
