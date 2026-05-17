---
title: "Type Alias"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-13
tags: [rust, types, aliases]
source_count: 2
---

# Type Alias

## Short Definition

`type Name = ExistingType;` — mavjud tipga **boshqa nom** berish. Yangi tip yaratmaydi — sinonim. Asosiy maqsad: uzun tip nomini qisqartirish va kodni o'qilishini oshirish.

## Why It Matters

Murakkab tiplar (`Box<dyn Fn() + Send + 'static>`, `Result<T, std::io::Error>`) ko'p marta yozilsa kod chiqimchi va xatoga moyil bo'ladi. Type alias bir martalik nom beradi va `?` operator, method'lar avtomatik ishlaydi (chunki tip o'zgarmaydi).

## Type Alias vs Newtype Pattern

| Xususiyat | Type Alias | [[newtype-pattern\|Newtype]] |
|-----------|------------|------------|
| Yangi tip | Yo'q (sinonim) | Ha (alohida tip) |
| Type safety | Yo'q | Ha (aralashtirib bo'lmaydi) |
| Method'lar | Inner barcha | Inner method'lar avtomatik kelmaydi |
| Trait impl | Inner uchun mavjud bo'lganlar | Yangi tip uchun yangi |
| Maqsad | Qisqartirish | Type safety, abstraktsiya, encapsulation |

`type Kilometers = i32;` — `Kilometers` va `i32` aralashtirib bo'ladi.
`struct Kilometers(i32);` — newtype, aralashtirib bo'lmaydi.

## Syntax and Examples

### Asosiy

```rust
type Kilometers = i32;

let x: i32 = 5;
let y: Kilometers = 10;
println!("{}", x + y);   // 15 — `i32` + `Kilometers` ishlaydi
```

`Kilometers` va `i32` — bir xil tip. Aralashtirish kompilyator tomonidan to'xtatilmaydi.

### Listing 20-25/26 — `Thunk` alias

Murakkab tip:

```rust
fn takes_long_type(f: Box<dyn Fn() + Send + 'static>) { /* ... */ }
fn returns_long_type() -> Box<dyn Fn() + Send + 'static> {
    Box::new(|| ())
}
```

Type alias bilan:

```rust
type Thunk = Box<dyn Fn() + Send + 'static>;

fn takes_long_type(f: Thunk) { /* ... */ }
fn returns_long_type() -> Thunk {
    Box::new(|| ())
}
```

*Thunk* — kechiktirilgan baholanish uchun saqlangan kod (closure tarjima'da mos atama).

### `std::io::Result<T>` — keng tarqalgan misol

`std::io` modulida:

```rust
type Result<T> = std::result::Result<T, std::io::Error>;
```

Endi `Write` trait'i:

```rust
pub trait Write {
    fn write(&mut self, buf: &[u8]) -> Result<usize>;
    fn flush(&mut self) -> Result<()>;
    fn write_all(&mut self, buf: &[u8]) -> Result<()>;
    fn write_fmt(&mut self, fmt: fmt::Arguments) -> Result<()>;
}
```

`Result<T, std::io::Error>` o'rniga `Result<T>` — qisqaroq, lekin baribir oddiy `Result<T, E>` — `?` operator, `map`, `unwrap_or` — barchasi ishlaydi.

### Generic alias

```rust
type Pair<T> = (T, T);
type Lookup<K, V> = std::collections::HashMap<K, V>;
type Graph<N> = HashMap<N, Vec<N>>;
```

Type parameter saqlanadi.

### Thread pool `Job` alias

```rust
type Job = Box<dyn FnOnce() + Send + 'static>;
```

Bu alias uzun trait-object tipini qisqartiradi. Semantika o'zgarmaydi: baribir bu `Box` ichidagi closure bo'lib, worker uni bir marta chaqiradi.

## Common Mistakes

- **Type alias type safety beradi deb o'ylash.** Bermaydi — `Kilometers` va `i32` bir xil. Type safety kerak bo'lsa newtype.
- **Trait'ni alias orqali implement qilishga urinish.** Alias yangi tip emas; `impl SomeTrait for Kilometers` aslida `impl SomeTrait for i32` — orphan rule'ga tushishi mumkin.
- **Pub alias bilan inner tipni private qilishga urinish.** Alias `pub` bo'lsa, foydalanuvchi inner tipni ham ko'radi (`Kilometers = i32`).
- **Recursive alias yaratishga urinish.** `type R = Box<R>;` — kompilyator buni qabul qilmaydi (newtype bilan ehtiyojni qondirish kerak).

## Related Concepts

- [[newtype-pattern|newtype pattern]] — alohida tip kerak bo'lsa
- [[generics]] — generic alias
- [[result|Result]] — `std::io::Result<T>` alias
- [[box-t|Box<T>]] — uzun closure tiplari uchun
- [[type-checking|type checking]]
- [[closures]] — `Thunk` alias misoli
- [[traits]]
- [[thread-pool]]
- [[fn-traits|Fn traits]]

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/21-2-from-single-threaded-to-multithreaded-server|21.2]]
