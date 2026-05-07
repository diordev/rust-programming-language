---
title: "Release Build & Release Profiles"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, cargo, release-profiles, opt-level, performance]
source_count: 3
---

# Release Build & Release Profiles

## Short Definition

Cargo'da ikki asosiy **release profile**: `dev` (tez compile, slow runtime) va `release` (slow compile, tez runtime). `Cargo.toml`'da `[profile.*]` bo'limlari orqali sozlanadi.

## Common Commands

```shell
$ cargo build            # dev profil
$ cargo build --release  # release profil → target/release/
```

## Notes

### opt-level — asosiy farq

| Profil | opt-level | Xususiyat |
|--------|-----------|-----------|
| `dev` | 0 | Tez compile, debug uchun |
| `release` | 3 | To'liq optimallashtirish |

### Profil sozlash (`Cargo.toml`)

```toml
[profile.dev]
opt-level = 1        # default 0 o'rniga biroz ko'proq optimallashtirish

[profile.release]
opt-level = 3        # default (odatda o'zgartirilmaydi)
panic = 'abort'      # stack unwind o'rniga to'xtatish — binary kichikroq
```

- Faqat o'zgartirilgan parametrlar yoziladi — qolganlari Cargo default'ida qoladi
- `opt-level` 0–3 oralig'ida: 3 = to'liq optimallashtirish
- Barcha profil parametrlari: [Cargo documentation](https://doc.rust-lang.org/cargo/reference/profiles.html)

### Output joyi

- `dev`: `target/debug/`
- `release`: `target/release/`

## Related Pages

- [[cargo|Cargo]]
- [[compiler]]
- [[cargo-toml|Cargo.toml]]
- [[panic|panic!]]
- [[abort-on-panic|abort on panic]]
- [[zero-cost-abstractions]]

## Sources

- [[1-3-hello-cargo-the-rust-programming-language]]
- [[9-1-unrecoverable-errors-with-panic-the-rust-programming-language]]
- [[14-1-customizing-builds-with-release-profiles-the-rust-programming-language|14.1 Release Profiles]]
