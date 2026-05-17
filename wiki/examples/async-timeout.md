---
title: "Async Timeout"
type: example
status: active
created: 2026-05-08
updated: 2026-05-08
tags: [rust, async, example]
source_count: 1
---

# Async Timeout

## Description

`trpl::select` va `trpl::sleep` yordamida custom `timeout` funksiyasini qurilishi — kichik async building block'lardan katta abstraktsiya compose qilish namunasi.

## Source

[[wiki/sources/17-3-working-with-any-number-of-futures]] — Listing 17-18, 17-19, 17-20.

## To'liq implementatsiya

```rust
use std::time::Duration;
use std::future::Future;
use trpl::Either;

async fn timeout<F: Future>(
    future_to_try: F,
    max_time: Duration,
) -> Result<F::Output, Duration> {
    match trpl::select(future_to_try, trpl::sleep(max_time)).await {
        Either::Left(output) => Ok(output),
        Either::Right(_)     => Err(max_time),
    }
}
```

## Ishlatish

```rust
fn main() {
    trpl::block_on(async {
        let slow = async {
            trpl::sleep(Duration::from_secs(5)).await;
            "Finally finished"
        };

        match timeout(slow, Duration::from_secs(2)).await {
            Ok(message) => println!("Succeeded with '{message}'"),
            Err(duration) => {
                println!("Failed after {} seconds", duration.as_secs())
            }
        }
    });
}
// Output: Failed after 2 seconds
```

## Qanday ishlaydi

```
timeout(slow_fut, 2s)
  ↓
trpl::select(slow_fut, sleep(2s))
  ↓
  sleep(2s) birinchi tugaydi → Either::Right(())
  ↓
Err(2s)
```

`trpl::select` birinchi argumentni birinchi poll qiladi — shuning uchun `future_to_try` birinchi o'rinda. Bu very short timeout holatlarda ham future'ga imkon beradi.

## Kengaytirish: retry bilan

```rust
async fn timeout_with_retry<F, Fut>(
    f: impl Fn() -> Fut,
    max_time: Duration,
    retries: u32,
) -> Result<Fut::Output, Duration>
where
    Fut: Future,
{
    for _ in 0..retries {
        if let Ok(result) = timeout(f(), max_time).await {
            return Ok(result);
        }
    }
    Err(max_time)
}
```

## Key Observations

- `timeout` o'zi `async fn` — awaitlanishi mumkin
- Generic `F: Future` — istalgan future bilan ishlaydi
- `F::Output` — inner future'ning return type'i
- Composability: `timeout` + `retry` → yana kattaroq abstraktsiya

## Concepts Used

- [[future|Future trait]] — `F: Future`, `F::Output`
- [[async-await|async/await]] — async fn timeout
- [[async-runtime|async runtime]] — select va sleep bilan raqobat

## Related Pages

- [[wiki/chapters/17-3-working-with-any-number-of-futures]]
- [[async-page-scraper]] — select'ning birinchi ishlatilishi (17.1)
