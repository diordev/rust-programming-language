---
title: "Cargo.lock"
type: tool
status: active
created: 2026-05-06
updated: 2026-05-07
tags: [rust, cargo]
source_count: 2
---

# Cargo.lock

## Short Definition

`Cargo.lock` Cargo resolved dependency versionsini exact saqlaydigan lock file.

## Common Commands

```shell
$ cargo build
$ cargo update
```

## Notes

- `Cargo.toml` version constraintsni belgilaydi.
- `Cargo.lock` reproducible builds uchun tanlangan exact versionsni saqlaydi.
- Cargo bu file'ni avtomatik boshqaradi.

## Related Pages

- [[cargo|Cargo]]
- [[cargo-toml|Cargo.toml]]
- [[dependencies]]
- [[semver|SemVer]]

## Sources

- [[1-3-hello-cargo]]
- [[wiki/sources/2-programming-a-guessing-game]]
