---
title: "CDN & Caching at Network Level"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 10
tags:
  - networking
  - cdn
  - caching
  - backend
---

> Phần 10 của series [[index|Computer Networking: Zero to Hero]]

## CDN là gì?

<!-- Network of servers đặt gần user (edge nodes) -->
<!-- Mục tiêu: giảm latency bằng cách serve từ gần user -->
<!-- Anycast: cùng IP, route đến server gần nhất -->
<!-- Cloudflare, Fastly, AWS CloudFront, Akamai -->

---

## CDN hoạt động như thế nào?

<!-- User request → DNS resolve → edge node gần nhất (anycast) -->
<!-- Edge cache hit: serve ngay, không cần về origin -->
<!-- Cache miss: edge fetch từ origin, cache lại, serve user -->
<!-- TTL và cache invalidation: khi nào edge làm mới cache? -->

---

## HTTP Caching Headers

<!-- Cache-Control: max-age, no-cache, no-store, public, private -->
<!-- ETag: fingerprint của content, conditional request -->
<!-- Last-Modified: timestamp, If-Modified-Since -->
<!-- Vary: cache theo header khác nhau (Accept-Encoding, Accept-Language) -->
<!-- Stale-while-revalidate: serve stale trong khi fetch mới ở background -->

---

## CDN và Dynamic Content

<!-- Static: image, CSS, JS — cache tốt -->
<!-- Dynamic: API response — cần careful -->
<!-- Edge computing: Cloudflare Workers, Lambda@Edge — chạy logic tại edge -->
<!-- Cache key: URL + header nào? Quan trọng để tránh cache wrong response -->

---

## Cache Invalidation: Bài toán khó

<!-- Purge by URL, tag, prefix -->
<!-- Surrogate-Key / Cache-Tag: group related content -->
<!-- Immutable asset: content-hash trong filename → cache forever -->
<!-- Blue-green deployment với CDN: flush cache khi deploy -->

---

## CDN và Security

<!-- DDoS mitigation: absorb tại edge, không về origin -->
<!-- WAF tại edge: block attack trước khi vào system -->
<!-- Bot detection -->
<!-- TLS termination tại edge -->

---

## Keywords để tìm hiểu

- `CDN how it works anycast`
- `HTTP Cache-Control headers`
- `ETag conditional requests`
- `CDN cache invalidation strategies`
- `Cloudflare Workers edge computing`
- `CDN DDoS protection`
- `stale-while-revalidate`
- `immutable cache assets`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
