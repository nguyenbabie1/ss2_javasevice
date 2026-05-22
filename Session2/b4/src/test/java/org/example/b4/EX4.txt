# Báo cáo phân tích lựa chọn kiến trúc Web Service

## REST cho Microservices hay SOAP cho Legacy Core Banking?

---

# 1. Tổng quan bài toán

Công ty tài chính đang vận hành song song:

* **Hệ thống Legacy Core Banking**

  * Xử lý giao dịch tài chính cốt lõi.
  * Yêu cầu:

    * độ tin cậy cực cao,
    * tính nhất quán dữ liệu,
    * bảo mật nghiêm ngặt,
    * hỗ trợ distributed transaction,
    * tuân thủ chuẩn tài chính quốc tế.

* **Hệ thống Microservices mới**

  * Phục vụ mobile/web app.
  * Yêu cầu:

    * hiệu suất cao,
    * mở rộng linh hoạt,
    * phát triển nhanh,
    * triển khai liên tục (CI/CD),
    * chịu tải lớn.

Mục tiêu là lựa chọn giữa **SOAP** và **REST** cho từng hệ thống sao cho:

* tối ưu hiệu năng,
* giảm chi phí vận hành,
* tăng tính bảo mật,
* hỗ trợ mở rộng dài hạn.

---

# 2. Tổng quan SOAP và REST

## SOAP (Simple Object Access Protocol)

SOAP là một giao thức Web Service chuẩn hóa, sử dụng XML để trao đổi dữ liệu.

### Đặc điểm chính

* Dựa trên XML.
* Có chuẩn WSDL mô tả service.
* Hỗ trợ:

  * WS-Security,
  * WS-ReliableMessaging,
  * WS-AtomicTransaction.
* Có thể hoạt động trên:

  * HTTP,
  * SMTP,
  * FTP,
  * TCP.

### Mục tiêu

Tập trung vào:

* tính chuẩn hóa,
* bảo mật,
* độ tin cậy,
* transaction enterprise.

---

## REST (Representational State Transfer)

REST là phong cách kiến trúc sử dụng HTTP và tài nguyên (resources).

### Đặc điểm chính

* Stateless.
* Sử dụng:

  * GET,
  * POST,
  * PUT,
  * DELETE.
* Hỗ trợ JSON/XML.
* Tối ưu cho web/mobile.

### Mục tiêu

Tập trung vào:

* hiệu suất,
* scalability,
* tính linh hoạt,
* phát triển nhanh.

---

# 3. Phân tích SOAP cho Legacy Core Banking

## 3.1 Ưu điểm của SOAP với hệ thống tài chính

## a) Bảo mật mạnh mẽ

SOAP hỗ trợ bộ tiêu chuẩn enterprise:

* WS-Security
* XML Encryption
* XML Signature
* SAML

Điều này rất quan trọng với:

* giao dịch ngân hàng,
* xác thực nhiều lớp,
* audit,
* compliance.

### Phù hợp vì:

Hệ thống core banking yêu cầu:

* chống giả mạo giao dịch,
* non-repudiation,
* message integrity.

SOAP xử lý rất tốt các yêu cầu này.

---

## b) Hỗ trợ Distributed Transactions

SOAP hỗ trợ:

* WS-AtomicTransaction
* ACID transaction

Điều này cực kỳ quan trọng trong:

* chuyển khoản,
* thanh toán liên ngân hàng,
* rollback transaction.

### Ví dụ

Nếu:

* trừ tiền tài khoản A thành công,
* nhưng cộng tiền tài khoản B thất bại,

hệ thống cần rollback toàn bộ.

SOAP phù hợp hơn REST cho tình huống này.

---

## c) Reliability cao

SOAP hỗ trợ:

* WS-ReliableMessaging

Đảm bảo:

* message delivery,
* retry,
* ordering.

Trong tài chính:

* mất 1 giao dịch = rủi ro nghiêm trọng.

---

## d) Hợp với kiến trúc enterprise cũ

Legacy systems thường:

* dùng ESB,
* Java EE,
* Oracle SOA Suite,
* IBM WebSphere.

SOAP tích hợp rất tốt với các nền tảng này.

---

## 3.2 Nhược điểm của SOAP

## a) Hiệu suất thấp hơn REST

SOAP dùng XML:

* verbose,
* payload lớn,
* parsing nặng.

