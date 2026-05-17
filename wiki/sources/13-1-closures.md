---
title: "13.1. Closures: Anonymous Functions that Capture Their Environment"
type: source
status: active
created: 2026-05-07
updated: 2026-05-07
tags: [rust, closures, fn-traits, capture, move]
source_count: 1
---

# 13.1. Closures: Anonymous Functions that Capture Their Environment

## Source

- URL: https://doc.rust-lang.org/stable/book/ch13-01-closures.html
- Kitob: The Rust Programming Language
- Bob: 13.1

## Detailed Summary

### Closures nima?

[[closures]] — o'zgaruvchida saqlanishi yoki boshqa funksiyaga argument sifatida uzatilishi mumkin bo'lgan anonim funksiyalar. Odatiy `fn` funksiyalardan asosiy farqi: closures o'zlari **aniqlangan scope'dagi qiymatlarni capture** qila oladi. Funksiyalar bu imkoniyatga ega emas.

### Capture: muhitdan qiymat olish

Misol: `giveaway` metodi `unwrap_or_else(|| self.most_stocked())` — bu closure `self` ga immutable reference qilib capture qiladi. Standard library `Inventory` yoki `ShirtColor` haqida hech narsa bilmaydi, biroq closure bu ma'lumotlarni olib kirdi.

```rust
fn giveaway(&self, user_preference: Option<ShirtColor>) -> ShirtColor {
    user_preference.unwrap_or_else(|| self.most_stocked())
}
```

### Type inference va annotatsiya

Closures uchun tip annotatsiyasi odatda shart emas — compiler narrow contextdan inferensiya qiladi. Lekin explicit yozish ham mumkin:

```rust
fn  add_one_v1   (x: u32) -> u32 { x + 1 }   // funksiya
let add_one_v2 = |x: u32| -> u32 { x + 1 };  // annotatsiyali closure
let add_one_v3 = |x|             { x + 1 };  // inference
let add_one_v4 = |x|               x + 1  ;  // bir expression — {} shart emas
```

**Muhim:** Birinchi chaqiruvda tip qotib qoladi. Bir closure'ni `String` bilan, keyin `i32` bilan chaqirib bo'lmaydi — **E0308** chiqadi.

### Capture modlari

Closure body'si nima qilishiga qarab uch xil capture:

| Mod | Qachon | Misol |
|-----|--------|-------|
| Immutable borrow (`&T`) | Faqat o'qish | `\|\| println!("{list:?}")` |
| Mutable borrow (`&mut T`) | O'zgartirish | `\|\| list.push(7)` |
| Ownership (`move`) | Thread yoki owneri zarur | `move \|\| println!("{list:?}")` |

Mutable borrow closure aniqlangandan chaqirilgunicha ochiq turadi — o'sha oralig'da boshqa immutable borrow bo'lmaydi.

`move` keyword closure'ni multithreading uchun tayyor qiladi: agar main thread bitirib `list`'ni drop qilsa, thread ichidagi reference invalid bo'lib qolardi. `move` buni oldini oladi.

```rust
thread::spawn(move || println!("From thread: {list:?}"))
    .join().unwrap();
```

### Fn traits (qanday chaqirilishi mumkin)

Closure body nima qilishiga qarab bir, ikki, yoki uchta `Fn` trait implement qilinadi (additive — yuqori qo'shiladi):

| Trait | Qo'llanilishi | Capture qoidasi |
|-------|--------------|-----------------|
| `FnOnce` | Faqat bir marta chaqirish | Captured qiymatni tashqariga move qiladi |
| `FnMut` | Bir necha marta, mutatsiya bo'lishi mumkin | Move qilmaydi, lekin mutatsiya qilishi mumkin |
| `Fn` | Bir necha marta, shared borrow bilan chaqiriladi | Ne move, ne mutatsiya |

Barcha closures kamida `FnOnce` implement qiladi. Bu jadval thread-safety jadvali emas; boshqa thread bilan ishlash uchun qo'shimcha `Send`/`Sync`/`'static` boundlar kerak bo'lishi mumkin.

`unwrap_or_else` → `FnOnce` (eng moslashuvchan, f ko'pi bilan bir marta chaqiriladi)
`sort_by_key` → `FnMut` (har element uchun bir marta chaqiriladi)

### FnOnce vs FnMut: xato misoli

```rust
// XATO: value closure'dan move qilinmoqda, FnMut bo'la olmaydi
list.sort_by_key(|r| {
    sort_operations.push(value); // value tashqariga move bo'ladi
    r.width
});
```

`E0507: cannot move out of value, a captured variable in an FnMut closure`

To'g'ri yechim — mutable counter:

```rust
let mut num_sort_operations = 0;
list.sort_by_key(|r| {
    num_sort_operations += 1; // mutable reference, move yo'q
    r.width
});
```

## Key Concepts

- [[closures]]
- [[fn-traits]] (`FnOnce`, `FnMut`, `Fn`)
- [[move-semantics]] (`move` keyword closures bilan)
- [[borrowing]] (capture modlari ownership qoidalariga bo'ysunadi)
- [[type-inference]] (closure type inference)
- [[concurrency]] (`move` closures thread uchun)

## Code Examples

- Listing 13-1: Shirt company giveaway — closure environment'dan capture
- Listing 13-2: Type annotatsiyali closure
- Listing 13-4..5: Immutable va mutable borrow capture
- Listing 13-6: `move` closure thread'da
- Listing 13-7..9: `sort_by_key` bilan `FnMut` va `FnOnce` farqi

## Exercises or Practice Ideas

- `FnOnce`, `FnMut`, `Fn` qabul qiluvchi funksiyalar yozing
- `move` keyword bilan va usiz thread closureni solishtirib ko'ring
- `sort_by_key` uchun o'z closure'ingizni yozing

## Questions Raised

- Closure'ni `Box<dyn Fn()>` bilan dynamic dispatch qilish qanday ishlaydi?
- Async/await bilan closures qanday o'zgaradi?

## Links To Update

- [[closures]]
- [[fn-traits]]
- [[13-functional-language-features]] chapter
