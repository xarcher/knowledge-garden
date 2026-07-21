---
title: "Dữ liệu đi từ A đến B như thế nào: Packet Journey"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 1
tags:
  - networking
  - osi
  - backend
---

> Phần 1 của series [[index|Computer Networking: Zero to Hero]]
> Bài mở đầu — không cần prerequisite

## Câu hỏi đơn giản, câu trả lời không đơn giản

<!-- Khi bạn gọi HTTP request, điều gì thực sự xảy ra? -->
<!-- Từ dòng code đến bit trên đường truyền và ngược lại -->
<!-- Bài này là big picture — các bài sau sẽ deep dive từng tầng -->

---

## OSI Model: Bản đồ để điều hướng

<!-- 7 tầng, nhưng không cần nhớ hết -->
<!-- Cách nhớ thực tế: L3 (IP), L4 (TCP/UDP), L7 (HTTP/WS/gRPC) -->
<!-- Encapsulation: mỗi tầng thêm header vào data của tầng trên -->

```
Application (L7): HTTP, WebSocket, gRPC, DNS
Transport   (L4): TCP, UDP — port numbers
Network     (L3): IP — routing, addressing
Data Link   (L2): Ethernet, WiFi — MAC address
Physical    (L1): bits trên dây, sóng radio
```

---

## Packet Journey: Từ curl đến server và về

### Client side
<!-- 1. App gọi write() vào socket -->
<!-- 2. Kernel: TCP segment, IP packet, Ethernet frame -->
<!-- 3. NIC: đưa lên đường truyền vật lý -->

### Network
<!-- Router nhận frame, strip L2 header, lookup routing table, forward -->
<!-- TTL giảm mỗi hop, đến 0 thì drop (traceroute dùng cái này) -->
<!-- Nhiều router trung gian: ISP, backbone, CDN edge -->

### Server side
<!-- NIC nhận bits, tái tạo frame -->
<!-- Kernel: strip headers từng tầng, deliver payload đến app qua socket -->
<!-- App gọi read() lấy data -->

---

## Encapsulation và De-encapsulation

```
[HTTP data]
[TCP header][HTTP data]
[IP header][TCP header][HTTP data]
[Ethernet header][IP header][TCP header][HTTP data][Ethernet trailer]
```

<!-- Mỗi tầng "bọc" data của tầng trên -->
<!-- Receiver "bóc" từng lớp từ dưới lên -->

---

## Latency ở đâu ra?

<!-- Propagation delay: tốc độ ánh sáng trong cáp quang ~200,000 km/s -->
<!-- Transmission delay: bandwidth, kích thước packet -->
<!-- Processing delay: router, switch lookup -->
<!-- Queuing delay: buffer đầy, phải chờ -->
<!-- Round-trip time (RTT): Hà Nội ↔ Singapore ~30ms, ↔ US ~200ms -->

---

## Tools để "nhìn" packet journey

```bash
# Xem các hop từ máy bạn đến destination
traceroute google.com

# Đo RTT
ping google.com

# Xem routing table
ip route

# Capture packet thực sự
tcpdump -i eth0 -n
```

---

## Keywords để tìm hiểu

- `OSI model explained`
- `TCP/IP model vs OSI model`
- `network packet encapsulation`
- `traceroute how it works TTL`
- `network latency components`
- `how does the internet work`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