Kết quả:

* tốn bandwidth,
* CPU cao hơn,
* latency lớn hơn.

---

## b) Phát triển phức tạp

SOAP yêu cầu:

* WSDL,
* XSD,
* contract management.

Developer phải xử lý:

* XML namespace,
* schema validation,
* WS-* standards.

Chi phí phát triển cao hơn REST.

---

## c) Khó linh hoạt

SOAP thay đổi API chậm hơn:

* contract-first,
* strict schema.

Không phù hợp với:

* agile iteration nhanh,
* release liên tục.

---

# 4. Phân tích REST cho Microservices

## 4.1 Ưu điểm của REST với Microservices

## a) Hiệu suất cao

REST thường dùng JSON:

* nhẹ hơn XML,
* parse nhanh,
* payload nhỏ.

Rất phù hợp:

* mobile apps,
* frontend web,
* API gateway.

### Lợi ích

* giảm latency,
* tăng throughput,
* giảm bandwidth.

---

## b) Stateless → Scalability tốt

REST stateless:

* server không lưu session.

Điều này giúp:

* scale horizontal dễ dàng,
* auto-scaling trên cloud/Kubernetes.

Rất phù hợp với microservices.

---

## c) Phát triển nhanh

REST:

* đơn giản,
* dễ test,
* dễ debug.

Developer có thể:

* build nhanh,
* deploy nhanh,
* CI/CD dễ dàng.

---

## d) Tích hợp frontend cực tốt

Frontend/mobile:

* React,
* Angular,
* Flutter,
* iOS,
* Android

đều tối ưu cho REST + JSON.

---

## e) Hệ sinh thái cloud-native mạnh

REST hoạt động rất tốt với:

* Docker,
* Kubernetes,
* API Gateway,
* Service Mesh,
* Serverless.

---

## 4.2 Nhược điểm của REST

## a) Không mạnh về transaction

REST:

* không có chuẩn transaction native như SOAP.

Distributed transaction trong microservices:

* phải dùng Saga Pattern,
* Eventual Consistency.

Điều này phức tạp hơn SOAP.

---

## b) Security không chuẩn hóa enterprise bằng SOAP

REST thường dùng:

* OAuth2,
* JWT,
* HTTPS.

Dù đủ mạnh cho web/mobile,
nhưng:

* không đầy đủ WS-Security enterprise-level như SOAP.

---

## c) Reliability cần tự triển khai

REST không có:

* WS-ReliableMessaging.

Developer phải tự xử lý:

* retry,
* idempotency,
* circuit breaker.

---

# 5. So sánh SOAP vs REST theo yêu cầu nghiệp vụ

| Tiêu chí                | SOAP                   | REST        |
| ----------------------- | ---------------------- | ----------- |
| Data Format             | XML                    | JSON/XML    |
| Hiệu suất               | Thấp hơn               | Cao hơn     |
| Payload                 | Nặng                   | Nhẹ         |
| Security                | Rất mạnh (WS-Security) | OAuth2/JWT  |
| Distributed Transaction | Tốt                    | Hạn chế     |
| Reliability             | Enterprise-grade       | Phải tự xây |
| Scalability             | Trung bình             | Rất tốt     |
| Development Speed       | Chậm                   | Nhanh       |
| Flexibility             | Thấp                   | Cao         |
| Mobile/Web Support      | Không tối ưu           | Rất tốt     |
| Legacy Integration      | Rất mạnh               | Trung bình  |
| Cloud-native            | Hạn chế                | Rất phù hợp |

---

# 6. Đề xuất kiến trúc cho công ty

# 6.1 Legacy Core Banking → Chọn SOAP

## Lý do chính

### a) Transaction integrity

Core banking cần:

* ACID,
* rollback,
* consistency.

SOAP hỗ trợ tốt hơn.

---

### b) Security compliance

Ngành tài chính yêu cầu:

* audit,
* message signing,
* encryption,
* non-repudiation.

SOAP + WS-Security là lựa chọn enterprise phù hợp.

---

### c) Reliability

Không được phép:

* mất message,
* duplicate transaction.

SOAP hỗ trợ reliability mạnh hơn.

---

### d) Tương thích hệ thống cũ

Giảm:

* chi phí migration,
* rủi ro chuyển đổi.

---

# 6.2 Microservices mới → Chọn REST

