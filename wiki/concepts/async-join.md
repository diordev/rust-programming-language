---
title: "Async Join"
type: concept
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, concurrency]
source_count: 1
---

# Async Join

## Short Definition

`trpl::join` va `trpl::join!` — bir nechta future'ni **birgalikda** kutish vositalari. Barchasi tugagach natija qaytaradi. Thread'dagi `.join()` ga o'xshash, lekin blocking emas.

## Why It Matters

Async concurrencyning asosiy qurilish bloki. `join` bo'lmasa future'larni ketma-ket `await` qilib, concurrencydan foydalanmagan bo'lardingiz. `join` esa bitta `await` bilan N ta ishni parallel bajaradi.

## Mental Model

`join` — restorandagi buffer: barcha stol (future) buyurtma qilganida servis boshlanadi. `select` — birinchi kelgan servis qilinadi (`join` emas). `join` — hammasi tayyor bo'lganda davom etiladi.

`spawn_task` bilan farq:
- `spawn_task` — mustaqil task; main blok unga e'tibor bermaydi
- `join` — ikkalasi birgalikda bitta composite future; ikkalasi tugashini aniq kutadi

## Syntax and Examples

### trpl::join — ikki future

```rust
let fut1 = async {
    for i in 1..10 {
        println!("first: {i}");
        trpl::sleep(Duration::from_millis(500)).await;
    }
};

let fut2 = async {
    for i in 1..5 {
        println!("second: {i}");
        trpl::sleep(Duration::from_millis(500)).await;
    }
};

trpl::join(fut1, fut2).await;
// Output tuple: ((), ()) — ikkalasi tugaganda
```

`join` ikkala output'dan tuzilgan `(A, B)` tuple qaytaruvchi yangi future yaratadi.

### trpl::join! — N ta future (makro)

```rust
let fut_a = async { /* ... */ };
let fut_b = async { /* ... */ };
let fut_c = async { /* ... */ };

trpl::join!(fut_a, fut_b, fut_c);
// compile time'da future soni ma'lum bo'lishi kerak
```

`join!` makro — ixtiyoriy sondagi future'lar uchun, ammo soni compile time'da belgilangan bo'lishi kerak.

### Fair scheduling

`trpl::join` **fair** — ikkala future'ni teng tez-tez tekshiradi:

```rust
// Deterministik order:
// first: 1, second: 1, first: 2, second: 2, ...
trpl::join(counting_1, counting_2).await;
```

Thread scheduler bunday kafolatni bermaydi — OS ixtiyoriy qaror qiladi. `join` esa algorithmic fairness ta'minlaydi.

### join vs ketma-ket await

```rust
// BU CONCURRENT EMAS — ketma-ket:
fut1.await;
fut2.await;

// BU CONCURRENT — birgalikda:
trpl::join(fut1, fut2).await;
```

### join_all — noma'lum sondagi future'lar

```rust
let futures: Vec<_> = urls.iter().map(|u| fetch(u)).collect();
futures::future::join_all(futures).await;
// Vec<T> qaytaradi
```

## Common Mistakes

- **Individual await vs join**: `fut1.await; fut2.await;` — ketma-ket, concurrent emas.
- **join vs join!**: `trpl::join` ikki argument, `trpl::join!` N ta argument.
- **Output tuple**: `join(a, b).await` natijasi `(A_output, B_output)` — agar output kerak bo'lsa destructure qiling.

## Related Concepts

- [[async-task|async task]] — `spawn_task` bilan yaratilgan mustaqil task
- [[future|Future trait]] — join yangi composite future yaratadi
- [[async-runtime|async runtime]] — fair scheduling ta'minlaydi
- [[concurrency]] — join'ning mohiyati concurrent bajarilish

## Sources

- [[wiki/sources/17-2-applying-concurrency-with-async]]
