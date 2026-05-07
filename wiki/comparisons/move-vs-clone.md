---
title: "Move vs Clone"
type: comparison
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, ownership]
source_count: 1
---

# Move vs Clone

## Short Version

Move ownershipni o'tkazadi va oldingi bindingni invalid qiladi. Clone heap data'ni explicit deep copy qiladi va ikkala binding valid qoladi.

## Comparison

| Feature | Move | Clone |
| --- | --- | --- |
| Syntax | `let s2 = s1;` | `let s2 = s1.clone();` |
| Heap data | Copy qilinmaydi | Copy qilinadi |
| Old binding | Invalid bo'ladi | Valid qoladi |
| Cost signal | Cheap metadata move | Potentially expensive |
| Purpose | Single ownerni saqlash | Independent owned copy yaratish |

## Example

```rust
let s1 = String::from("hello");
let s2 = s1;
// s1 no longer valid
```

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {s1}, s2 = {s2}");
```

## Related Pages

- [[move-semantics|move semantics]]
- [[copy-trait|Copy trait]]
- [[string-type|String]]
- [[e0382-borrow-of-moved-value|E0382 borrow of moved value]]
