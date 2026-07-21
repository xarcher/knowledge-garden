---
title: "Load Balancer & Proxy: L4 vs L7, Reverse Proxy"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 9
tags:
  - networking
  - load-balancer
  - proxy
  - backend
---

> Phần 9 của series [[index|Computer Networking: Zero to Hero]]

## Tại sao cần Load Balancer?

<!-- Single server: SPOF, giới hạn capacity -->
<!-- Horizontal scaling: nhiều server, cần phân phối traffic -->
<!-- Health check: tự động loại server dead -->

---

## L4 vs L7 Load Balancer

### L4 (Transport Layer)
<!-- Dựa trên IP, port, TCP/UDP -->
<!-- Không nhìn vào HTTP content -->
<!-- Nhanh hơn, ít overhead -->
<!-- Use case: raw TCP, UDP, không cần routing theo content -->
<!-- AWS NLB, HAProxy TCP mode -->

### L7 (Application Layer)
<!-- Nhìn vào HTTP header, URL, cookie -->
<!-- Route theo path, host header -->
<!-- SSL termination, header manipulation -->
<!-- Chậm hơn L4 nhưng flexible hơn -->
<!-- AWS ALB, nginx, Envoy -->

---

## Algorithms phân phối traffic

<!-- Round-robin: đơn giản, không tính load thực tế -->
<!-- Weighted round-robin: server mạnh hơn nhận nhiều hơn -->
<!-- Least connections: gửi đến server ít connection nhất -->
<!-- IP hash: cùng client → cùng server (sticky) -->
<!-- Random with 2 choices (power of two): balance tốt, đơn giản -->

---

## Reverse Proxy vs Forward Proxy

<!-- Forward proxy: client → proxy → internet (VPN, corporate proxy) -->
<!-- Reverse proxy: client → proxy → server (nginx trước app) -->
<!-- Reverse proxy hide server topology -->

---

## Vấn đề thực tế với Load Balancer và WebSocket

<!-- WebSocket: stateful, long-lived connection -->
<!-- Round-robin sẽ route different WS connection đến different server -->
<!-- Sticky session: dùng cookie hoặc IP hash -->
<!-- AWS ALB: support WebSocket natively -->

---

## Health Check

<!-- Active: LB gửi probe đến backend (HTTP GET /health) -->
<!-- Passive: monitor real traffic, detect error -->
<!-- Graceful shutdown: drain connection trước khi remove server -->

---

## Keywords để tìm hiểu

- `L4 vs L7 load balancer`
- `nginx reverse proxy configuration`
- `load balancing algorithms`
- `AWS ALB vs NLB`
- `sticky session load balancer`
- `health check graceful shutdown`
- `Envoy proxy service mesh`
- `load balancer websocket support`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
