---
title: "Stream from Iterator"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, streams, example]
source_count: 1
---

# Stream from Iterator

## Description

`trpl::stream_from_iter` bilan iterator'dan stream yaratish va `while let Some(v) = stream.next().await` pattern'i bilan iteratsiya.

## Source

[[wiki/sources/17-4-streams-futures-in-sequence]] — Listing 17-21, 17-22.

## Kod

```rust
use trpl::StreamExt;  // next() metodi uchun zarur

fn main() {
    trpl::block_on(async {
        let values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
        let iter = values.iter().map(|n| n * 2);
        let mut stream = trpl::stream_from_iter(iter);  // mut kerak

        while let Some(value) = stream.next().await {
            println!("The value was: {value}");
        }
    });
}
// Output:
// The value was: 2
// The value was: 4
// The value was: 6
// ... (20 gacha)
```

## Muhim jihatlar

**`StreamExt` import zarur**: `next()` metodi `StreamExt`da. Yo'q bo'lsa:
```
error[E0599]: no method named `next` found for struct `Iter`
help: the following traits provide `next`:
1  + use crate::trpl::StreamExt;
```

**`stream` mut bo'lishi kerak**: Iterator kabi `next()` chaqiruvi stream ichki holatini o'zgartiradi.

**`while let Some` pattern**: `for v in stream` hali async iteratorlar bilan ishlamaydi — xuddi async channel'da bo'lgani kabi `while let` kerak.

## Iterator va Stream solishtirish

```rust
// Iterator (sinxron):
for value in iter {
    println!("{value}");
}

// Stream (asinxron):
while let Some(value) = stream.next().await {
    println!("{value}");
}
```

## Concepts Used

- [[stream|Stream trait]] — StreamExt, next method
- [[iterators]] — stream'ning sinxron analogi
- [[async-await|async/await]] — stream.next().await

## Related Pages

- [[wiki/chapters/17-4-streams-futures-in-sequence]]
- [[async-channel-messages]] — `while let Some(v) = rx.recv().await` — xuddi shu pattern
