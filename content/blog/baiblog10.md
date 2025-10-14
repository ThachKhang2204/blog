---
title: "Bảo Mật API: Giới Thiệu Spring Security và JWT cho Backend"
date: 2025-10-13
tags: ["java", "spring-boot", "spring-security", "jwt", "backend", "security"]
---

Chúng ta đã xây dựng một CRUD API hoạt động tốt, nhưng nó vẫn còn một lỗ hổng lớn: **bất kỳ ai cũng có thể truy cập và thay đổi dữ liệu mà không cần xác thực**. Trong thế giới thực, đây là điều không thể chấp nhận. Bài viết này sẽ giới thiệu cách bảo vệ các endpoint của bạn bằng **Spring Security** kết hợp với **JSON Web Tokens (JWT)**, một trong những phương pháp phổ biến và hiệu quả nhất hiện nay. 🔐

---

## 1. Tại sao phải bảo mật API?

Một API không được bảo vệ giống như một ngôi nhà không khóa cửa. Nó cho phép bất kỳ ai cũng có thể thực hiện các hành động nguy hiểm như:

- Đọc dữ liệu nhạy cảm của người dùng khác.
- Tạo ra dữ liệu giả mạo hoặc spam.
- Sửa đổi hoặc xóa các dữ liệu quan trọng, gây ảnh hưởng đến toàn bộ hệ thống.

**Spring Security** là một framework mạnh mẽ và linh hoạt, giúp bạn dễ dàng thêm các lớp xác thực (Authentication) và phân quyền (Authorization) vào ứng dụng Spring.

---

## 2. JWT là gì và hoạt động như thế nào?

**JSON Web Token (JWT)** là một chuỗi mã hóa nhỏ gọn, chứa thông tin về người dùng (ví dụ: username, quyền hạn). Nó hoạt động như một "thẻ ra vào" kỹ thuật số.

**Luồng hoạt động cơ bản:**

1. **Đăng nhập:** Client gửi `username` và `password` đến endpoint login (`/api/login`).
2. **Xác thực:** Backend kiểm tra thông tin. Nếu hợp lệ, tạo và trả về một JWT.
3. **Lưu trữ:** Client lưu JWT (trong `localStorage` hoặc `sessionStorage`).
4. **Gửi yêu cầu:** Client gửi JWT trong Header:  
   `Authorization: Bearer <token>`
5. **Ủy quyền:** Backend kiểm tra JWT, nếu hợp lệ sẽ cho phép truy cập tài nguyên.

---

## 3. Cài đặt và cấu hình Spring Security

Thêm các dependencies cần thiết vào `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

Sau đó, bạn cần tạo lớp cấu hình để kiểm soát quyền truy cập vào API, chỉ định endpoint công khai (như `/login`) và endpoint yêu cầu xác thực.

---

## 4. Các thành phần chính cần xây dựng

| Thành phần | Chức năng |
|------------|----------|
| **JwtUtil / JwtProvider** | Tạo, giải mã và xác thực JWT |
| **UserDetailsServiceImpl** | Tải thông tin người dùng từ DB |
| **JwtRequestFilter** | Chặn mỗi request, kiểm tra Header có JWT không |
| **SecurityConfig** | Cấu hình Spring Security, khai báo quy tắc bảo mật |

---

## 5. Kết luận

Bảo mật là một phần **không thể thiếu** của bất kỳ ứng dụng backend chuyên nghiệp nào. Việc thiết lập **Spring Security + JWT** có thể phức tạp ban đầu, nhưng đổi lại bạn nhận được:

✅ API an toàn  
✅ Quản lý truy cập theo vai trò (Role-Based Access Control)  
✅ Kiến trúc sẵn sàng cho hệ thống lớn (Microservices, Cloud)

Đây là bài viết **kết thúc chuỗi Java & JavaScript cơ bản**. Nếu bạn đã đi đến đây – bạn đã nắm vững nền tảng để trở thành một lập trình viên full-stack thực thụ! 🚀

Hẹn gặp lại ở các bài nâng cao tiếp theo về **JWT Refresh Token, Role Permission, và Deployment thực tế**.
