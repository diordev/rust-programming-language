---
title: "IP Address Enum Example"
type: example
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, example, enums]
source_count: 1
---

# IP Address Enum Example

## Goal

IP address kindi va data'sini avval struct + enum bilan, keyin variantlar ichida data tutadigan enum bilan, oxirida har xil shape'dagi variantlar bilan modellashtirish — [[enums]]ning struct ustidagi afzalliklarini ko'rsatadi.

## Version 1: Datasiz Enum

```rust
enum IpAddrKind {
    V4,
    V6,
}

fn route(ip_kind: IpAddrKind) {}

fn main() {
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;

    route(IpAddrKind::V4);
    route(IpAddrKind::V6);
}
```

`IpAddrKind` faqat *qaysi turdagi* address ekanini bildiradi, lekin haqiqiy address data'sini saqlamaydi.

## Version 2: Struct + Enum

```rust
enum IpAddrKind {
    V4,
    V6,
}

struct IpAddr {
    kind: IpAddrKind,
    address: String,
}

let home = IpAddr {
    kind: IpAddrKind::V4,
    address: String::from("127.0.0.1"),
};

let loopback = IpAddr {
    kind: IpAddrKind::V6,
    address: String::from("::1"),
};
```

Bu ishlaydi, lekin "kind va address bir-biriga bog'liq" qoidasi struct field'lari orqali implicit; type system uni majburlay olmaydi.

## Version 3: Variant Ichida Data

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));
```

Endi alohida struct kerak emas. Variant nomi automatically constructor function bo'ldi: `IpAddr::V4` — `String`dan `IpAddr` yasovchi funksiya.

## Version 4: Variantlar Har Xil Shape'da

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

V4 — to'rt sonli, V6 — string. Struct bilan bunday qilib bo'lmaydi: bitta `address` field har ikkala shape'ni qabul qila olmaydi.

## Version 5: Standard Library Style

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Standard library `IpAddr` aynan shu pattern'dan foydalanadi: variant ichida har biriga moslashtirilgan struct. Enum variantlari *istalgan* turdagi data — primitive, struct, hatto boshqa enum — tashishi mumkin.

## Related Concepts

- [[enums]]
- [[enum-variants|enum variants]]
- [[constructor-functions|constructor functions]]
- [[structs]]
- [[standard-library|standard library]]

## Sources

- [[6-1-defining-an-enum-the-rust-programming-language]]
