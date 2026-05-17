---
title: "Request-Response"
type: concept
status: active
created: 2026-05-13
updated: 2026-05-13
tags: [rust, networking, protocol]
source_count: 2
---

# Request-Response

## Short Definition

Client avval so'rov yuboradi, server esa unga javob qaytaradi degan interaction modeli. Chapter 21 kontekstida browser `GET / HTTP/1.1` yuboradi va server `HTTP/1.1 200 OK` yoki `404 NOT FOUND` qaytaradi.

## Why It Matters

Web server behavior'ni tushunishda "kim birinchi gapiradi?" savoli muhim. Server o'zi hech narsa boshlamaydi; u tinglaydi va incoming request'ga javob beradi. Routing, error handling, va throughput ham shu modelga quriladi.

## Mental Model

Bu patternni pochta almashinuvi kabi ko'rish mumkin: client xat yuboradi, server esa xatga javob qaytaradi. TCP connection texnik transport bo'lsa, request-response uning ichidagi application oqimidir.

## Syntax and Examples

### Browser request

```text
GET / HTTP/1.1
Host: 127.0.0.1:7878
```

### Server response

```text
HTTP/1.1 404 NOT FOUND\r\nContent-Length: 54\r\n\r\n...
```

## Common Mistakes

- **Connection'ni response bilan tenglashtirish**: connection transport, request-response esa undan foydalanadigan mantiqiy almashinuv.
- **Bir page load = bir request deb o'ylash**: browser ko'pincha bir necha request yoki ulanish yaratadi.
- **Server first-mover deb o'ylash**: oddiy HTTP server client so'ramaguncha hech narsa yubormaydi.

## Related Concepts

- [[http]]
- [[tcp]]
- [[web-server]]
- [[tcp-stream]]

## Sources

- [[wiki/sources/21-final-project-building-a-multithreaded-web-server|21]]
- [[wiki/sources/21-1-building-a-single-threaded-web-server|21.1]]
