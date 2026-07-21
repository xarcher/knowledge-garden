---
title: "TLS/HTTPS: Handshake, Certificate, mTLS"
draft: true
created: 2026-07-21
series: "Computer Networking: Zero to Hero"
part: 6
tags:
  - networking
  - tls
  - https
  - security
  - backend
---

> Phần 6 của series [[index|Computer Networking: Zero to Hero]]
> Prerequisite: [[03-tcp-deep-dive|TCP Deep Dive]]

## TLS là gì và tại sao không phải SSL?

<!-- SSL deprecated, TLS là successor -->
<!-- TLS 1.2 vs TLS 1.3: khác nhau gì, tại sao 1.3 tốt hơn -->

---

## TLS Handshake: Từng bước

### TLS 1.2
<!-- 2-RTT: ClientHello → ServerHello + Certificate → ClientKeyExchange → Finished -->
<!-- Asymmetric crypto để trao đổi key, symmetric sau đó -->

### TLS 1.3
<!-- 1-RTT: gộp nhiều bước, loại bỏ cipher suite yếu -->
<!-- 0-RTT (early data): trade-off với replay attack -->

---

## Certificate và PKI

<!-- X.509 certificate: public key + identity + signature của CA -->
<!-- Certificate chain: leaf → intermediate → root CA -->
<!-- Tại sao browser tin? Root CA built-in vào OS/browser -->
<!-- Certificate validation: expiry, revocation (OCSP, CRL), chain -->
<!-- SNI: nhiều domain trên 1 IP, server biết certificate nào dùng -->

---

## Symmetric vs Asymmetric Crypto trong TLS

<!-- Asymmetric (RSA, ECDSA): chậm, dùng để trao đổi session key -->
<!-- Symmetric (AES-GCM, ChaCha20): nhanh, dùng để encrypt actual data -->
<!-- Perfect Forward Secrecy: mỗi session key độc lập, leak key cũ không decrypt được -->
<!-- ECDHE: key exchange hiệu quả, PFS built-in -->

---

## mTLS: Mutual TLS

<!-- TLS thường: chỉ client verify server -->
<!-- mTLS: cả hai phía verify nhau -->
<!-- Use case: microservice internal communication, zero-trust network -->
<!-- Kubernetes service mesh: Istio, Linkerd dùng mTLS -->
<!-- Overhead: certificate management cho từng service -->

---

## TLS trong thực tế backend

<!-- Termination: TLS kết thúc ở đâu? Load balancer, API gateway, hay từng service? -->
<!-- Certificate rotation: tự động với Let's Encrypt, cert-manager -->
<!-- Debugging: openssl s_client, curl -v -->

```bash
# Check certificate
openssl s_client -connect google.com:443 -showcerts

# Check expiry
echo | openssl s_client -connect google.com:443 2>/dev/null | openssl x509 -noout -dates
```

---

## Keywords để tìm hiểu

- `TLS 1.3 handshake explained`
- `TLS certificate chain PKI`
- `perfect forward secrecy ECDHE`
- `mTLS mutual TLS microservices`
- `TLS termination load balancer`
- `Let's Encrypt cert-manager kubernetes`
- `TLS 0-RTT replay attack`
- `OCSP stapling certificate revocation`

---

## Tham khảo

<!-- Thêm link sau khi đọc -->
- RFC 8446 (TLS 1.3): https://datatracker.ietf.org/doc/html/rfc8446
