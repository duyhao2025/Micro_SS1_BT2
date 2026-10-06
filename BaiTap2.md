# Bài tập 2 - Cân nhắc bài toán đánh đổi với Microservices

## 1. Bối cảnh

Microservices là một kiến trúc trong đó hệ thống được chia thành nhiều service nhỏ, mỗi service đảm nhiệm một nghiệp vụ cụ thể và có thể được phát triển, triển khai và mở rộng tương đối độc lập.

Tuy nhiên, Microservices không phải là giải pháp phù hợp cho mọi hệ thống. Việc chuyển đổi từ Monolith sang Microservices cần cân nhắc giữa lợi ích nhận được và chi phí, độ phức tạp phát sinh.

---

## 2. So sánh Monolith và Microservices

| Tiêu chí    | Monolith                             | Microservices                     |
| ----------- | ------------------------------------ | --------------------------------- |
| Cấu trúc    | Một ứng dụng thống nhất              | Nhiều service độc lập             |
| Triển khai  | Thường triển khai toàn bộ hệ thống   | Có thể triển khai từng service    |
| Mở rộng     | Thường phải scale toàn bộ ứng dụng   | Có thể scale từng service         |
| Giao tiếp   | Chủ yếu gọi trực tiếp trong ứng dụng | Giao tiếp qua network/API/message |
| Database    | Thường dùng chung database           | Có thể sở hữu database riêng      |
| Độ phức tạp | Thấp hơn                             | Cao hơn                           |
| Vận hành    | Đơn giản hơn                         | Cần monitoring, logging, DevOps   |
| Phù hợp     | Hệ thống nhỏ và vừa                  | Hệ thống lớn, phức tạp            |

---

## 3. Ưu điểm của Microservices

### 3.1. Dễ mở rộng

Mỗi service có thể được mở rộng độc lập tùy theo nhu cầu.

Ví dụ, hệ thống thương mại điện tử có lượng truy cập vào `Product Service` cao hơn `User Service`. Có thể scale riêng `Product Service` mà không cần scale toàn bộ hệ thống.

### 3.2. Triển khai độc lập

Một service có thể được cập nhật hoặc triển khai mà không nhất thiết phải triển khai lại toàn bộ hệ thống.

Điều này giúp team phát hành tính năng nhanh hơn.

### 3.3. Độc lập về công nghệ

Mỗi service có thể sử dụng công nghệ phù hợp với nghiệp vụ của mình.

Ví dụ:

* Java/Spring Boot cho Order Service.
* Go cho một service cần hiệu năng cao.
* Python cho một service liên quan đến Machine Learning.

### 3.4. Phân chia trách nhiệm rõ ràng

Mỗi service tập trung vào một nghiệp vụ cụ thể.

Ví dụ:

```text
User Service
Product Service
Order Service
Payment Service
Shipping Service
```

Điều này giúp các team dễ tập trung vào nghiệp vụ mà mình phụ trách.

### 3.5. Giảm phạm vi ảnh hưởng khi có lỗi

Nếu một service gặp sự cố, về lý thuyết các service khác vẫn có thể tiếp tục hoạt động nếu hệ thống được thiết kế tốt.

Ví dụ, `Notification Service` bị lỗi thì chức năng đặt hàng vẫn có thể tiếp tục hoạt động.

---

## 4. Nhược điểm và thách thức của Microservices

### 4.1. Độ trễ mạng

Trong Monolith, các module có thể gọi nhau trực tiếp trong cùng một ứng dụng.

Trong Microservices, các service thường phải giao tiếp qua network nên có thể phát sinh:

* Network latency.
* Timeout.
* Connection failure.
* Retry và circuit breaker.

### 4.2. Quản lý dữ liệu phức tạp

Khi mỗi service có database riêng, việc đảm bảo tính nhất quán dữ liệu trở nên khó khăn hơn.

Ví dụ:

```text
Order Service
      ↓
Payment Service
      ↓
Inventory Service
```

Nếu thanh toán thành công nhưng cập nhật kho thất bại, hệ thống cần cơ chế xử lý để đảm bảo trạng thái nghiệp vụ hợp lý.

### 4.3. Vận hành phức tạp

Một Monolith có thể chỉ cần triển khai một ứng dụng.

Trong Microservices có thể phải quản lý hàng chục hoặc hàng trăm service cùng với:

* Docker/Kubernetes.
* Monitoring.
* Logging.
* Service Discovery.
* API Gateway.
* Message Broker.
* CI/CD.

### 4.4. Khó debug

Một request của người dùng có thể đi qua nhiều service.

Ví dụ:

```text
Client
  ↓
API Gateway
  ↓
Order Service
  ↓
Payment Service
  ↓
Inventory Service
```

Khi xảy ra lỗi, developer phải theo dõi toàn bộ luồng request thay vì chỉ kiểm tra một ứng dụng.

### 4.5. Chi phí phát triển và vận hành cao hơn

Microservices yêu cầu nhiều kiến thức và công cụ hơn so với Monolith.

Doanh nghiệp có thể phải đầu tư thêm vào:

* DevOps.
* Cloud infrastructure.
* Monitoring.
* Logging.
* CI/CD.
* Nhân sự có kinh nghiệm.

---

## 5. Microservices không phải là "viên đạn bạc"

Microservices không phải lúc nào cũng tốt hơn Monolith.

Nếu doanh nghiệp có:

* Hệ thống nhỏ.
* Team ít người.
* Nghiệp vụ chưa ổn định.
* Lượng người dùng chưa lớn.
* Hạ tầng và DevOps còn hạn chế.

thì việc sử dụng Microservices ngay từ đầu có thể tạo ra nhiều sự phức tạp không cần thiết.

Trong trường hợp này, một kiến trúc Monolith được thiết kế tốt có thể là lựa chọn phù hợp hơn.

Ngược lại, với hệ thống lớn có nhiều team, nghiệp vụ phức tạp và yêu cầu mở rộng độc lập, Microservices có thể mang lại nhiều lợi ích.

---

## 6. Kết luận

Microservices mang lại nhiều lợi ích như khả năng mở rộng độc lập, triển khai độc lập và phân chia nghiệp vụ rõ ràng. Tuy nhiên, nó cũng tạo ra nhiều thách thức về network, dữ liệu phân tán, vận hành, monitoring và debugging.

Vì vậy, doanh nghiệp không nên chuyển sang Microservices chỉ vì đây là một kiến trúc phổ biến. Cần đánh giá quy mô hệ thống, nghiệp vụ, nhân sự, chi phí và yêu cầu thực tế trước khi lựa chọn.

**Microservices không phải là "viên đạn bạc". Kiến trúc tốt nhất là kiến trúc phù hợp với bài toán thực tế của doanh nghiệp.**
