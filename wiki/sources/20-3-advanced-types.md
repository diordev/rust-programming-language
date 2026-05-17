---
title: "20.3. Advanced Types"
type: source
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, types, advanced]
source_count: 1
---

# 20.3. Advanced Types

## Source

- File: `raw/books/the_rust_programming_language/20.3. Advanced Types.md`
- URL: <https://doc.rust-lang.org/stable/book/ch20-03-advanced-types.html>
- Book: *The Rust Programming Language*

## Detailed Summary

20.3 — Rust type system'ining yana to'rt xususiyatini ochadi: newtype pattern (qayta ko'rib), [[type-alias|type aliases]], [[never-type|never type (`!`)]], va [[dynamically-sized-types|dynamically sized types]] (DST).

### Newtype Pattern Qayta — Type Safety va Abstraction

20.2'da [[newtype-pattern]] orphan rule chetlab o'tish vositasi sifatida ko'rilgan. 20.3'da yana ikki foydalanishni ta'kidlaydi:

1. **Statik type safety.** `Millimeters(u32)` va `Meters(u32)` — ikkalasi `u32`, lekin aralashtirib bo'lmaydi. Funksiya `Millimeters` qabul qilsa, `Meters` yoki oddiy `u32` qabul qilmaydi.

2. **Implementation detail yashirish.** `People` newtype `HashMap<i32, String>` ni o'rab oladi. Foydalanuvchi `add_name(name: String)` API'sini ko'radi, ichida ID assignment va HashMap structure noma'lum. Bu — yengil [[encapsulation]].

### Type Aliases (Type Synonyms)

`type Name = ExistingType;` — mavjud tipga **boshqa nom** berish. Yangi tip yaratmaydi, sinonim:

```rust
type Kilometers = i32;

let x: i32 = 5;
let y: Kilometers = 10;
println!("{}", x + y);   // ishlaydi — bir xil tip
```

`Kilometers` va `i32` aralashtirib bo'ladi → newtype'dan farqli o'laroq, type safety yo'q.

#### Asosiy Foydalanish: Takrorlanishni Qisqartirish

Listing 20-25 — uzun tip qaytadan yoziladi:

```rust
fn takes_long_type(f: Box<dyn Fn() + Send + 'static>) { /* ... */ }
fn returns_long_type() -> Box<dyn Fn() + Send + 'static> {
    Box::new(|| ())
}
```

Listing 20-26 — alias bilan:

```rust
type Thunk = Box<dyn Fn() + Send + 'static>;

fn takes_long_type(f: Thunk) { /* ... */ }
fn returns_long_type() -> Thunk { Box::new(|| ()) }
```

*Thunk* — kechiktirilgan baholanish uchun saqlangan kod.

#### `std::io::Result<T>` Misoli

`std::io` modulida:

```rust
type Result<T> = std::result::Result<T, std::io::Error>;
```

`Write` trait'i `Result<T, std::io::Error>` o'rniga `Result<T>` ishlatadi:

```rust
pub trait Write {
    fn write(&mut self, buf: &[u8]) -> Result<usize>;
    fn flush(&mut self) -> Result<()>;
    fn write_all(&mut self, buf: &[u8]) -> Result<()>;
    fn write_fmt(&mut self, fmt: fmt::Arguments) -> Result<()>;
}
```

Foydalari: (1) qisqaroq yozish, (2) konsistent interfeys, (3) `?` operator va `Result<T, E>` method'lari avtomatik ishlaydi.

### Never Type — `!`

`!` — *empty type*, qiymatga ega bo'la olmaydi. Funksiya hech qachon qaytmaganda return tipi:

```rust
fn bar() -> ! {
    panic!();
}
```

Bunday funksiyalar — *[[diverging-functions|diverging functions]]*.

#### `!` ning Match'da Coercion'i

`match` arm'larining tipi bir xil bo'lishi kerak:

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,        // u32
    Err(_) => continue,    // !  →  u32 ga coerce
};
```

`continue` `!` qaytaradi (loop boshiga qaytadi, qiymat yo'q). Kompilyator: `!` har tipga coerce → match natijasi `u32`.

#### `unwrap` da `!`

```rust
impl<T> Option<T> {
    pub fn unwrap(self) -> T {
        match self {
            Some(val) => val,                           // T
            None => panic!("called on None"),           // !
        }
    }
}
```

Match natijasi `T` — `panic!` `!` qaytarib, `T`'ga coerce.

#### `loop` Ekspressiyasi

```rust
fn forever() -> ! {
    loop {
        print!("...");
    }
}
```

`loop` `break` siz — `!` qaytaradi.

### Dynamically Sized Types (DST)

Compile-time'da o'lchami noma'lum tiplar — *DST*:

- `str` — har string boshqa uzunlikda
- `[T]` — slice, runtime'da uzunlik
- `dyn Trait` — runtime'da implementatsiya

**Oltin qoida:** DST'larni har doim **pointer orqali** ishlatish kerak (reference, `Box`, `Rc`).

#### `str` ni To'g'ridan-to'g'ri Ishlatish — Xato

```rust
let s1: str = "Hello there!";    // E0277
let s2: str = "How's it going?";
```

`s1` 12 byte, `s2` 15 byte — bir xil tip turli o'lchamda bo'la olmaydi (stack ramka belgilanmagan). Yechim: `&str`, `Box<str>`, `Rc<str>`.

#### Fat Pointer

DST referensi — "fat pointer": ptr + metadata.

```
&str       =  &[ptr to str data] [length]
&[T]       =  &[ptr to slice]    [length]
&dyn Trait =  &[ptr to data]     [vtable ptr]
```

Compile-time'da fat pointer'ning o'lchami ma'lum (2 word).

#### `Sized` Trait

`Sized` — marker trait. Compile-time'da o'lchami ma'lum bo'lgan har tip avtomatik `Sized`.

Generic funksiyalar default'da `T: Sized` talab qiladi:

```rust
fn generic<T>(t: T) { /* ... */ }
// implicit:
fn generic<T: Sized>(t: T) { /* ... */ }
```

#### `?Sized` — Cheklovni Bo'shatish

DST'larni qabul qila oladigan generic uchun:

```rust
fn generic<T: ?Sized>(t: &T) { /* ... */ }
```

- `?Sized` = "T may or may not be Sized"
- `t: T` → `t: &T` — DST pointer orqali

`?Trait` sintaksisi **faqat `Sized` uchun**, boshqa trait'lar bilan ishlamaydi.

## Key Concepts

- [[newtype-pattern|newtype pattern]]
- [[type-alias|type alias]]
- [[never-type|never type (!)]]
- [[diverging-functions|diverging functions]]
- [[dynamically-sized-types|dynamically sized types]]
- [[sized-trait|Sized trait]]
- [[string-slice|string slice]]
- [[slices]]
- [[trait-object|trait object]]
- [[panic]]
- [[loop]]
- [[match]]
- [[result|Result]]
- [[box-t|Box<T>]]
- [[generics]]
- [[trait-bounds|trait bounds]]

## Code Examples

- [[thunk-type-alias|Thunk type alias]] — Listing 20-25, 20-26
- [[io-result-alias|std::io::Result alias]] — `Write` trait foydalanishi

## Exercises or Practice Ideas

1. **Type alias vs newtype:** `type Email = String` va `struct Email(String)` — ikkalasini implement qiling. `fn send(to: Email)` ga `String` argument berishga urining. Qaysi xato beradi?
2. **DST function:** `fn len_of<T: ?Sized + AsRef<str>>(t: &T) -> usize { t.as_ref().len() }` — `&str`, `String`, `Box<str>` bilan testlang.
3. **Diverging branches:** Funksiya yozing — argument bo'lsa qaytaradi, bo'lmasa `panic!`. `match` ichida `!` coercion'ni ishlating.
4. **Custom Result alias:** `MyError` enum yarating, `MyResult<T> = Result<T, MyError>` alias qiling, va `?` operatorni `From` impl bilan ishlating.
5. **Box<dyn Trait>:** `dyn Animal` trait object'larini `Vec<Box<dyn Animal>>` ichida saqlang, `iter().for_each(|a| a.sound())` chaqirib testlang.
6. **`!` from `loop`:** `fn server() -> !` yozing, `loop { handle_request() }` bilan. Kompilyator OK qabul qilishini ko'ring.

## Questions Raised

- `!` tip qachon stable bo'ladi (hozir nightly'da `Never`/`Infallible` orqali)?
- Custom DST (`struct Wrapper { data: [u8] }`) qachon foydali?
- `?Sized` bilan `?Send` parallel bo'lishi mumkinmi (kompilyator dizayn qarori)?
- Type alias generic'larda lifetime parameter saqlanadimi?
- `Box<str>` vs `String` — qaysi joyda biri ikkinchisidan yaxshiroq?

## Links To Update

- [[index|Rust Wiki Index]] — yangi sources, concept'lar, examples
- [[overview]] — 20.3 synthesis
- [[wiki/chapters/20-3-advanced-types|Chapter 20.3]]
- [[newtype-pattern]] — type alias bilan taqqoslash
- [[string-slice]] — DST kontekstida
- [[trait-object]] — DST kontekstida
- [[match]] — `!` coercion havolasi
- [[loop]] — break'siz `loop` `!`
- [[unwrap]] — `!` ishlatadigan implementatsiya
- [[generics]] — `?Sized` cheklovi
