---
title: "ReactJS – Xây Dựng Giao Diện Người Dùng Theo Cách Của Facebook"
date: 2025-10-13
tags: ["javascript", "react", "frontend", "ui"]
---

Trong thế giới frontend, sự thay đổi diễn ra chóng mặt. Nhưng giữa hàng loạt các thư viện và framework, **ReactJS** nổi lên như một thế lực thống trị. Được tạo ra và bảo trì bởi Facebook, React đã thay đổi hoàn toàn cách chúng ta suy nghĩ và xây dựng giao diện người dùng (UI) cho các ứng dụng web hiện đại.

---

## 1. React là gì? (Và nó KHÔNG phải là một framework)

Một điểm quan trọng cần làm rõ: React là một **thư viện (library)** JavaScript, không phải là một **framework** hoàn chỉnh như Angular hay Vue.

* **Thư viện:** Cung cấp các công cụ để giải quyết một vấn đề cụ thể, đó là xây dựng giao diện người dùng. Bạn có toàn quyền quyết định các công cụ khác đi kèm.
* **Framework:** Cung cấp một bộ khung sườn hoàn chỉnh và áp đặt các quy tắc về cách xây dựng ứng dụng.

Triết lý chính của React là xây dựng UI từ các mảnh ghép nhỏ, độc lập và có thể tái sử dụng được gọi là **Components**.

---

## 2. Các khái niệm cốt lõi làm nên sức mạnh của React

Để hiểu React, bạn cần nắm vững các khái niệm nền tảng sau.

| Khái niệm | Mô tả |
|---|---|
| **Components** | Các khối xây dựng cơ bản của UI. Mỗi component là một hàm JavaScript trả về mã trông giống HTML (gọi là JSX). Ví dụ: `Button`, `Header`, `UserProfile`. |
| **JSX (JavaScript XML)** | Một phần mở rộng cú pháp, cho phép viết mã giống HTML ngay trong file JavaScript, giúp việc mô tả UI trở nên trực quan. |
| **Props & State** | **Props** là dữ liệu truyền từ cha xuống con (chỉ đọc). **State** là dữ liệu nội tại của component, khi thay đổi sẽ khiến component render lại. |
| **Virtual DOM** | React tạo một bản sao của DOM trong bộ nhớ. Khi state thay đổi, nó sẽ so sánh và chỉ cập nhật những thay đổi cần thiết lên DOM thật, giúp tăng hiệu năng. |

---

## 3. Ví dụ về một Component React đơn giản

Hãy xem một component `Counter` đơn giản sử dụng **React Hooks** (`useState`), một tính năng hiện đại giúp quản lý state.

## 4. Bảng công cụ cần thiết cho dự án

Nhiệm vụ                   | Backend (Spring Boot)             | Frontend (React)
---------------------------|-----------------------------------|-------------------------
Quản lý dự án              | Maven hoặc Gradle                 | npm hoặc Yarn
Xây dựng API               | Spring Web (RestControllers)      | -
Tương tác CSDL             | Spring Data JPA, Hibernate        | -
Gọi API                    | -                                 | Axios, Fetch API
Quản lý State              | -                                 | Context API, Redux
Bảo mật                    | Spring Security                   | JWT (JSON Web Tokens)

## 5. Kết luận

Kết hợp Spring Boot và React là một lựa chọn mạnh mẽ và phổ biến để xây dựng các ứng dụng web hiện đại. 
Việc tách biệt rõ ràng giữa backend và frontend không chỉ giúp dự án dễ quản lý, phát triển song song,
mà còn tạo ra các hệ thống linh hoạt, dễ dàng mở rộng và bảo trì trong tương lai.

Trong bài viết cuối cùng của chuỗi bài này, chúng ta sẽ nhìn về tương lai:
những xu hướng mới nhất trong hệ sinh thái Java và JavaScript.


```jsx
import React, { useState } from 'react';

function Counter() {
    // Khai báo một state mới, đặt tên là "count"
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Bạn đã nhấn {count} lần</p>
            <button onClick={() => setCount(count + 1)}>
                Nhấn vào tôi
            </button>
        </div>
    );
}