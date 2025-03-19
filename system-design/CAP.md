---
title: "CAP Theorem"
draft: false
---

# CAP Theorem

The **CAP theorem** (Consistency, Availability, Partition Tolerance) nói rằng trong hệ thống phân tán, bạn chỉ có thể đảm bảo tối đa **2 trong 3 thuộc tính**.

- **Consistency**: mọi node thấy cùng 1 dữ liệu tại cùng 1 thời điểm.
- **Availability**: hệ thống luôn phản hồi dù có lỗi một phần.
- **Partition Tolerance**: hệ thống vẫn hoạt động khi có mất kết nối mạng giữa các node.

➡ Thực tế, các hệ thống hiện đại thường chọn **AP** (như Cassandra) hoặc **CP** (như MongoDB, HBase).

