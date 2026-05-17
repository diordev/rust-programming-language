---
title: "Fn Traits: FnOnce, FnMut, Fn"
type: concept
status: active
created: 2026-05-07
updated: 2026-05-13
tags: [rust, fn-traits, closures, traits, functional]
source_count: 3
---

# Fn Traits: FnOnce, FnMut, Fn

## Short Definition

Closure'ni qanday chaqirish mumkinligini ifodalovchi uch pog'onali trait hierarchy. Closure body nima qilishiga qarab bir, ikki, yoki uchala trait avtomatik implement qilinadi. Additive — yuqori qo'shiladi: `Fn` ham `FnMut`, ham `FnOnce`ni implement qiladi.

## Why It Matters

Funksiya signature'ida `F: FnOnce()`, `F: FnMut()`, yoki `F: Fn()` yozish orqali closure'dan qanday foydalanilishini aniqlanadi. Bu closure'lar bilan ishlashning xavfsizligini ta'minlaydi: masalan, `sort_by_key` closure'ni bir necha marta chaqirishi kerak — shuning uchun `FnMut` talab qiladi, `FnOnce` emas.

## Mental Model

```
Fn  ⊂  FnMut  ⊂  FnOnce
```

- `FnOnce` — eng keng: barcha closures kiradi
- `FnMut` — bir necha marta chaqiriladi, capture mutatsiya bo'lishi mumkin
- `Fn` — eng tor: concurrent xavfsiz, muhitni o'zgartirmaydi

Agar closure `FnOnce` bo'lsa — `FnMut` yoki `Fn` bo'lmaydi.
Agar `FnMut` bo'lsa — `FnOnce` ham bo'ladi, lekin `Fn` emas.
Agar `Fn` bo'lsa — uchala trait ham implement qilingan.

[[function-pointers|Function pointer]] (`fn(...) -> ...`) closure emas, lekin uchala `Fn*` traitni implement qiladi. Shuning uchun `F: Fn(...)` bound named functionni ham, closure'ni ham qabul qiladi.

## Syntax and Examples

```rust
// FnOnce: bir marta chaqiriladi, captured qiymat move bo'ladi
fn apply_once<F: FnOnce() -> String>(f: F) -> String {
    f()
}

// FnMut: bir necha marta, mutatsiya bo'lishi mumkin
fn apply_multiple<F: FnMut()>(mut f: F) {
    f();
    f();
    f();
}

// Fn: concurrent xavfsiz, bir necha marta
fn apply_shared<F: Fn() -> i32>(f: F) -> i32 {
    f() + f()
}
```

```rust
fn add_one(x: i32) -> i32 { x + 1 }

fn apply<F: Fn(i32) -> i32>(f: F) -> i32 {
    f(5)
}

assert_eq!(apply(add_one), 6);      // function pointer ham Fn
assert_eq!(apply(|x| x + 1), 6);    // closure ham Fn
```

```rust
// unwrap_or_else — FnOnce (eng moslashuvchan)
impl<T> Option<T> {
    pub fn unwrap_or_else<F>(self, f: F) -> T
    where F: FnOnce() -> T { ... }
}

// sort_by_key — FnMut (bir necha marta chaqiriladi)
list.sort_by_key(|r| r.width); // bu closure FnMut
```

```rust
pub fn execute<F>(&self, f: F)
where
    F: FnOnce() + Send + 'static,
{
    let job = Box::new(f);
    self.sender.send(job).unwrap();
}
```

Thread pool `execute` uchun `FnOnce` to'g'ri bound, chunki har job worker tomonidan aynan bir marta bajariladi. `Fn` yoki `FnMut` ham ishlashi mumkin bo'lgan closure'lar allaqachon `FnOnce`ni implement qiladi.

```rust
// FnOnce closure sort_by_key'da ISHLAYDI:
let mut num = 0;
list.sort_by_key(|r| { num += 1; r.width }); // FnMut ✓

// FnOnce closure sort_by_key'da ISHLAMAYDI:
let value = String::from("x");
list.sort_by_key(|r| {
    sort_ops.push(value); // value move bo'ladi — E0507 ✗
    r.width
});
```

## Common Mistakes

**1. `FnMut` kerak joyda value'ni move qilish:**
`sort_by_key` har element uchun closure'ni chaqiradi. Agar closure `value`'ni move qilsa, ikkinchi chaqiruvda `value` yo'q — bu E0507.

**2. `FnMut` closure'ni `mut` siz chaqirish:**
```rust
let mut count = 0;
let mut increment = || count += 1; // FnMut
increment(); // ok
// lekin: let increment = || ... — mut siz E0596
```

**3. `Fn` kerak joyda `FnMut` uzatish:**
Concurrent kontekstda `Fn` talab qilinishi mumkin, `FnMut` thread-safe emas.

## Related Concepts

- [[closures]] — `Fn` traitlarini implement qiluvchi asosiy tuzilma
- [[function-pointers|function pointers]] — uchala `Fn*` traitni implement qiladi
- [[returning-closures|returning closures]] — return type sifatida `impl Fn` yoki `Box<dyn Fn>`
- [[traits]] — `FnOnce`, `FnMut`, `Fn` standart trait'lar
- [[trait-bounds]] — `F: FnMut()` kabi generic bound
- [[generics]] — closure qabul qiluvchi funksiyalar generic bo'ladi
- [[concurrency]] — `Fn` concurrent xavfsiz, `FnMut` emas
- [[thread-pool]]
- [[send-trait|Send trait]]
- [[static-lifetime|static lifetime]]

## Sources

- [[13-1-closures|13.1 Closures]]
- [[wiki/sources/20-4-advanced-functions-and-closures|20.4 Advanced Functions and Closures]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
