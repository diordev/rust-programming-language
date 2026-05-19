---
title: "async user address composition"
type: example
status: active
created: 2026-05-19
updated: 2026-05-19
tags: [rust, async, backend, example]
source_count: 1
---

# async user address composition

## Context

Backend service compositionda bir async call natijasi keyingi call inputi bo'lishi mumkin. Bunday flow parallel emas, dependency-driven sequential async flow hisoblanadi.

## Code

```rust
struct User {
    user_id: u64,
    name: String,
    addr_id: u64,
}

struct Address {
    addr_id: u64,
    location: String,
}

struct UserInfo {
    user: User,
    addr: Address,
}

async fn get_user_by_id(user_id: u64) -> User {
    User {
        user_id,
        name: String::from("Ali"),
        addr_id: 10,
    }
}

async fn get_address_by_id(addr_id: u64) -> Address {
    Address {
        addr_id,
        location: String::from("Tashkent"),
    }
}

async fn get_user_with_address(user_id: u64) -> UserInfo {
    let user = get_user_by_id(user_id).await;
    let addr = get_address_by_id(user.addr_id).await;
    UserInfo { user, addr }
}
```

## Explanation

- `get_user_by_id(user_id).await` tugamaguncha `addr_id` ma'lum emas.
- Shu sabab `get_address_by_id` oldingi natijaga bog'liq.
- `.await` code'ni o'qilishi oson sequential flowga aylantiradi, lekin thread'ni blocking kutishga majbur qilmaydi.

## Related Concepts

- [[async-await|async/await]]
- [[future|Future]]
- [[async-block|async block]]
- [[io-bound|I/O-bound]]

## Sources

- [[wiki/sources/rust-for-backend-developers-async-in-rust]]

