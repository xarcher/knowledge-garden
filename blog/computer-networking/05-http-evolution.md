---
title: "HTTP/1.1 → HTTP/2 → HTTP/3: Tại sao mỗi version ra đời"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 5
tags:
  - networking
  - http
  - protocol
  - backend
---

> Phần 5 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[03-tcp-deep-dive|TCP Deep Dive]], [[04-udp-and-quic|UDP & QUIC]]

## HTTP/0.9 và HTTP/1.0: Khởi đầu đơn giản

<!-- 1 request → 1 TCP connection → đóng ngay -->
<!-- Overhead: 3-way handshake cho mỗi request -->

---

## HTTP/1.1: Keep-Alive và những vấn đề mới

<!-- Connection: keep-alive — tái sử dụng TCP connection -->
<!-- Pipelining: gửi nhiều request không chờ response — nhưng broken trên thực tế -->
<!-- HOL Blocking: response phải theo thứ tự request -->
<!-- Workaround của browser: mở 6-8 connection song song đến cùng domain -->
<!-- Header không nén, verbose, lặp lại mỗi request -->

---

## HTTP/2: Multiplexing giải quyết HOL (một phần)

<!-- Binary protocol thay vì text -->
<!-- Multiplexing: nhiều stream trên 1 TCP connection -->
<!-- Header compression: HPACK -->
<!-- Server Push: server gửi resource trước khi client hỏi -->
<!-- Vẫn còn HOL blocking ở TCP layer -->

---

## HTTP/3: Bỏ TCP, dùng QUIC

<!-- QUIC giải quyết TCP HOL blocking -->
<!-- 0-RTT/1-RTT connection -->
<!-- Connection migration -->
<!-- Tại sao adoption chậm: UDP bị block, infrastructure cần update -->

---

## So sánh thực tế

<!-- Khi nào HTTP/2 thực sự tốt hơn HTTP/1.1? -->
<!-- HTTP/3 ở đâu rồi? (~30% web traffic 2024) -->
<!-- Backend internal service: vẫn HTTP/1.1 hoặc gRPC (HTTP/2) -->

---

## Keywords để tìm hiểu

- `HTTP/1.1 keep-alive pipelining`
- `HTTP/2 multiplexing streams`
- `HTTP/2 HPACK header compression`
- `HTTP/2 server push`
- `HTTP/3 QUIC explained`
- `HTTP head of line blocking`
- `gRPC HTTP/2`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
- RFC 9110 (HTTP Semantics): https://datatracker.ietf.org/doc/html/rfc9110
- RFC 9113 (HTTP/2): https://datatracker.ietf.org/doc/html/rfc9113
- RFC 9114 (HTTP/3): https://datatracker.ietf.org/doc/html/rfc9114
