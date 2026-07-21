---
title: "TCP Deep Dive: Reliability trên nền Unreliable IP"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 3
tags:
  - networking
  - tcp
  - protocol
  - backend
---

> Phần 3 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[02-ip-routing|IP & Routing]]

## Tại sao backend engineer cần hiểu TCP sâu?

<!-- Không phải để làm network engineer -->
<!-- Mà để hiểu: tại sao connection timeout, tại sao latency tăng đột biến -->
<!-- Tại sao HTTP/2, QUIC, WebSocket thiết kế như vậy -->
<!-- Debug production: tcpdump, Wireshark, ss, netstat -->

---

## IP là unreliable — TCP giải quyết điều đó như thế nào?

<!-- Connectionless, best-effort: không đảm bảo deliver, order, dedup -->
<!-- Packet có thể: mất, duplicate, reorder, delay -->
<!-- TCP ngồi trên IP và thêm: reliability, ordering, flow control, congestion control -->

---

## 3-Way Handshake

```
Client          Server
  |--- SYN ------->|   seq=x
  |<-- SYN-ACK ----|   seq=y, ack=x+1
  |--- ACK ------->|   ack=y+1
  |   ESTABLISHED  |
```

<!-- Tại sao cần 3 bước, không phải 2? -->
<!-- SYN flood attack: half-open connection, SYN cookie -->
<!-- TCP Fast Open: skip handshake cho connection lặp lại -->
<!-- Initial Sequence Number (ISN): random, tại sao không bắt đầu từ 0? -->

---

## Sequence Number và Reliability

<!-- Mỗi byte có sequence number -->
<!-- Receiver gửi ACK xác nhận đã nhận -->
<!-- Retransmission timeout (RTO): gửi lại nếu không nhận ACK -->
<!-- Duplicate ACK → fast retransmit, không cần chờ timeout -->
<!-- Selective ACK (SACK): chỉ retransmit những gì bị mất, không retransmit cả window -->

---

## Flow Control: Receiver Window

<!-- Receiver quảng bá window size: "tôi còn nhận được X bytes" -->
<!-- Sender không được gửi quá window size -->
<!-- Window scale option: TCP header chỉ có 16-bit window, cần scale cho high-bandwidth link -->
<!-- Zero window: receiver buffer đầy, sender phải dừng -->
<!-- Zero window probe: sender gửi 1 byte để check receiver còn sống không -->

---

## Congestion Control

<!-- Khác flow control: không phải receiver mà là NETWORK bị quá tải -->
<!-- Làm sao biết network congested? Packet loss, RTT tăng -->

### Slow Start
<!-- Bắt đầu cwnd = 1 MSS, tăng gấp đôi mỗi RTT -->
<!-- Tại sao "slow"? So với tối đa thì slow, nhưng tăng exponential -->

### Congestion Avoidance
<!-- Sau khi cwnd vượt ssthresh: tăng linear (1 MSS mỗi RTT) -->

### Fast Retransmit & Fast Recovery
<!-- 3 duplicate ACK → retransmit ngay, không chờ timeout -->
<!-- Giảm ssthresh và cwnd, tiếp tục từ đó -->

### CUBIC và BBR
<!-- CUBIC: thuật toán mặc định Linux, dựa trên time since last loss -->
<!-- BBR (Google): dựa trên bandwidth và RTT, không chờ packet loss -->
<!-- BBR tốt hơn trong high-latency, lossy network -->

---

## 4-Way Termination

```
Client          Server
  |--- FIN ------->|
  |<-- ACK --------|
  |<-- FIN --------|
  |--- ACK ------->|
  | TIME_WAIT 2MSL |
```

<!-- TIME_WAIT: tại sao cần chờ 2*MSL (thường 60-120s)? -->
<!-- Đảm bảo ACK cuối đến nơi, tránh old packet từ connection cũ nhầm vào connection mới -->
<!-- TIME_WAIT accumulation: server nhiều connection ngắn → port exhaustion -->
<!-- SO_REUSEADDR, SO_REUSEPORT để giảm thiểu vấn đề này -->

---

## TCP State Machine

<!-- CLOSED → LISTEN → SYN_RCVD → ESTABLISHED → FIN_WAIT_1 → FIN_WAIT_2 → TIME_WAIT → CLOSED -->

```bash
# Xem state thực tế trên Linux
ss -tan

# CLOSE_WAIT nhiều: application không close socket đúng cách (bug!)
# TIME_WAIT nhiều: normal, nhưng watch out port exhaustion
# SYN_RECV nhiều: có thể đang bị SYN flood
```

---

## Nagle's Algorithm và TCP_NODELAY

<!-- Gom nhiều write() nhỏ thành 1 packet để giảm overhead -->
<!-- Vấn đề với real-time app: thêm latency không mong muốn -->
<!-- TCP_NODELAY: disable Nagle -->
<!-- Delayed ACK tương tác với Nagle → 40ms delay kinh điển -->
<!-- Trading bot, game server, interactive terminal: luôn set TCP_NODELAY -->

---

## Kernel Socket Buffer

<!-- Send buffer và receive buffer: mỗi socket có 2 buffer trong kernel -->
<!-- Khi send buffer đầy: write() block hoặc EAGAIN (non-blocking) -->
<!-- Khi receive buffer đầy: TCP advertise zero window, sender dừng -->

```bash
# Xem và tune buffer size
cat /proc/sys/net/core/rmem_max
cat /proc/sys/net/core/wmem_max

# Per socket (Java/Go: thường set qua socket options)
# SO_SNDBUF, SO_RCVBUF
```

---

## Debug TCP trên Linux

```bash
# Tổng quan connection và state
ss -tan

# Stats: retransmit, errors
ss -s
netstat -s | grep -i retrans

# Capture packet
tcpdump -i eth0 port 8080 -w capture.pcap

# Analyze với Wireshark: xem RTT, window size, retransmission
```

---

## Keywords để tìm hiểu

- `TCP 3-way handshake RFC 793`
- `TCP congestion control CUBIC BBR`
- `TCP flow control sliding window`
- `TCP TIME_WAIT explanation`
- `Nagle algorithm TCP_NODELAY delayed ACK`
- `TCP SACK selective acknowledgment`
- `TCP Fast Open TFO`
- `SYN flood SYN cookie protection`
- `Linux socket buffer tuning`
- `tcpdump wireshark tutorial`
- `TCP state machine diagram`
- `BBR congestion control Google`

---

## Tham khảo

- RFC 793 (TCP): https://datatracker.ietf.org/doc/html/rfc793
- RFC 2018 (SACK): https://datatracker.ietf.org/doc/html/rfc2018
