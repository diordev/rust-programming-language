---
title: "const va static o'rtasidagi farq"
type: question
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, question, const, static]
source_count: 2
---

# const va static o'rtasidagi farq

## Answer

Qisqa javob:

- `const` — compile-time value; odatda inlined ishlatiladi.
- `static` — program xotirasida bitta aniq allocationga ega shared value.

Eng sodda mental model:

- `const` = "qiymatning formulasi"
- `static` = "xotirada bitta doimiy joy"

### `const` qanday ishlaydi

```rust
const LIMIT: u32 = 100;
```

`const` uchun alohida, yagona memory address bo'lishi shart emas. Rust Reference bo'yicha `const`lar asosan ishlatilgan joyiga qo'yib yuboriladi. Shu sababli bir xil `const`ga olingan reference'lar doim bir xil addressni ko'rsatadi deb kafolat yo'q.

`const` odatda quyidagilar uchun yaxshi:

- magic number o'rniga nomlangan qiymat;
- formula yoki limit;
- configga o'xshash immutable compile-time qiymat.

### `static` qanday ishlaydi

```rust
static APP_NAME: &str = "my-app";
```

`static` esa program ichida bitta allocation yaratadi. Shu `static`ga olingan reference'lar bitta shared joyga qaraydi. U butun dastur davomida yashaydi.

Bu `static`ni quyidagi holatlar uchun mos qiladi:

- katta immutable data;
- bitta shared address kerak bo'lganda;
- global counter yoki synchronization primitive kerak bo'lganda;
- FFI yoki global state bilan ishlaganda.

### Eng muhim amaliy farqlar

1. Memory modeli

```rust
const A: u32 = 7;
static B: u32 = 7;
```

- `A` alohida storagega ega bo'lishi shart emas.
- `B` bitta aniq storagega ega.

2. Address identity

`static`da "mana shu bitta object" degan g'oya bor. `const`da esa bu kafolat yo'q.

3. Lifetime

`static` itemning o'zi butun program davomida yashaydi. Shu sababli u bilan ishlaydigan reference'lar ko'pincha [[static-lifetime|'static lifetime]] bilan bog'lanadi.

Lekin ehtiyot bo'ling: `static` keyword va `'static` lifetime bir xil narsa emas.

- `static` keyword = item turi
- `'static` = lifetime tushunchasi

Masalan, string literal:

```rust
let s: &'static str = "hello";
```

Bu yerda `s` variable emas, reference type `'static` lifetimega ega. Bu `static` item e'lon qilinmadi.

4. Mutability

- `const` har doim immutable
- oddiy `static` ham immutable
- `static mut` esa mutable bo'lishi mumkin, lekin unga access `unsafe`

```rust
static mut COUNTER: u32 = 0;

unsafe {
    COUNTER += 1;
}
```

Nega `unsafe`? Chunki bu shared mutable global state bo'lib, [[data-race|data race]] va noto'g'ri parallel access xavfini oshiradi.

5. Drop behavior

`static` itemlar program tugaganda oddiy local variable kabi `drop` qilinmaydi. Rust Reference buni aniq aytadi. `const` esa ishlatilgan joydagi value sifatida yashaydi; agar u destructorga ega type bo'lsa, o'sha joy scope'dan chiqganda drop bo'lishi mumkin.

### Qachon qaysi birini tanlash kerak

Default tanlov: `const`.

`static`ga faqat quyidagi holatlarda o'ting:

- katta data saqlayapsiz;
- bitta shared address kerak;
- interior mutability yoki synchronization primitive kerak;
- FFI/global state kabi haqiqiy static storage kerak.

Amaliy qoida:

- limit, threshold, formula: `const`
- app-wide shared atomic/mutex/table: `static`

### `static`ning foydali real misoli

```rust
use std::sync::atomic::{AtomicUsize, Ordering};

fn next_id() -> usize {
    static COUNTER: AtomicUsize = AtomicUsize::new(0);
    COUNTER.fetch_add(1, Ordering::Relaxed)
}
```

Bu yerda `COUNTER` har call'da qayta yaratilmaydi. Hamma calllar bitta global counterni ko'radi. Shu narsani oddiy `const` bilan qilib bo'lmaydi.

### `static` nega ko'p boshlovchilarga g'alati tuyuladi

Chunki u oddiy variable emas. `let` local binding bo'lsa, `static` ko'proq global storage declaration. Uni "Rustdagi xavfsiz default yo'l" deb emas, "maxsus shared storage vositasi" deb tushunsangiz ancha oson bo'ladi.

## Evidence

- [[constants]]: `const` immutable, typed, compile-time value.
- [[static-items]]: `static` bitta allocationga ega shared storage beradi.
- [[static-lifetime|'static lifetime]]: `'static` lifetime va `static` keyword bir xil emas.
- [[variables-and-mutability|variables and mutability]]: `const` variable bindinglardan alohida turadi.
- [Rust Reference: Constant items](https://doc.rust-lang.org/stable/reference/items/constant-items.html)
- [Rust Reference: Static items](https://doc.rust-lang.org/stable/reference/items/static-items.html)

## Follow-up

- `static mut` o'rniga `AtomicUsize`, `Mutex`, `OnceLock`, `LazyLock` qachon ishlatilishini ko'rish.
- `const fn` va oddiy `fn` farqini ko'rish.
- `static` va [[static-lifetime|'static lifetime]]ni ko'proq misollar bilan ajratib mashq qilish.

## Related Pages

- [[constants]]
- [[static-items]]
- [[static-lifetime|'static lifetime]]
- [[variables-and-mutability|variables and mutability]]
- [[ownership]]
- [[data-race|data race]]
