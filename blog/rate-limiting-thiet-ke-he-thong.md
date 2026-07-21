---
title: "Thiết kế Rate Limiting: Token Bucket, Sliding Window và Distributed"
draft: true
created: 2026-07-21
tags:
  - system-design
  - rate-limiting
  - backend
---

## Tại sao cần Rate Limiting?

<!-- Giải thích ngắn: bảo vệ hệ thống khỏi abuse, DDoS, giảm tải, fair usage -->
<!-- Ví dụ thực tế: API public, login endpoint, payment -->

---

## Các thuật toán phổ biến

### 1. Fixed Window Counter
<!-- Đơn giản nhất, nhưng vấn đề gì ở boundary? -->

### 2. Sliding Window Log
<!-- Chính xác hơn, nhưng trade-off về memory? -->

### 3. Sliding Window Counter
<!-- Kết hợp giữa fixed và log, tại sao lại được dùng nhiều? -->

### 4. Token Bucket
<!-- Cơ chế hoạt động: token được thêm theo rate, request tiêu token -->
<!-- Ưu điểm: cho phép burst traffic -->
<!-- Dùng ở đâu: AWS API Gateway, Stripe -->

### 5. Leaky Bucket
<!-- So sánh với Token Bucket: output rate cố định -->
<!-- Khi nào dùng cái này thay vì Token Bucket? -->

---

## Distributed Rate Limiting

<!-- Vấn đề: nhiều instance, mỗi instance có counter riêng → không chính xác -->

### Dùng Redis
<!-- INCR + EXPIRE, Lua script để atomic operation -->
<!-- Redis sliding window với ZSET -->

### Race condition và atomicity
<!-- Tại sao cần atomic? Dùng Lua script hoặc Redis transaction -->

### Sticky session approach
<!-- Đơn giản hơn nhưng trade-off là gì? -->

### Approximate vs Exact
<!-- Đôi khi không cần chính xác 100%, trade-off latency vs accuracy -->

---

## Triển khai thực tế

<!-- Đặt rate limiter ở đâu: API Gateway, middleware, application layer? -->
<!-- Header chuẩn trả về: X-RateLimit-Limit, X-RateLimit-Remaining, Retry-After -->
<!-- Xử lý khi bị limit: 429 Too Many Requests -->

---

## Kinh nghiệm thực tế / Bài học

<!-- Điền vào đây sau khi bạn đã có trải nghiệm hoặc đọc xong -->

---

## Tham khảo

<!-- Thêm link sau khi đọc -->

---

## Keywords để tìm hiểu

- `token bucket algorithm`
- `sliding window rate limiting redis`
- `rate limiting distributed system`
- `redis lua script atomic rate limit`
- `leaky bucket vs token bucket`
- `API rate limiting best practices`
- `X-RateLimit headers RFC`
- `Guava RateLimiter java`
- `golang.org/x/time/rate`
- `rate limiting at scale stripe engineering blog`
- `cloudflare rate limiting architecture`
