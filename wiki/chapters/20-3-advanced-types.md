---
title: "20.3. Advanced Types"
type: chapter
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, types, advanced]
source_count: 1
---

# 20.3. Advanced Types

## Learning Goal

Rust type system'ining qolgan qism tushunish: [[type-alias|type aliases]] va [[newtype-pattern]] o'rtasidagi farq, [[never-type|never type (`!`)]] ning roli, [[dynamically-sized-types|dynamically sized types]] (DST) bilan ishlash va [[sized-trait|`Sized`]] cheklovini boshqarish.

## Main Ideas

- **Type alias** (`type Name = Existing;`) — sinonim, yangi tip emas. Maqsad: takror yozishni qisqartirish.
- **Newtype** (`struct Name(Existing)`) — alohida tip. Maqsad: type safety, abstraction, encapsulation.
- **Never type `!`** — qiymatga ega bo'la olmaydigan tip. Diverging funksiyalar va `match` arm coercion uchun.
- **DST** — `str`, `[T]`, `dyn Trait` — runtime o'lchami. Faqat pointer orqali ishlatish mumkin.
- **`Sized` trait** — implicit generic bound. `?Sized` cheklovni bo'shatadi.

## Bobning Tarkibi

| Bo'lim | Mavzu |
|--------|-------|
| Newtype Pattern | Type safety, abstraction, encapsulation (qayta) |
| Type Aliases | `type Name = ExistingType`, `Thunk`, `io::Result<T>` |
| Never Type | `!`, diverging functions, `match` arm coercion |
| DST | `str`, `[T]`, `dyn Trait`, fat pointers |
| `Sized` & `?Sized` | Generic implicit bound, DST qabul qiluvchi generics |

## Type Alias vs Newtype

| | Type Alias | Newtype |
|---|------------|---------|
| Sintaksis | `type Kilometers = i32;` | `struct Kilometers(i32);` |
| Yangi tip? | Yo'q | Ha |
| Type safety? | Yo'q | Ha |
| Inner method'lar | Avtomatik | Qo'lda delegate yoki Deref |
| Maqsad | Qisqartirish | Safety, abstraction |

## Asosiy Misollar

### Type Alias — Thunk

```rust
type Thunk = Box<dyn Fn() + Send + 'static>;

let f: Thunk = Box::new(|| println!("hi"));

fn takes_long_type(f: Thunk) { /* ... */ }
fn returns_long_type() -> Thunk { Box::new(|| ()) }
```

Batafsil: [[thunk-type-alias]].

### `std::io::Result<T>`

```rust
type Result<T> = std::result::Result<T, std::io::Error>;

pub trait Write {
    fn write(&mut self, buf: &[u8]) -> Result<usize>;
    fn flush(&mut self) -> Result<()>;
}
```

Batafsil: [[io-result-alias]].

### Never Type Coercion

```rust
let guess: u32 = match input.trim().parse() {
    Ok(n) => n,           // u32
    Err(_) => continue,   // !  →  u32 ga coerce
};
```

`continue`, `panic!`, `loop {}` — barchasi `!` qaytaradi.

### DST — `&str` vs `str`

```rust
let s1: str = "Hello";     // E0277 — str !Sized
let s2: &str = "Hello";    // OK — fat pointer (ptr + length)
```

### `?Sized` Generic

```rust
fn print<T: ?Sized + std::fmt::Display>(t: &T) {
    println!("{}", t);
}

print(&42);       // i32 (Sized)
print("hello");   // str (DST) via &
```

## Concepts

- [[newtype-pattern|newtype pattern]]
- [[type-alias|type alias]]
- [[never-type|never type (!)]]
- [[diverging-functions|diverging functions]]
- [[dynamically-sized-types|dynamically sized types]]
- [[sized-trait|Sized trait]]
- [[string-slice|string slice]] — `str` DST
- [[slices]] — `[T]` DST
- [[trait-object|trait object]] — `dyn Trait` DST
- [[panic]] — `!` source
- [[loop]] — break'siz `!`
- [[match]] — `!` coercion
- [[result|Result]] — `io::Result<T>`
- [[box-t|Box<T>]] — DST'larni saqlash
- [[generics]] — `?Sized` cheklovi

## Examples

- [[thunk-type-alias|Thunk type alias]] — Listing 20-25, 20-26
- [[io-result-alias|std::io::Result alias]]

## Exercises

1. **Type alias vs newtype:** `type Email = String` va `struct Email(String)`. `fn send(to: Email)` ga `String` argument berishga urining.
2. **DST function:** `fn len_of<T: ?Sized + AsRef<str>>(t: &T) -> usize`. `&str`, `String`, `Box<str>` bilan testlang.
3. **Diverging branches:** `Result<T>` qaytaruvchi funksiya, `Err` arm `panic!` bilan tugaydigan.
4. **Custom Result alias:** `MyError` + `MyResult<T> = Result<T, MyError>` + `?` bilan.
5. **Heterogeneous trait objects:** `Vec<Box<dyn Animal>>` ichida turli implementor'lar.
6. **Server forever:** `fn server() -> !` `loop { handle() }` bilan.

## Review Questions

1. Type alias va newtype orasidagi 4 ta asosiy farq nima?
2. `type Kilometers = i32;` da `Kilometers + i32` ishlaydimi? Nega?
3. `Thunk` aliasi qaysi 4 komponentni yashiradi?
4. `std::io::Result<T>` ning `Result<T, E>` method'lari ishlaydimi? Nega?
5. `!` tipida nechta qiymat mavjud?
6. `match` arm'i `continue` bilan tugashi mumkin — nima uchun bu type system'da to'g'ri?
7. `panic!()` qaysi tipga coerce bo'la oladi?
8. Nima uchun `let s: str = "hi";` ishlamaydi?
9. Fat pointer nima ekan va u nechta word egallaydi?
10. `T: Sized` va `T: ?Sized` orasidagi farq nima?

## Related Pages

- [[wiki/sources/20-3-advanced-types|Source: 20.3]]
- [[wiki/chapters/20-2-advanced-traits|Chapter 20.2: Advanced Traits]] — newtype, associated types
- [[wiki/chapters/20-1-unsafe-rust|Chapter 20.1: Unsafe Rust]]
- [[wiki/chapters/4-understanding-ownership|Chapter 4: Slices]] — `&str` kelib chiqishi
- [[18-object-oriented-programming|Chapter 18: OOP]] — `dyn Trait` kelib chiqishi
