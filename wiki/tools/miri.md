---
title: "Miri"
type: tool
status: active
created: 2026-05-08
updated: 2026-05-16
tags: [rust, unsafe, tool, debugging]
source_count: 3
---

# Miri

## Maqsadi

**Miri** — Rust uchun rasmiy [[undefined-behavior|undefined behavior]] (UB) detektori. Kodingiz ishlatilayotganda Rust qoidalarini buzilishini aniqlaydi.

## Static vs Dynamic Analysis

| | Borrow Checker | Miri |
|---|----------------|------|
| Vaqt | Compile-time | Runtime |
| Tabiati | Static | Dynamic |
| Topadigan narsa | Borrow qoidalari, type errors | UB, memory bugs |

Miri — interpreter: kodingizni virtual mashin'da ishlatadi va har xotira operatsiyasini tekshiradi.

Backend pointer source unsafe code yozilganda Miri'ni maxsus tavsiya qiladi. Bu to'g'ri signal: raw pointerlar va qo'lda aliasing ishlatilgan joylar Miri uchun birinchi kandidat.

## O'rnatish

Nightly toolchain talab qiladi:

```shell
rustup +nightly component add miri
```

Bu sizning loyihangiz versiyasini o'zgartirmaydi — faqat asboblarni qo'shadi.

## Foydalanish

```shell
cargo +nightly miri run            # binary'ni Miri ostida
cargo +nightly miri test           # testlarni Miri ostida
```

### Misol

`Listing 20-7` (ixtiyoriy adresga slice yaratish) Miri'da:

```text
error: Undefined Behavior: pointer not dereferenceable: pointer must be
dereferenceable for 40000 bytes, but got 0x1234[noalloc] which is a
dangling pointer (it has no provenance)
```

Miri xato joyni aniq ko'rsatadi va sabab haqida hint beradi.

## Miri Nimani Topa Oladi

- Use-after-free
- Out-of-bounds memory access
- Aliasing rules buzilishi (Stacked Borrows)
- Uninitialized memory o'qish
- Invalid type values (e.g., `bool` ichida 2)
- Misaligned pointer access
- Memory leaks (ba'zi rejimlarda)

## Cheklovlar

- **Faqat ishlatilgan kodni tekshiradi.** Test coverage past bo'lsa, ko'p bug'lar topilmaydi.
- **Tashqi C kod (FFI) ishlay olmaydi.** Miri C funksiyalarini interpret qilmaydi.
- **Sekin.** Native ishlashdan ~10-100x sekin.
- **Nightly faqat.** Stable Rust'da yo'q.
- Nightly talab qilishi Rustning [[release-channels]] modeli va unstable tooling policy'si bilan bog'liq.

## Foydali Flag'lar

```shell
# Strict provenance — pointer manipulation rules qattiqroq
MIRIFLAGS="-Zmiri-strict-provenance" cargo +nightly miri run

# Permissive provenance — integer-to-pointer cast warninglarini o'chirish
MIRIFLAGS="-Zmiri-permissive-provenance" cargo +nightly miri run

# Backtrace
MIRIFLAGS="-Zmiri-backtrace=full" cargo +nightly miri run
```

## Common Mistakes

- **Miri "topmadi" deb to'liq ishonish.** UB topilmasligi, UB yo'q deganini bildirmaydi — kod yo'lining ishlatilmaganini bildiradi.
- **Miri'ni faqat panic'da ishlatish.** Profilaktik ishlatish (CI'da) ko'p bug'lar oldini oladi.
- **Permissive provenance bilan Strict natijalarini qabul qilish.** Strict — qattiqroq, "haqiqat" tomonga yaqinroq.

## Manbalar

- [Miri GitHub repository](https://github.com/rust-lang/miri)
- [Rustonomicon](https://doc.rust-lang.org/nomicon/) — unsafe Rust uchun chuqur qo'llanma

## Related

- [[unsafe-rust|unsafe Rust]]
- [[undefined-behavior|undefined behavior]]
- [[raw-pointer|raw pointer]]
- [[rustup]] — toolchain manager
- [[compiler]]
- [[nightly-rust]]
- [[release-channels]]

## Sources

- [[wiki/sources/20-1-unsafe-rust|20.1 Unsafe Rust]]
- [[wiki/sources/22-7-g-how-rust-is-made-and-nightly-rust|22.7. G - How Rust is Made and Nightly Rust]]
- [[wiki/sources/rust-for-backend-developers-pointers]]
