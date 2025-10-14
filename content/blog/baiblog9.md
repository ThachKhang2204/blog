---
title: "Full-Stack Thực Chiến: Kết Nối React Frontend với Spring Boot Backend"
date: 2025-10-13
tags: ["react", "spring-boot", "full-stack", "api", "axios", "tutorial"]
---

Ở các bài trước, chúng ta đã xây dựng một **CRUD API** mạnh mẽ với Spring Boot và tìm hiểu cách tạo giao diện người dùng với **React**. Bây giờ là lúc kết nối hai mảnh ghép này lại để tạo thành một ứng dụng full-stack hoàn chỉnh. Trong bài này, chúng ta sẽ xây dựng một giao diện React đơn giản để hiển thị, thêm, và xóa sản phẩm thông qua API backend. 🔗

---

## 1. Chuẩn bị môi trường React

Nếu chưa có, bạn hãy tạo một dự án React mới bằng công cụ `create-react-app`.

```bash
npx create-react-app my-product-app
cd my-product-app
```

Để giao tiếp với API, chúng ta sẽ cài đặt Axios – một HTTP client phổ biến.

```bash
npm install axios
```

---

## 2. Xây dựng Component hiển thị danh sách sản phẩm

Chúng ta sẽ tạo file `ProductList.js` để:

- Lấy dữ liệu sản phẩm từ API bằng **axios**
- Lưu vào state với **useState**
- Hiển thị lên giao diện bằng `.map()`

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const API_URL = 'http://localhost:8080/api/products';

function ProductList() {
    const [products, setProducts] = useState([]);

    useEffect(() => {
        axios.get(API_URL)
            .then(response => setProducts(response.data))
            .catch(error => console.error('Error fetching products!', error));
    }, []);

    return (
        <div>
            <h1>Danh sách sản phẩm</h1>
            <ul>
                {products.map(product => (
                    <li key={product.id}>
                        {product.name} - ${product.price}
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default ProductList;
```

---

## 3. Thêm chức năng Thêm & Xóa sản phẩm

Tiếp theo, chúng ta mở rộng `ProductList.js` để thêm sản phẩm (POST) và xóa sản phẩm (DELETE).

```javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const API_URL = 'http://localhost:8080/api/products';

function ProductList() {
    const [products, setProducts] = useState([]);
    const [newProductName, setNewProductName] = useState('');
    const [newProductPrice, setNewProductPrice] = useState(0);

    const fetchProducts = () => {
        axios.get(API_URL).then(res => setProducts(res.data));
    };

    useEffect(() => {
        fetchProducts();
    }, []);

    const handleAddProduct = () => {
        const newProduct = { name: newProductName, price: newProductPrice };
        axios.post(API_URL, newProduct).then(() => {
            fetchProducts();
            setNewProductName('');
            setNewProductPrice(0);
        });
    };

    const handleDeleteProduct = (id) => {
        axios.delete(`${API_URL}/${id}`).then(() => {
            fetchProducts();
        });
    };

    return (
        <div>
            <h1>Quản lý sản phẩm</h1>

            <div>
                <input
                    type="text"
                    value={newProductName}
                    onChange={e => setNewProductName(e.target.value)}
                    placeholder="Tên sản phẩm"
                />
                <input
                    type="number"
                    value={newProductPrice}
                    onChange={e => setNewProductPrice(e.target.value)}
                    placeholder="Giá"
                />
                <button onClick={handleAddProduct}>Thêm sản phẩm</button>
            </div>

            <ul>
                {products.map(product => (
                    <li key={product.id}>
                        {product.name} - ${product.price}
                        <button onClick={() => handleDeleteProduct(product.id)}>Xóa</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default ProductList;
```

---

## 4. Lưu ý về CORS

Để frontend React (cổng 3000) gọi API backend Spring Boot (cổng 8080), hãy đảm bảo bạn đã bật CORS trong Controller:

```java
@CrossOrigin(origins = "http://localhost:3000")
```

---

## 5. Kết luận

Chúc mừng! Bạn đã kết nối thành công **React + Spring Boot** để tạo thành một ứng dụng Full-Stack thực thụ. Từ đây, bạn có thể mở rộng với:

- Cập nhật sản phẩm (PUT)
- Tìm kiếm, phân trang
- Bảo mật với JWT & Spring Security

Trong bài tiếp theo, chúng ta sẽ tích hợp **Spring Security + JWT** để bảo vệ API.
