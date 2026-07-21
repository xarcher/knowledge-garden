---
title: "DNS: Hệ thống tra cứu của Internet"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 8
tags:
  - networking
  - dns
  - backend
---

> Phần 8 của series [[index|Computer Networking: Zero to Hero]]

## DNS là gì?

<!-- Distributed database: hostname → IP address -->
<!-- Tại sao distributed? Không thể 1 server handle toàn bộ internet -->
<!-- UDP port 53 (query nhỏ), TCP port 53 (response lớn, zone transfer) -->

---

## DNS Resolution: Từng bước

<!-- Browser cache → OS cache → /etc/hosts → Recursive resolver (ISP) -->
<!-- Recursive resolver → Root nameserver → TLD nameserver → Authoritative nameserver -->

```
Browser → Recursive Resolver → Root NS (.) → TLD NS (.com) → Auth NS (example.com) → IP
```

<!-- Iterative vs recursive query -->
<!-- Caching tại mỗi tầng: TTL quyết định cache duration -->

---

## DNS Record Types

<!-- A: hostname → IPv4 -->
<!-- AAAA: hostname → IPv6 -->
<!-- CNAME: alias → canonical name -->
<!-- MX: mail server -->
<!-- TXT: arbitrary text (SPF, DKIM, domain verification) -->
<!-- NS: nameserver của domain -->
<!-- PTR: reverse DNS (IP → hostname) -->
<!-- SRV: service discovery (host, port, priority) -->

---

## TTL và Caching

<!-- TTL thấp: thay đổi propagate nhanh, nhưng nhiều query hơn -->
<!-- TTL cao: ít query, nhưng thay đổi chậm propagate -->
<!-- Khi migrate server: giảm TTL trước, migrate, rồi tăng lại -->
<!-- Negative caching: NXDOMAIN cũng được cache -->

---

## DNS và Backend

<!-- Service discovery: Kubernetes dùng DNS (CoreDNS) -->
<!-- Load balancing qua DNS: round-robin, weighted -->
<!-- Health check: DNS failover -->
<!-- TTL ngắn cho canary/blue-green deployment -->

---

## DNS Security

<!-- DNS poisoning: inject fake records vào cache -->
<!-- DNSSEC: sign record với crypto, verify chain of trust -->
<!-- DoH (DNS over HTTPS): encrypt DNS query, bypass ISP snooping -->
<!-- DoT (DNS over TLS): tương tự DoH -->

---

## Keywords để tìm hiểu

- `DNS resolution explained step by step`
- `DNS record types A AAAA CNAME MX`
- `DNS TTL caching`
- `DNSSEC how it works`
- `DNS over HTTPS DoH`
- `Kubernetes CoreDNS service discovery`
- `DNS load balancing`
- `DNS cache poisoning`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
- RFC 1034/1035 (DNS): https://datatracker.ietf.org/doc/html/rfc1035
