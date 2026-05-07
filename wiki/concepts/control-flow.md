---
title: "Control Flow"
type: concept
status: active
created: 2026-05-06
updated: 2026-05-06
tags: [rust, control-flow]
source_count: 3
---

# Control Flow

## Short Definition

Control flow program qaysi code blockni va necha marta bajarishini boshqaradi.

## Why It Matters

Branching va repetition har bir non-trivial programda kerak. Rustda asosiy constructs: `if`, `match`, `if let`, `let...else`, `loop`, `while`, `for`, `break`, `continue`.

## Mental Model

- `if`: bool conditionga qarab branch.
- `match`: value shape yoki variantiga qarab exhaustive branch.
- `if let`: bitta pattern mos kelsa branch.
- `let...else`: pattern mos kelmasa early exit, mos kelsa happy path.
- `loop`: explicit stop bo'lmaguncha takrorlash.
- `while`: condition true bo'lgancha takrorlash.
- `for`: collection yoki range bo'ylab iteration.

## Syntax and Examples

```rust
if number < 5 {
    println!("condition was true");
} else {
    println!("condition was false");
}
```

```rust
for number in (1..4).rev() {
    println!("{number}!");
}
```

```rust
match value {
    Some(n) => println!("{n}"),
    None => println!("nothing"),
}
```

```rust
if let Some(n) = value {
    println!("{n}");
}
```

```rust
let Some(n) = value else {
    return;
};
```

## Common Mistakes

- `if` conditionga non-bool value berish.
- `if` expression arms turli type qaytarishi.
- Collection iterationda `while` + manual index bilan bounds mistakes qilish.
- `match`da casesni exhaustive yopmaslik.
- `if let`ni exhaustive handling kerak bo'lgan joyda ishlatish.

## Related Concepts

- [[if-expressions|if expressions]]
- [[match]]
- [[if-let|if let]]
- [[let-else|let...else]]
- [[exhaustive-matching|exhaustive matching]]
- [[loop]]
- [[while-loop|while loop]]
- [[for-loop|for loop]]

## Sources

- [[3-5-control-flow-the-rust-programming-language]]
- [[6-2-the-match-control-flow-construct-the-rust-programming-language]]
- [[6-3-concise-control-flow-with-if-let-and-let-else-the-rust-programming-language]]
