# Phân tích vai trò của HTTP Status Code trong REST API

## 1. Vai trò của HTTP Status Codes

HTTP Status Codes giúp client hiểu chính xác kết quả xử lý request.

Ví dụ:
- 200 OK → Request thành công
- 201 Created → Tạo dữ liệu thành công
- 204 No Content → Xóa thành công
- 404 Not Found → Không tìm thấy dữ liệu
- 500 Internal Server Error → Lỗi phía server

Nhờ đó:
- Client xử lý lỗi dễ dàng
- Frontend hiển thị thông báo chính xác
- API trở nên rõ ràng và chuẩn RESTful hơn

---

## 2. Vì sao không nên trả về null?

Nếu API trả về null:

```java
return null;
