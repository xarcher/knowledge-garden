---
title: "IP & Routing: Địa chỉ, Subnet, NAT"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 2
tags:
  - networking
  - ip
  - routing
  - backend
---

> Phần 2 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[01-packet-journey|Packet Journey]]

## IP Address: Nhiều hơn một dãy số

<!-- IPv4: 32-bit, ~4.3 tỷ địa chỉ — tại sao không đủ -->
<!-- IPv6: 128-bit, giải quyết exhaustion nhưng transition chậm -->
<!-- Public vs Private IP: 10.x, 172.16-31.x, 192.168.x.x -->
<!-- Loopback: 127.0.0.1, ::1 -->

---

## Subnet và CIDR

<!-- /24 = 256 địa chỉ, /16 = 65536, /32 = 1 host -->
<!-- Network address và broadcast address -->
<!-- Tính subnet: 192.168.1.0/24 → host range 192.168.1.1 - 192.168.1.254 -->
<!-- Tại sao backend engineer cần biết: VPC, Kubernetes network, security group -->

---

## Routing: Packet đi đường nào?

<!-- Routing table: destination → next hop, interface -->
<!-- Longest prefix match: /28 ưu tiên hơn /24 -->
<!-- Default route: 0.0.0.0/0 — gửi đi đâu khi không match gì -->
<!-- Static vs dynamic routing (BGP, OSPF) -->

---

## NAT: Giải pháp tạm thời cho IPv4 exhaustion

<!-- Nhiều máy dùng chung 1 public IP -->
<!-- NAT table: (private IP, port) ↔ (public IP, port) -->
<!-- Tại sao NAT phức tạp: không thể initiate từ outside vào -->
<!-- NAT traversal: STUN, TURN, hole punching (WebRTC, game) -->
<!-- Tác động với backend: X-Forwarded-For, Real-IP header -->

---

## IP trong context backend

<!-- Docker network: bridge, host, overlay -->
<!-- Kubernetes: pod IP, service IP (ClusterIP), node IP -->
<!-- Cloud VPC: subnet, route table, internet gateway -->
<!-- Security group / firewall: rule dựa trên IP và port -->

---

## Keywords để tìm hiểu

- `CIDR notation explained`
- `IPv4 vs IPv6`
- `NAT how it works`
- `BGP routing protocol basics`
- `Docker networking modes`
- `Kubernetes networking CNI`
- `VPC subnets AWS`
- `NAT traversal STUN TURN`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
