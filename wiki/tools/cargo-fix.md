---
title: "cargo fix / rustfix"
type: tool
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, tool, cargo, rustfix]
source_count: 2
---

# cargo fix / rustfix

## Short Definition

`cargo fix` compiler suggestion'lariga tayangan holda warning fix'larni avtomatik qo'llaydi; Appendix D bu capability'ni `rustfix` tooli bilan bog'laydi.

## Common Commands

```shell
$ cargo fix
```

## Notes

- `unused_mut` kabi clear compiler suggestion'lar uchun juda qulay.
- Appendix E `cargo fix`ni edition migration workflow bilan ham bog'laydi.
- Bu tool semantic refactordan ko'ra suggestion-driven mechanical cleanup uchun mosroq.

## Related Pages

- [[cargo|Cargo]]
- [[compiler]]
- [[edition-migration]]
- [[cargo-fix-edition-migration]]
- [[cargo-fix-unused-mut-example|cargo fix unused mut example]]

## Sources

- [[wiki/sources/22-4-d-useful-development-tools|22.4]]
- [[wiki/sources/22-5-e-editions|22.5]]