## Lý do chính

### a) High performance

REST + JSON:

* nhanh,
* nhẹ,
* tối ưu mobile/web.

---

### b) Scalability

Microservices cần:

* auto-scaling,
* container orchestration.

REST phù hợp cloud-native.

---

### c) Agile development

REST:

* dễ build,
* dễ deploy,
* hỗ trợ DevOps tốt.

---

### d) Frontend integration

REST là chuẩn phổ biến cho:

* SPA,
* mobile app,
* public API.

---

# 7. Kiến trúc khuyến nghị tổng thể

## Mô hình Hybrid Architecture

### Đề xuất

```text
Mobile/Web Apps
       |
   REST APIs
       |
API Gateway
       |
Microservices Layer (REST)
       |
Integration Layer / ESB
       |
SOAP Services
       |
Legacy Core Banking
```

---

# 8. Giải thích mô hình Hybrid

## REST cho tầng ngoài

Dùng cho:

* frontend,
* mobile,
* public API,
* microservices.

Mục tiêu:

* tốc độ,
* scalability,
* trải nghiệm người dùng.

---

## SOAP cho tầng lõi tài chính

Dùng cho:

* giao dịch ngân hàng,
* settlement,
* payment processing.

Mục tiêu:

* consistency,
* security,
* reliability.

---

## Integration Layer / ESB

Đóng vai trò:

* chuyển đổi REST ↔ SOAP,
* orchestration,
* security mediation.

Ví dụ:

* MuleSoft,
* WSO2,
* IBM Integration Bus.

---

# 9. Rủi ro nếu chọn sai

## Nếu dùng REST cho Core Banking

Rủi ro:

* transaction inconsistency,
* security gap,
* mất reliability.

Hậu quả:

* lỗi giao dịch tài chính,
* vi phạm compliance.

---

## Nếu dùng SOAP cho Microservices

Rủi ro:

* chậm phát triển,
* hiệu suất thấp,
* scaling khó khăn.

Hậu quả:

* trải nghiệm người dùng kém,
* tăng chi phí cloud.

---

# 10. Kết luận cuối cùng

## Khuyến nghị chính thức

| Hệ thống                     | Công nghệ đề xuất |
| ---------------------------- | ----------------- |
| Legacy Core Banking          | SOAP              |
| Microservices / Mobile / Web | REST              |

---

## Lý do cốt lõi

### SOAP phù hợp vì:

* bảo mật enterprise mạnh,
* transaction reliability cao,
* hỗ trợ distributed transaction,
* phù hợp hệ thống tài chính lõi.

### REST phù hợp vì:

* hiệu suất cao,
* scalability mạnh,
* phát triển nhanh,
* tối ưu cloud-native và mobile/web.

---

# 11. Glossary (Thuật ngữ cốt lõi)

| Thuật ngữ               | Giải thích                                    |
| ----------------------- | --------------------------------------------- |
| SOAP                    | Giao thức Web Service dùng XML                |
| REST                    | Kiến trúc Web Service dựa trên HTTP           |
| WSDL                    | File mô tả SOAP Service                       |
| XML                     | Định dạng dữ liệu dạng markup                 |
| JSON                    | Định dạng dữ liệu nhẹ                         |
| Stateless               | Server không lưu trạng thái client            |
| WS-Security             | Chuẩn bảo mật của SOAP                        |
| OAuth2                  | Chuẩn xác thực phổ biến cho REST              |
| JWT                     | JSON Web Token                                |
| ACID                    | Atomicity, Consistency, Isolation, Durability |
| Distributed Transaction | Giao dịch phân tán nhiều hệ thống             |
| API Gateway             | Cổng quản lý API                              |
| ESB                     | Enterprise Service Bus                        |
| Scalability             | Khả năng mở rộng                              |
| Reliability             | Độ tin cậy truyền message                     |
| Microservices           | Kiến trúc chia nhỏ dịch vụ                    |

---

# 12. Tổng kết ngắn gọn

* **SOAP** nên dùng cho:

  * core banking,
  * giao dịch tài chính,
  * enterprise integration.

* **REST** nên dùng cho:

  * microservices,
  * mobile/web APIs,
  * cloud-native systems.

* Giải pháp tối ưu cho công ty tài chính là:

  ## “Hybrid Architecture: REST bên ngoài + SOAP ở lõi giao dịch.”
