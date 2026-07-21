---
title: "UDP & QUIC: Khi nào không cần Reliability"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 4
tags:
  - networking
  - udp
  - quic
  - protocol
  - backend
---

> Phần 4 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[03-tcp-deep-dive|TCP Deep Dive]]

## UDP: Đơn giản đến mức tối thiểu

### Cấu trúc UDP datagram
```
 0      7 8     15 16    23 24    31
+--------+--------+--------+--------+
|   Source Port   | Destination Port|
+--------+--------+--------+--------+
|     Length      |    Checksum     |
+--------+--------+--------+--------+
|          data octets ...
+---------------- ...
```

<!-- Header chỉ 8 bytes, so với TCP 20-60 bytes -->
<!-- Checksum optional (IPv4), mandatory (IPv6) -->
<!-- Không có: sequence number, ACK, window, connection state -->

### UDP làm gì và không làm gì

<!-- Làm: multiplexing qua port, optional error detection (checksum) -->
<!-- Không làm: connection setup, reliability, ordering, flow control, congestion control -->
<!-- Fire and forget: gửi xong không biết có đến không, không quan tâm -->

---

## Khi nào UDP tốt hơn TCP?

### Latency quan trọng hơn reliability
<!-- DNS: query/response đơn giản, retry logic tự làm ở app layer nếu cần -->
<!-- NTP: time sync, packet cũ không có giá trị -->
<!-- DHCP: broadcast, không có connection để setup -->

### Data cũ thì vô nghĩa — bỏ còn hơn delay
<!-- Video/Audio call: mất 1 frame → glitch nhỏ, retransmit → delay nghe thấy -->
<!-- Online game: position update 100ms trước đã lỗi thời, không cần retransmit -->
<!-- Live metrics, sensor data: giá trị mới nhất mới quan trọng -->

### Multicast và Broadcast
<!-- TCP là unicast (1-1), UDP hỗ trợ multicast (1-nhiều) và broadcast -->
<!-- Discovery protocol, live streaming đến nhiều receiver -->

---

## TCP vs UDP: So sánh thực tế

| | TCP | UDP |
|---|---|---|
| Connection | Yes (3-way handshake) | No |
| Reliability | Guaranteed | Best-effort |
| Ordering | Yes | No |
| Flow control | Yes | No |
| Congestion control | Yes | No |
| Header size | 20-60 bytes | 8 bytes |
| Latency overhead | Higher | Lower |
| Use case | HTTP, SSH, DB, file transfer | DNS, video call, game, QUIC |

---

## Head-of-Line Blocking: Vấn đề của TCP

<!-- TCP đảm bảo ordered delivery → nếu packet 5 bị mất, packet 6-10 phải chờ -->
<!-- HTTP/1.1: 1 request/connection → HOL blocking rõ ràng -->
<!-- HTTP/2: multiplexing trên 1 TCP connection → HOL blocking ở TCP layer -->
<!-- Giải pháp triệt để: không dùng TCP → QUIC -->

---

## QUIC: Reliability trên UDP, làm đúng hơn TCP

<!-- Google phát triển ~2012, chuẩn hóa RFC 9000 (2021) -->
<!-- HTTP/3 = HTTP/2 semantics + QUIC transport -->

### Tại sao không fix TCP mà làm lại từ đầu?
<!-- TCP Ossification: middleboxes (firewall, NAT, proxy) assume TCP behavior -->
<!-- Không thể thay đổi TCP header mà không break network equipment -->
<!-- UDP "transparent" hơn với middleboxes, QUIC encrypt header để tránh bị tamper -->

### QUIC giải quyết được gì?

#### 0-RTT / 1-RTT Connection
<!-- TCP+TLS: 3-way handshake (1 RTT) + TLS handshake (1-2 RTT) = 2-3 RTT -->
<!-- QUIC: kết hợp transport + crypto handshake = 1 RTT lần đầu, 0-RTT cho connection sau -->

#### Independent Streams, no HOL Blocking
<!-- Mỗi stream độc lập: stream A mất packet không block stream B -->
<!-- So sánh HTTP/2 trên TCP vs HTTP/3 trên QUIC -->

#### Connection Migration
<!-- TCP connection = 4-tuple (src IP, src port, dst IP, dst port) -->
<!-- Đổi IP (WiFi → 4G) = connection chết, phải reconnect -->
<!-- QUIC dùng Connection ID → đổi IP, connection vẫn sống -->
<!-- Quan trọng với mobile app -->

### QUIC có thay thế TCP hoàn toàn không?
<!-- Cho HTTP/web: đang dần replace (HTTP/3 ~30% traffic) -->
<!-- Cho database, microservice internal: vẫn TCP -->
<!-- UDP/QUIC bị block ở một số corporate network -->

---

## Implement reliability trên UDP: Những thứ cần làm

<!-- Khi bạn muốn UDP nhưng cần một số reliability feature -->
<!-- ACK + retransmit cho critical message -->
<!-- Sequence number để detect loss và reorder -->
<!-- Congestion control để không flood network -->
<!-- Đây chính xác là những gì QUIC làm -->

---

## Thực tế: Debug UDP

```bash
# Xem UDP socket
ss -uan

# UDP stats: errors, buffer overflow
netstat -su

# Capture UDP
tcpdump -i eth0 udp port 53

# UDP buffer overflow: application đọc không kịp → packet bị drop silently
cat /proc/sys/net/core/rmem_max
```

<!-- UDP drop silently: không có error, không có notification -->
<!-- Cần metrics ở application layer để detect -->

---

## Keywords để tìm hiểu

- `UDP RFC 768`
- `UDP vs TCP when to use`
- `TCP head of line blocking HTTP/2`
- `QUIC RFC 9000`
- `HTTP/3 QUIC explained`
- `QUIC 0-RTT connection`
- `QUIC connection migration`
- `TCP ossification middlebox`
- `UDP multicast`
- `reliable UDP implementation`
- `QUIC vs TCP performance comparison`

---

## Tham khảo

- RFC 768 (UDP): https://datatracker.ietf.org/doc/html/rfc768
- RFC 9000 (QUIC): https://datatracker.ietf.org/doc/html/rfc9000
