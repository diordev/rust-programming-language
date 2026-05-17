---
title: "Union Type"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, unsafe, ffi, types]
source_count: 1
---

# Union Type

## Short Definition

`union` — `struct` ga o'xshash, lekin **bir vaqtning o'zida faqat bitta** field'ga qiymat qo'yiladi. Barcha field'lar bir xil xotira manzilini bo'lishadi. Asosan C interop uchun.

## Why It Matters

C tilidagi `union` mexanizmini Rust'da ifodalash uchun. Sof Rust kodida deyarli ishlatilmaydi — `enum` (Rust'ning tagged union) ko'p hollarda ehtiyojni qondiradi.

## Mental Model

```
struct  →  hamma field'lar bir vaqtda qiymatga ega, alohida xotira
enum    →  bitta variant aktiv, kompilyator tag saqlaydi (turini biladi)
union   →  bitta field aktiv, lekin kompilyator qaysi biri ekanligini bilmaydi
```

Field'ga kirish (`u.field`) xavfli — chunki kompilyator hozirgi paytda qaysi field "to'g'ri" interpretatsiya ekanligini bilmaydi.

## Syntax and Examples

```rust
#[repr(C)]
union MyUnion {
    f1: u32,
    f2: f32,
}

fn main() {
    let u = MyUnion { f1: 1 };

    unsafe {
        let v = u.f1;       // u32 sifatida o'qish — OK chunki f1 yozilgan
        // let bad = u.f2;  // f32 sifatida o'qish — bit pattern interpretatsiya
    }
}
```

### C bilan integratsiya

```c
// C tomondagi struct
typedef union {
    int i;
    char bytes[4];
} ConvertU;
```

```rust
#[repr(C)]
union ConvertU {
    i: i32,
    bytes: [u8; 4],
}
```

`#[repr(C)]` — C layout konventsiyasiga rioya qilish uchun.

## Common Mistakes

- **Sof Rust kodida `union` ishlatish.** Aksariyat hollarda `enum` to'g'riroq tanlov.
- **Yozilgan field'dan boshqasini o'qish.** Bit pattern interpretatsiya — UB bo'lmasligi kerak (sodda integer/float uchun OK), lekin `String`/`Vec` kabi `Drop` implement qilgan turlar uchun xavfli.
- **`Drop`'li turlarni union ichida ishlatish.** `union` `Drop` ni avtomatik chaqirmaydi (qaysi field aktiv ekanini bilmaydi). Agar `Drop` impl'i bo'lgan turlar bo'lsa — `ManuallyDrop<T>` ishlatish kerak.

## Related Concepts

- [[unsafe-rust|unsafe Rust]] — field access faqat `unsafe`
- [[unsafe-superpowers|unsafe superpowers]] — superpower #5
- [[ffi|FFI]] — C union'lar uchun
- [[enums]] — Rust idiomatic alternativ (tagged union)
- [[structs]] — taqqoslash

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
