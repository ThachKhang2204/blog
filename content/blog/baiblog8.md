---
title: "Thực Hành: Xây Dựng CRUD API Đầu Tiên với Spring Boot và JPA"
date: 2025-10-13
tags: ["java", "spring-boot", "backend", "api", "crud", "tutorial"]
---

Sau khi đã tìm hiểu về các khái niệm lý thuyết, đã đến lúc chúng ta bắt tay vào thực hành. Trong bài này, chúng ta sẽ xây dựng một ứng dụng web cơ bản nhất nhưng cũng quan trọng nhất: một **RESTful API** hỗ trợ các thao tác **CRUD (Create, Read, Update, Delete)**. Chúng ta sẽ sử dụng Spring Boot và Spring Data JPA để quản lý một danh sách sản phẩm đơn giản. 🛠️

---

## 1. CRUD là gì và tại sao nó quan trọng?

CRUD là viết tắt của bốn hoạt động cơ bản trong việc quản lý dữ liệu lâu dài. Hầu hết mọi ứng dụng, từ mạng xã hội đến trang thương mại điện tử, đều được xây dựng dựa trên bốn thao tác này.

| Thao tác | Tên gọi | HTTP Method tương ứng | Mô tả |
|---|---|---|---|
| **C**reate | Tạo mới | `POST` | Thêm một bản ghi dữ liệu mới (ví dụ: đăng một bài viết mới). |
| **R**ead | Đọc | `GET` | Lấy một hoặc nhiều bản ghi dữ liệu (ví dụ: xem danh sách sản phẩm). |
| **U**pdate | Cập nhật | `PUT` / `PATCH` | Chỉnh sửa một bản ghi dữ liệu đã có (ví dụ: thay đổi giá sản phẩm). |
| **D**elete | Xóa | `DELETE` | Xóa một bản ghi dữ liệu (ví dụ: xóa một bình luận). |

---

## 2. Chuẩn bị dự án

Chúng ta sẽ tiếp tục sử dụng **Spring Initializr** (`start.spring.io`) để tạo dự án.

**Cấu hình cần thiết:**
* Project: **Maven**
* Language: **Java**
* Dependencies:
    1.  **Spring Web:** Để xây dựng các RESTful API.
    2.  **Spring Data JPA:** Để tương tác với cơ sở dữ liệu một cách dễ dàng.
    3.  **H2 Database:** Một cơ sở dữ liệu trong bộ nhớ (in-memory) rất tiện lợi cho việc phát triển và thử nghiệm nhanh.

Sau khi tạo, hãy mở dự án bằng IDE của bạn.

---

## 3. Xây dựng các tầng ứng dụng

Một ứng dụng Spring Boot thường được cấu trúc theo các tầng (layers) rõ ràng.

| Tầng | Mô tả |
|---|---|
| **Entity (Model)** | Định nghĩa đối tượng dữ liệu. Đây là một lớp Java được ánh xạ tới một bảng trong cơ sở dữ liệu. |
| **Repository** | Tầng truy cập dữ liệu. Spring Data JPA sẽ tự động tạo các phương thức CRUD cơ bản cho chúng ta. |
| **Controller** | Tầng tiếp nhận và xử lý các yêu cầu HTTP từ người dùng, sau đó trả về kết quả. |

**Ví dụ về lớp Entity `Product`:**
Ví dụ về Interface ProductRepository:

Java

public interface ProductRepository extends JpaRepository<Product, Long> {
    // Spring Data JPA tự động cung cấp các phương thức như save(), findById(), findAll(), deleteById()
}

## 4. Viết API Controller cho các thao tác CRUD
Đây là nơi chúng ta định nghĩa các "endpoint" (URL) cho API của mình.

Java

@RestController
@RequestMapping("/api/products") // Tiền tố chung cho tất cả các endpoint
public class ProductController {

    @Autowired
    private ProductRepository productRepository;

    // 1. CREATE a new product
    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productRepository.save(product);
    }

    // 2. READ all products
    @GetMapping
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    // 3. READ a single product by ID
    @GetMapping("/{id}")
    public Product getProductById(@PathVariable Long id) {
        return productRepository.findById(id).orElse(null); // Trả về null nếu không tìm thấy
    }

    // 4. UPDATE a product
    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product productDetails) {
        Product product = productRepository.findById(id).orElse(null);
        if (product != null) {
            product.setName(productDetails.getName());
            product.setPrice(productDetails.getPrice());
            return productRepository.save(product);
        }
        return null;
    }

    // 5. DELETE a product
    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productRepository.deleteById(id);
    }
}

## 5. Kết luận
Chỉ với vài bước đơn giản, chúng ta đã xây dựng thành công một RESTful API hoàn chỉnh với Spring Boot.
Bạn có thể chạy ứng dụng và dùng các công cụ như Postman hoặc cURL để kiểm tra các endpoint vừa tạo.
Đây là nền tảng cơ bản để xây dựng bất kỳ ứng dụng backend phức tạp nào.

Trong bài tiếp theo, chúng ta sẽ tìm hiểu cách kết nối ứng dụng React ở frontend
với API backend này để tạo thành một ứng dụng full-stack hoàn chỉnh.

```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;
    
    // Getters and Setters
}