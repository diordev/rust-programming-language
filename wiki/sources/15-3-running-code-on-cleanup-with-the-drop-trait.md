---
title: "15.3. Running Code on Cleanup with the Drop Trait"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, smart-pointers, drop, memory, ownership]
source_count: 1
---

# 15.3. Running Code on Cleanup with the Drop Trait

## Source

`raw/books/the_rust_programming_language/15.3. Running Code on Cleanup with the Drop Trait.md`
URL: https://doc.rust-lang.org/stable/book/ch15-03-drop.html

## Detailed Summary

Bu bo'lim smart pointer pattern uchun ikkinchi muhim trait — `Drop`ni tushuntiradi. `Drop` qiymat scope dan chiqib ketganda avtomatik ishlaydi va xotira, fayl, tarmoq ulanishi kabi resurslarni tozalash imkonini beradi.

### Drop Trait qanday implement qilinadi

`Drop` trait prelude da bo'ladi — alohida `use` shart emas. Faqat `drop(&mut self)` metodini yozish kerak:

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created");
}
// Chiqish:
// CustomSmartPointers created
// Dropping CustomSmartPointer with data `other stuff`!
// Dropping CustomSmartPointer with data `my stuff`!
```

O'zgaruvchilar **yaratilish tartibining teskarisida** drop qilinadi: `d` avval, so'ng `c`.

### Drop ni qo'lda chaqirib bo'lmaydi

`c.drop()` yozilsa [[e0040-explicit-use-of-destructor|E0040]] xatosi chiqadi. Sababi: Rust scope oxirida avtomatik ham chaqiradi — natijada **double free** muammosi yuzaga keladi.

```rust
c.drop(); // ERROR: explicit use of destructor method
```

### Erta tozalash: std::mem::drop

Lock yoki boshqa resursni scope tugashidan oldin bo'shatish kerak bo'lsa, `drop(value)` funksiyasi ishlatiladi. Bu ham prelude da bo'ladi:

```rust
fn main() {
    let c = CustomSmartPointer { data: String::from("some data") };
    println!("CustomSmartPointer created");
    drop(c);  // erta va xavfsiz tozalash
    println!("CustomSmartPointer dropped before the end of main");
}
// Chiqish:
// CustomSmartPointer created
// Dropping CustomSmartPointer with data `some data`!
// CustomSmartPointer dropped before the end of main
```

`std::mem::drop` va `Drop::drop` metodi — ikki alohida narsa. Birinchisi funksiya (qo'lda ishlatiladi), ikkinchisi trait metodi (faqat Rust avtomatik chaqiradi).

### Box<T> va Drop aloqasi

`Box<T>` scope dan chiqqanda `Drop` implement qilgani uchun heap da saqlangan data ham avtomatik tozalanadi. Programmist `free()` yoki `delete` chaqirmaydi.

## Key Concepts

- [[drop|Drop trait]] — cleanup logic uchun; `drop(&mut self)` implement qilinadi
- [[destructor]] — umumiy dasturlash termini; Rustda `drop` funksiyasi = destructor
- `std::mem::drop` — erta tozalash funksiyasi (prelude da)
- Teskari tartib — drop yaratilish tartibining teskarisida ishlaydi
- [[double-free|double free]] — `Drop::drop`ni qo'lda chaqirish taqiqining sababi

## Code Examples

Listing 15-14: Drop trait implement qilish
Listing 15-15: `c.drop()` — E0040 xatosi
Listing 15-16: `drop(c)` — to'g'ri erta tozalash usuli

## Exercises or Practice Ideas

1. `CustomSmartPointer` yozib, bir necha instance yarating; drop tartibini terminal chiqishidan kuzating.
2. `c.drop()` chaqirib, E0040 xatosini qarang va `drop(c)` bilan to'g'rilang.
3. `Box<T>` va `CustomSmartPointer`ni birgalikda ishlatib, ikkalasi drop qilinishini kuzating.
4. Lock simulyatsiyasi: `MutexGuard`-like struct yozib, `Drop` orqali lock bo'shatishni modellashtiring.

## Questions Raised

- `Rc<T>` drop qilinganda nima bo'ladi? (keyingi 15.4 bo'limi)
- `RefCell<T>` runtime borrow checking bilan Drop qanday ishlaydi?

## Links To Update

- [[drop]] — Drop trait bo'limini kengaytirish
- [[wiki/chapters/15-smart-pointers]] — 15.3 bo'limini qo'shish
- [[index]] — yangi source qo'shish
