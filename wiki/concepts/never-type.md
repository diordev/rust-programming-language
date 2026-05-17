---
title: "Never Type (!)"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, types, control-flow]
source_count: 3
---

# Never Type (`!`)

## Short Definition

`!` — qiymatga ega bo'la olmaydigan **empty type** (type theory'da). Funksiya hech qachon qaytmasligini ifodalaydi. Type theory'da: `!` har tipga **coerce** bo'la oladi.

## Why It Matters

Hech qachon qaytmaydigan funksiyalar mavjud: `panic!()`, `loop {}` (break'siz), `process::exit()`. Bunday funksiyalarning return tipini `!` bilan belgilaymiz. Bu type system'ga foydali signal beradi: `match` arm'lari turli tipda bo'la oladi, agar ulardan bir-ikkitasi `!` bo'lsa.

Backend beginner source bu type'ni erta tilga oladi, lekin ataylab chuqur ochmaydi. Bu to'g'ri yondashuv: `!` ko'pincha avval "kompilyator ishlatadigan g'alati type" bo'lib ko'rinadi, keyin expression va control-flow materiallari yig'ilgach tabiiylashadi.

## Mental Model

`!` — "shu nuqtaga hech qachon yetib kelmaydi." Kompilyator buni biladi va boshqa arm'larning tipini ishlatadi.

```
match x {
    Ok(n)  => n,         // u32
    Err(_) => continue,  // !  →  u32 ga coerce
}
// natija tipi: u32
```

`return value;` ham expression kontekstida `!` sifatida ishlaydi, chunki u current functionga boshqaruvni qaytarmaydi.

## Diverging Functions

Return tipi `!` bo'lgan funksiya — *diverging function*:

```rust
fn bar() -> ! {
    panic!("bu funksiya hech qachon qaytmaydi");
}

fn forever() -> ! {
    loop {
        println!("...");
    }
}
```

Standart kutubxonadagi misollar:
- `panic!()` — `!`
- `process::exit(code)` — `!`
- `unimplemented!()`, `todo!()`, `unreachable!()` — `!`
- `loop { }` (break'siz) — `!`

## `match` da `!` ning Roli

`match` barcha arm'lari bir xil tipga ega bo'lishi kerak:

```rust
let guess = match guess.trim().parse() {
    Ok(num) => num,             // u32
    Err(_)  => continue,        // !  →  u32 ga coerce qiladi
};
```

`continue` `!` qaytaradi (loop boshiga qaytadi, `guess`'ga qiymat beriladi emas). Kompilyator: "`!` har tipga coerce bo'ladi, demak `Err` arm tipi `u32`. Natija — `u32`."

Agar `Err` arm `"hello"` qaytarsa — bu xato, chunki ikki arm tipi turlicha:

```rust
let bad = match parse() {
    Ok(_)  => 5,        // i32
    Err(_) => "hello",  // &str   ← E0308: mismatched types
};
```

## `unwrap` Implementatsiyasidagi `!`

```rust
impl<T> Option<T> {
    pub fn unwrap(self) -> T {
        match self {
            Some(val) => val,                                       // T
            None => panic!("called `Option::unwrap()` on a None"),  // !
        }
    }
}
```

Ikki arm: `T` va `!`. `!` `T`'ga coerce → match natijasi `T`. Mantiqiy: `unwrap` `None` holatda `T` qaytarmaydi (panic — program tugaydi).

## `loop` ekspressiyasi

```rust
fn forever() -> ! {
    print!("forever ");
    loop {
        print!("and ever ");
    }
}
```

`loop {}` (break'siz) — `!` ifodasi. `break value` qo'shsa — value tipi.

## `!` Qiymat Yarata Olmaslik

`!` tip `0 ta` qiymatga ega — siz uni yarata olmaysiz:

```rust
let x: ! = ???   // mumkin emas
```

Faqat diverging operatsiyalar (`panic!`, `loop`, `continue`, `break`, `return`) `!` "qaytaradi."

## `!` ning Boshqa Foydalanishlari

```rust
// Conditional kompilyatsiya
fn might_diverge() -> ! {
    if cfg!(debug_assertions) {
        panic!("debug mode");
    } else {
        loop { /* server */ }
    }
}

// Async cancellation pattern
async fn run_forever() -> ! {
    loop {
        do_work().await;
    }
}
```

## Common Mistakes

- **`!` qiymat saqlash deb o'ylash.** Hech qanday qiymat yo'q.
- **`-> !` qaytaruvchi funksiyaga `return value;` yozish.** Kompilyator xato beradi — `!` qaytarish mumkin emas.
- **`break` bilan `loop` `!` deb hisoblash.** `break` bo'lsa loop `()` yoki `break value` tipini qaytaradi, `!` emas.

## Related Concepts

- [[panic]] — eng keng tarqalgan `!` manbai
- [[match]] — `!` arm coercion
- [[loop]] — break'siz `loop` `!` qaytaradi
- [[control-flow|control flow]]
- [[unwrap]] — `!` ishlaydigan klassik misol
- [[diverging-functions|diverging functions]] — `-> !` qaytaruvchi funksiyalar
- [[unit-type|unit type]] (`()`) — taqqoslash uchun

## Sources

- [[wiki/sources/20-3-advanced-types|20.3 Advanced Types]]
- [[wiki/sources/rust-for-backend-developers-primitive-types]]
- [[wiki/sources/rust-for-backend-developers-functions]]
