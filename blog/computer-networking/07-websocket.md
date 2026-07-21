---
title: "WebSocket Deep Dive: Từ TCP đến Full-Duplex Connection"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 7
tags:
  - websocket
  - networking
  - protocol
  - backend
---

> Phần 7 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[03-tcp-deep-dive|TCP Deep Dive]], [[05-http-evolution|HTTP Evolution]], [[06-tls-https|TLS/HTTPS]]

## WebSocket là gì, và tại sao nó tồn tại?

<!-- HTTP request-response không đủ cho real-time: polling, long-polling, SSE — vấn đề của từng cái -->
<!-- WebSocket ra đời để giải quyết gì? RFC 6455 (2011) -->
<!-- Use case: trading, chat, live dashboard, multiplayer game -->

---

## Nền tảng: TCP và tại sao WebSocket chạy trên nó

<!-- TCP: reliable, ordered, connection-oriented -->
<!-- 3-way handshake: SYN → SYN-ACK → ACK -->
<!-- TCP stream vs UDP datagram — tại sao WS chọn TCP -->
<!-- Port 80 (ws://) và 443 (wss://) — piggyback trên HTTP infrastructure -->

---

## HTTP Upgrade: Cơ chế chuyển protocol

<!-- WebSocket bắt đầu bằng HTTP request — tại sao thiết kế như vậy? -->
<!-- Firewall, proxy thân thiện với HTTP -->

### Request Upgrade
```
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

### Response 101 Switching Protocols
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

### Sec-WebSocket-Key / Accept hoạt động như thế nào?
<!-- Key là random base64, server concat với GUID cố định rồi SHA-1 + base64 -->
<!-- Mục đích: prevent cache poisoning, không phải security -->

---

## WebSocket Frame: Đơn vị truyền dữ liệu

<!-- Sau khi upgrade, TCP stream được chia thành frames -->

### Cấu trúc frame
```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-------+-+-------------+-------------------------------+
|F|R|R|R| opcode|M| Payload len |    Extended payload length    |
|I|S|S|S|  (4)  |A|     (7)     |             (16/64)           |
|N|V|V|V|       |S|             |   (if payload len==126/127)   |
| |1|2|3|       |K|             |                               |
+-+-+-+-+-------+-+-------------+ - - - - - - - - - - - - - - -+
```

<!-- FIN bit: fragmentation, message có thể chia nhiều frame -->
<!-- Opcode: 0x1 text, 0x2 binary, 0x8 close, 0x9 ping, 0xA pong -->
<!-- Masking: client → server BẮT BUỘC mask, server → client KHÔNG mask. Tại sao? -->

### Masking và lý do bất đối xứng
<!-- Chống cache poisoning từ proxy trung gian -->
<!-- Server không cần mask vì không có proxy vấn đề ở chiều này -->

---

## Full-Duplex: Thực sự hoạt động như thế nào?

<!-- HTTP: half-duplex — request xong mới response được -->
<!-- WebSocket: cả hai chiều độc lập, không cần chờ nhau -->
<!-- Trên nền TCP: một stream, nhưng hai bên đọc/ghi song song -->
<!-- OS level: read thread và write thread trên cùng socket fd -->

<!-- Golang example: goroutine đọc và goroutine ghi chạy đồng thời -->
<!-- Vấn đề concurrent write: cần mutex hay channel? -->

---

## Keep-Alive và Connection Management

### Ping / Pong (RFC 6455 built-in)
<!-- Opcode 0x9 ping, 0xA pong -->
<!-- Server gửi ping → client phải pong ngay lập tức -->
<!-- Detect zombie connection: connected nhưng không phản hồi -->

### TCP Keepalive vs WebSocket Ping/Pong
<!-- Hai cơ chế khác nhau, tầng khác nhau -->
<!-- TCP keepalive: OS level, phát hiện network failure -->
<!-- WS ping/pong: application level, phát hiện peer còn sống -->

### Idle timeout và proxy
<!-- Nginx, AWS ALB có idle timeout mặc định (60s, 60s) -->
<!-- Nếu không ping/pong → proxy ngắt connection âm thầm -->
<!-- Cấu hình proxy_read_timeout, keepalive_timeout -->

---

## Đóng Connection đúng cách

<!-- Close handshake: gửi close frame → nhận close frame → đóng TCP -->
<!-- Close frame có status code: 1000 normal, 1001 going away, 1011 server error -->
<!-- Không close đúng cách → half-open connection, resource leak -->
<!-- So sánh: TCP FIN vs RST trong context WebSocket -->

---

## WebSocket qua TLS (wss://)

<!-- TLS handshake xảy ra trước HTTP upgrade -->
<!-- Thứ tự: TCP connect → TLS handshake → HTTP upgrade → WS frames -->
<!-- Certificate pinning trong trading bot: có cần không? -->

---

## Scalability: Một server, nhiều connection

<!-- Mỗi WS connection = 1 TCP socket fd, giữ mãi -->
<!-- C10K problem — tại sao thread-per-connection không scale -->
<!-- Event loop model: Node.js, Golang goroutine, Java NIO/Netty -->
<!-- Memory per connection: goroutine stack ~2-8KB, so sánh với thread 1MB -->

---

## Thực tế: Những thứ docs không nói

<!-- Điền sau khi làm thực tế -->
<!-- Edge case: server gửi invalid frame, partial frame -->
<!-- Proxy stripping Upgrade header -->
<!-- Load balancer và sticky session với WS -->

---

## Keywords để tìm hiểu

- `RFC 6455 websocket protocol`
- `HTTP upgrade mechanism`
- `Sec-WebSocket-Key accept algorithm`
- `websocket frame structure opcode`
- `websocket masking why client only`
- `full duplex TCP websocket`
- `websocket ping pong heartbeat`
- `TCP keepalive vs application heartbeat`
- `websocket proxy nginx timeout`
- `C10K problem event loop`
- `gorilla websocket golang`
- `nhooyr.io/websocket golang`
- `websocket close handshake`
- `TLS over websocket wss`

---

## Tham khảo

- RFC 6455: https://datatracker.ietf.org/doc/html/rfc6455
