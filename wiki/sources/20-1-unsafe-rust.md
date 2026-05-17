---
title: "20.1. Unsafe Rust"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, ffi, raw-pointers]
source_count: 1
---

# 20.1. Unsafe Rust

## Source

- File: `raw/books/the_rust_programming_language/20.1. Unsafe Rust.md`
- URL: <https://doc.rust-lang.org/stable/book/ch20-01-unsafe-rust.html>
- Book: *The Rust Programming Language*

## Detailed Summary

Rust'da kompiler memory safetyni compile time'da tekshiradi. Lekin Rust ichida **ikkinchi til** bor — *unsafe Rust* — bu kafolatlarni amalga oshirmaydi. Unsafe Rust mavjud sabablari:

1. **Static analysis konservativ** — kompiler ba'zi to'g'ri kodni rad etadi, chunki ishonch hosil qila olmaydi. `unsafe` bilan: "ishonib qo'y."
2. **Hardware tabiatan unsafe** — OS API'lari, memory-mapped I/O, va embedded ishlar uchun low-level kirish kerak.

`unsafe` "tekshiruvlarni o'chirish" emas: borrow checker, type checker, lifetime'lar — barchasi davom etadi. `unsafe` faqat **5 ta superpower**'ga ruxsat beradi (boshqa joyda yo'q).

### 5 ta Unsafe Superpowers

1. **Raw pointer dereference qilish.** `*const T`, `*mut T` — borrow rules bo'ysunmaydi, null bo'lishi mumkin, validlikni kafolatlamaydi. Yangi sintaksis: `&raw const x`, `&raw mut x` (raw borrow operators). Yaratish safe, dereference unsafe.

2. **Unsafe function/method chaqirish.** `unsafe fn name() { ... }` — caller `unsafe { name() }` orqali chaqirib invariantlarni saqlashga va'da beradi. Rust 2024'da `unsafe fn` ichida ham `unsafe` blok kerak (warn).

3. **Mutable static'ga kirish.** `static mut COUNTER: u32 = 0;` — global mutable. Multi-thread'da data race xavfi → UB. `static_mut_refs` lint reference yaratishni man qiladi; `&raw const COUNTER` ishlatish kerak. `SAFETY:` izohi konventsiyasi.

4. **Unsafe trait implement qilish.** `unsafe trait Foo { ... }` + `unsafe impl Foo for T { ... }`. Misollar: `Send`, `Sync` (auto traits, lekin raw pointer'li struct'lar uchun qo'lda implement qilinadi).

5. **Union field'iga kirish.** `union` — hamma field'lar bir xotirada. C interop uchun, sof Rust'da `enum` ishlatish ma'qul.

### Safe Abstraction over Unsafe

Eng muhim pattern: ichkarida `unsafe` ishlatib, tashqaridan safe API berish. Misol — `split_at_mut`:

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();

    assert!(mid <= len);

    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

Borrow checker bir slice'ni ikki marta mutably borrow qila olmaydi (Listing 20-5 fail). Lekin `mid` slice ichida bo'lsa va ikki bo'lak non-overlapping bo'lsa — xavfsiz. `unsafe` blok bu kompilyator bilmagan haqiqatni isbotlash uchun.

Anti-pattern (Listing 20-7): ixtiyoriy adresdan slice yaratish — UB. [[miri|Miri]] bunday holatlarni runtime'da ushlaydi.

### FFI (Foreign Function Interface)

`extern` kalit so'zi orqali boshqa tilda yozilgan funksiyalarni chaqirish.

```rust
unsafe extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("{}", abs(-3));
    }
}
```

`unsafe extern "C"` blokidagi har item implicitly unsafe. `safe` keyword — xavfsiz funksiyalarni alohida belgilash imkoni:

```rust
unsafe extern "C" {
    safe fn abs(input: i32) -> i32;
}
```

`safe` — dasturchining va'dasi. Kompilyator ishonadi.

Rust funksiyasini boshqa tilga ochish uchun `extern "C" fn` + `#[unsafe(no_mangle)]` ishlatiladi:

```rust
#[unsafe(no_mangle)]
pub extern "C" fn call_from_c() {
    println!("Just called a Rust function from C!");
}
```

### Mutable Static — Misol

```rust
static mut COUNTER: u32 = 0;

/// SAFETY: Calling this from more than a single thread at a time is UB,
/// so you *must* guarantee single-threaded access.
unsafe fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}

fn main() {
    unsafe {
        // SAFETY: only main thread.
        add_to_count(3);
        println!("COUNTER: {}", *(&raw const COUNTER));
    }
}
```

Multi-thread bo'lsa: `Mutex<T>`, `RwLock<T>`, yoki `Atomic*` ishlatish kerak.

### Miri — UB Detector

Nightly tool. Static borrow checker'dan farqi: **runtime'da** kodni interpret qilib UB'ni ushlaydi.

```shell
rustup +nightly component add miri
cargo +nightly miri run
cargo +nightly miri test
```

Listing 20-7 (ixtiyoriy adres) Miri'da:
```text
error: Undefined Behavior: pointer not dereferenceable: pointer must be
       dereferenceable for 40000 bytes, but got 0x1234[noalloc]
```

Cheklov: faqat ishlatilgan kodni tekshiradi. Test coverage past bo'lsa — bug'lar topilmaydi.

### Foydalanish Konventsiyalari

- **`SAFETY:` izohi** — har unsafe blok va unsafe funksiya tepasida yozish; nima saqlanishi kerakligini tushuntirish.
- **Unsafe blok minimal scope** — memory bug topilganda kichik joy qarash uchun.
- **Safe abstraction** — public API hech qachon `unsafe` bo'lmasligi (mumkin bo'lganda).
- **Auditing** — `unsafe` bloklar har birini ko'rib chiqish kerak, chunki ulardan biri butun program memory safety'sini buzishi mumkin.

## Key Concepts

- [[unsafe-rust|unsafe Rust]]
- [[unsafe-superpowers|unsafe superpowers]]
- [[raw-pointer|raw pointer]]
- [[raw-borrow-operators|raw borrow operators]]
- [[safe-abstraction|safe abstraction]]
- [[ffi|FFI]]
- [[extern-block|extern block]]
- [[mutable-static|mutable static]]
- [[unsafe-trait|unsafe trait]]
- [[union-type|union]]
- [[undefined-behavior|undefined behavior]]
- [[miri|Miri]]
- [[send-trait|Send trait]]
- [[sync-trait|Sync trait]]
- [[memory-safety|memory safety]]
- [[data-race|data race]]

## Code Examples

- [[split-at-mut-unsafe|split_at_mut safe abstraction]] — Listing 20-4..20-7

## Exercises or Practice Ideas

1. `Vec<T>` ning `push` metodini soddalashtirilgan implementatsiyasini yozing — capacity oshirish unsafe ishlatishni talab qiladi (lekin Vec'ning real implementatsiyasiga qarash mumkin).
2. C kutubxonasidagi bitta funksiyani Rust'dan chaqirish (masalan, `libc::strlen`).
3. `static mut` o'rniga `AtomicU32` bilan counter yozish va benchmark farqini ko'rish.
4. Listing 20-7 ni Miri ostida ishlatib, qaysi xato topilishini tekshiring.
5. `unsafe trait MyMarker` yarating va uni `Send`-style auto trait bilan implement qiling.

## Questions Raised

- `static_mut_refs` lint qaysi Rust versiyadan boshlab default deny bo'ldi?
- Stacked Borrows nimani qo'shadi raw pointer aliasing rules ustida?
- `extern "C-unwind"` ni qachon ishlatish kerak?
- `safe` keyword extern blokda qaysi versiyadan kelgan?
- `union` ichidagi `Drop` impl'ini qanday boshqarish kerak (`ManuallyDrop`)?

## Links To Update

- [[index|Rust Wiki Index]] — yangi sourcelar va concept'lar
- [[overview]] — unsafe Rust haqida qisqa eslatma
- [[wiki/chapters/20-1-unsafe-rust|Chapter 20.1]] — chapter sahifasi
- [[unsafe-rust]] — mavjud sahifa kengaytirildi
- [[send-trait|Send]], [[sync-trait|Sync]] — unsafe trait kontekstidagi rolni qo'shish
- [[static-items|static items]] — `static mut` haqida bo'lim
