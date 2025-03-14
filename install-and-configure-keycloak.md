---
icon: sign-posts-wrench
---

# Install & configure Keycloak

### 1. Cài đặt Standalone Server với Java

Đây là phương pháp truyền thống và thích hợp cho các môi trường phát triển hoặc triển khai quy mô nhỏ.

#### Yêu cầu hệ thống:

* Java 11+ (JDK 17 được khuyến nghị cho phiên bản mới nhất)
* RAM tối thiểu 512MB (khuyến nghị 1GB+)
* 1GB+ dung lượng ổ đĩa trống

#### Cài đặt từng bước:

**Bước 1: Tải và giải nén Keycloak**

```bash
# Tải phiên bản mới nhất
wget https://github.com/keycloak/keycloak/releases/download/21.1.1/keycloak-21.1.1.zip

# Giải nén
unzip keycloak-21.1.1.zip

# Di chuyển vào thư mục
cd keycloak-21.1.1
```

**Bước 2: Khởi động ở chế độ development (để kiểm thử)**

```bash
bin/kc.sh start-dev
```

Lưu ý: Chế độ này không nên sử dụng cho môi trường production, nhưng rất thuận tiện để thử nghiệm nhanh.

**Bước 3: Cấu hình cho môi trường production**

```bash
# Tạo cấu hình ban đầu
bin/kc.sh build

# Tạo tài khoản admin (nếu cần)
# Bạn sẽ được nhắc tạo trong lần đầu truy cập
```

**Bước 4: Khởi động ở chế độ production**

```bash
bin/kc.sh start
```

#### Cấu hình nâng cao:

Để tùy chỉnh cấu hình, bạn có thể chỉnh sửa file conf/keycloak.conf hoặc sử dụng các tham số khi khởi động:

```bash
bin/kc.sh start \
  --hostname=keycloak.example.com \
  --https-certificate-file=/path/to/cert.pem \
  --https-certificate-key-file=/path/to/key.pem \
  --http-port=8080 \
  --https-port=8443
```

#### Ưu điểm:

* Kiểm soát hoàn toàn môi trường và cấu hình
* Hiệu suất tốt hơn so với container
* Dễ dàng sao lưu và khôi phục
* Phù hợp cho việc phát triển và thử nghiệm

#### Nhược điểm:

* Cần quản lý Java runtime thủ công
* Khó scale theo chiều ngang
* Quản lý phức tạp hơn khi có nhiều instance
* Cần cấu hình load balancer riêng nếu muốn HA

### 2. Triển khai bằng Docker Container

Docker là một phương pháp triển khai phổ biến do tính đơn giản và tính nhất quán giữa các môi trường.

#### Yêu cầu:

* Docker Engine
* Ít nhất 1GB RAM cho container
* Một số kiến thức về Docker

#### Cài đặt cơ bản với Docker:

**Bước 1: Chạy Keycloak ở chế độ development**

```bash
docker run -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:21.1.1 start-dev
```

**Bước 2: Chạy Keycloak với PostgreSQL cho môi trường production**

```bash
# Tạo network để các container giao tiếp
docker network create keycloak-network

# Chạy PostgreSQL
docker run -d --name postgres \
  --network keycloak-network \
  -e POSTGRES_DB=keycloak \
  -e POSTGRES_USER=keycloak \
  -e POSTGRES_PASSWORD=password \
  -v postgres_data:/var/lib/postgresql/data \
  postgres:14

# Chạy Keycloak
docker run -d --name keycloak \
  --network keycloak-network \
  -p 8080:8080 -p 8443:8443 \
  -e KC_DB=postgres \
  -e KC_DB_URL=jdbc:postgresql://postgres:5432/keycloak \
  -e KC_DB_USERNAME=keycloak \
  -e KC_DB_PASSWORD=password \
  -e KC_HOSTNAME=localhost \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=password \
  quay.io/keycloak/keycloak:21.1.1 \
  start
```

**Bước 3: Sử dụng Docker Compose cho môi trường production**

Tạo file `docker-compose.yml`:

```yaml
version: '3'

services:
  postgres:
    image: postgres:14
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: password
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "keycloak"]
      interval: 5s
      timeout: 5s
      retries: 5

  keycloak:
    image: quay.io/keycloak/keycloak:21.1.1
    command:
      - start
    environment:
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/keycloak
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: password
      KC_HOSTNAME: localhost
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
      # Production options
      KC_HTTP_ENABLED: "true"
      KC_HTTPS_ENABLED: "true"
      KC_HTTPS_CERTIFICATE_FILE: /opt/keycloak/conf/cert.pem
      KC_HTTPS_CERTIFICATE_KEY_FILE: /opt/keycloak/conf/key.pem
    ports:
      - "8080:8080"
      - "8443:8443"
    depends_on:
      postgres:
        condition: service_healthy
    volumes:
      - ./certs:/opt/keycloak/conf

volumes:
  postgres_data:
```

Và khởi động với:

```bash
docker-compose up -d
```

#### Ưu điểm:

* Triển khai đơn giản và nhanh chóng
* Nhất quán giữa các môi trường
* Dễ dàng tích hợp vào CI/CD pipeline
* Cô lập dependencies
* Dễ dàng nâng cấp phiên bản

#### Nhược điểm:

* Hiệu suất thấp hơn một chút so với bản standalone
* Cần hiểu về Docker để tùy chỉnh nâng cao
* Quản lý persistent storage đôi khi phức tạp
* Bảo mật cần được cấu hình cẩn thận

### 3. Triển khai trên Kubernetes/OpenShift

Kubernetes và OpenShift mang lại khả năng mở rộng, tính sẵn sàng cao và tự động hóa quản lý cho Keycloak trong môi trường doanh nghiệp.

#### Yêu cầu:

* Cluster Kubernetes/OpenShift đang hoạt động
* kubectl hoặc oc CLI đã cài đặt
* Helm (không bắt buộc nhưng khuyến nghị)
* Kiến thức về Kubernetes/OpenShift

#### Cài đặt với Helm Chart:

**Bước 1: Thêm Bitnami repository và cập nhật**

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

**Bước 2: Tạo file values.yaml cho Helm chart**

```yaml
keycloak:
  auth:
    adminUser: admin
    adminPassword: password
  
  service:
    type: ClusterIP
  
  ingress:
    enabled: true
    hostname: keycloak.example.com
    tls: true
  
  postgresql:
    enabled: true
    auth:
      username: keycloak
      password: password
      database: keycloak
  
  resources:
    requests:
      cpu: "500m"
      memory: "1Gi"
    limits:
      cpu: "1"
      memory: "2Gi"
  
  replicas: 2
```

**Bước 3: Cài đặt Keycloak sử dụng Helm**

```bash
helm install keycloak bitnami/keycloak \
  -f values.yaml \
  -n keycloak \
  --create-namespace
```

#### Cài đặt với OpenShift Operator:

**Bước 1: Cài đặt Keycloak Operator**

```bash
oc create -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/main/kubernetes/keycloaks.k8s.keycloak.org-v1.yml
oc create -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/main/kubernetes/keycloakrealmimports.k8s.keycloak.org-v1.yml
oc apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/main/deploy/operator.yaml
```

**Bước 2: Tạo file Keycloak CR (Custom Resource)**

```yaml
apiVersion: k8s.keycloak.org/v1
kind: Keycloak
metadata:
  name: example-keycloak
  namespace: keycloak
spec:
  instances: 2
  db:
    vendor: postgres
    host: postgres
    database: keycloak
    schema: public
    usernameSecret:
      name: keycloak-db-secret
      key: username
    passwordSecret:
      name: keycloak-db-secret
      key: password
  http:
    tlsSecret: keycloak-tls-secret
  hostname:
    hostname: keycloak.apps.example.com
  ingress:
    enabled: true
```

**Bước 3: Triển khai Keycloak**

```bash
oc apply -f keycloak-cr.yaml
```

#### Ưu điểm:

* Khả năng tự động mở rộng theo nhu cầu
* Tính sẵn sàng cao (High Availability)
* Tự động khôi phục khi có lỗi (self-healing)
* Quản lý cấu hình dễ dàng qua ConfigMaps/Secrets
* Tích hợp sâu với hệ thống giám sát và logging
* Phân phối tải tự động

#### Nhược điểm:

* Độ phức tạp cao hơn
* Đòi hỏi kiến thức về Kubernetes
* Chi phí cơ sở hạ tầng cao hơn
* Cần thêm thời gian để thiết lập ban đầu

### 4. Quarkus-based Keycloak (Phiên bản mới)

Từ phiên bản 17 trở đi, Keycloak đã chuyển từ WildFly sang kiến trúc dựa trên Quarkus, mang lại hiệu suất và thời gian khởi động tốt hơn.

#### Đặc điểm chính:

* Thời gian khởi động nhanh hơn đáng kể
* Tiêu thụ bộ nhớ thấp hơn
* Hiệu suất cải thiện
* CLI mới với nhiều tùy chọn hơn
* Yêu cầu JDK 11+ (khuyến nghị JDK 17)

#### Cài đặt Quarkus-based Keycloak:

**Bước 1: Tải và giải nén**

```bash
wget https://github.com/keycloak/keycloak/releases/download/21.1.1/keycloak-21.1.1.zip
unzip keycloak-21.1.1.zip
cd keycloak-21.1.1
```

**Bước 2: Build optimized distribution**

```bash
./bin/kc.sh build
```

**Bước 3: Khởi động ở chế độ production**

```bash
./bin/kc.sh start \
  --hostname=keycloak.example.com \
  --https-certificate-file=/path/to/cert.pem \
  --https-certificate-key-file=/path/to/key.pem
```

#### Những điểm khác biệt chính với phiên bản cũ:

* Không còn sử dụng file cấu hình standalone.xml
* Cấu hình thông qua CLI hoặc file keycloak.conf
* Quy trình khởi động hai bước (build + start)
* Tên các tham số cấu hình đã thay đổi
* Cách định nghĩa các provider tùy chỉnh

#### Ưu điểm:

* Hiệu suất tốt hơn
* Tiêu thụ tài nguyên ít hơn
* Thời gian khởi động nhanh hơn
* Phù hợp hơn cho môi trường container và serverless

#### Nhược điểm:

* Không tương thích ngược hoàn toàn với cấu hình cũ
* Cần học cách cấu hình mới
* Một số tính năng cũ có thể chưa được hỗ trợ đầy đủ

### 5. Cấu hình cơ sở dữ liệu

Keycloak hỗ trợ nhiều loại cơ sở dữ liệu khác nhau. Sau đây là hướng dẫn cài đặt cho các CSDL phổ biến:

#### H2 (mặc định - chỉ nên dùng cho development)

```bash
# Chế độ development (H2 in-memory)
./bin/kc.sh start-dev

# Chế độ development (H2 file-based)
./bin/kc.sh start-dev --db-url=jdbc:h2:file:~/keycloak-db/keycloak;AUTO_SERVER=TRUE
```

#### PostgreSQL (được khuyến nghị cho production)

**Bước 1: Cài đặt PostgreSQL**

```bash
# Trên Ubuntu/Debian
sudo apt update
sudo apt install postgresql postgresql-contrib

# Tạo database và user
sudo -u postgres psql -c "CREATE DATABASE keycloak;"
sudo -u postgres psql -c "CREATE USER keycloak WITH ENCRYPTED PASSWORD 'password';"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE keycloak TO keycloak;"
```

**Bước 2: Cấu hình Keycloak với PostgreSQL**

```bash
# Build với vendor PostgreSQL
./bin/kc.sh build --db=postgres

# Khởi động với PostgreSQL
./bin/kc.sh start \
  --db=postgres \
  --db-url=jdbc:postgresql://localhost:5432/keycloak \
  --db-username=keycloak \
  --db-password=password \
  --db-pool-min-size=5 \
  --db-pool-max-size=15
```

#### MySQL

**Bước 1: Cài đặt MySQL**

```bash
# Trên Ubuntu/Debian
sudo apt update
sudo apt install mysql-server

# Bảo mật MySQL
sudo mysql_secure_installation

# Tạo database và user
sudo mysql -e "CREATE DATABASE keycloak CHARACTER SET utf8 COLLATE utf8_unicode_ci;"
sudo mysql -e "CREATE USER 'keycloak'@'localhost' IDENTIFIED BY 'password';"
sudo mysql -e "GRANT ALL PRIVILEGES ON keycloak.* TO 'keycloak'@'localhost';"
sudo mysql -e "FLUSH PRIVILEGES;"
```

**Bước 2: Cấu hình Keycloak với MySQL**

```bash
# Build với vendor MySQL
./bin/kc.sh build --db=mysql

# Khởi động với MySQL
./bin/kc.sh start \
  --db=mysql \
  --db-url="jdbc:mysql://localhost:3306/keycloak?characterEncoding=UTF-8" \
  --db-username=keycloak \
  --db-password=password \
  --db-pool-min-size=5 \
  --db-pool-max-size=15
```

#### Microsoft SQL Server

**Bước 1: Cấu hình Microsoft SQL Server**

```bash
# Tạo database và user (sử dụng SQL Server Management Studio hoặc T-SQL)
# CREATE DATABASE keycloak;
# CREATE LOGIN keycloak WITH PASSWORD = 'password';
# USE keycloak;
# CREATE USER keycloak FOR LOGIN keycloak;
# EXEC sp_addrolemember 'db_owner', 'keycloak';
```

**Bước 2: Cấu hình Keycloak với MSSQL**

```bash
# Build với vendor MSSQL
./bin/kc.sh build --db=mssql

# Khởi động với MSSQL
./bin/kc.sh start \
  --db=mssql \
  --db-url="jdbc:sqlserver://localhost:1433;databaseName=keycloak;encrypt=true;trustServerCertificate=true" \
  --db-username=keycloak \
  --db-password=password \
  --db-pool-min-size=5 \
  --db-pool-max-size=15
```

#### Lưu ý quan trọng về cơ sở dữ liệu:

1. **Sao lưu thường xuyên**: Hãy đảm bảo có kế hoạch sao lưu định kỳ
2. **Connection Pool**: Tùy chỉnh pool size theo tải của hệ thống
3. **Bảo mật**: Sử dụng SSL/TLS cho kết nối database trong môi trường production
4. **Monitoring**: Giám sát hiệu năng database để phát hiện vấn đề sớm
5. **Clustering**: Cân nhắc triển khai database cluster cho môi trường HA

### So sánh các phương pháp cài đặt

| Tiêu chí         | Standalone          | Docker         | Kubernetes               | Quarkus-based      |
| ---------------- | ------------------- | -------------- | ------------------------ | ------------------ |
| Độ phức tạp      | Thấp                | Trung bình     | Cao                      | Trung bình         |
| Hiệu suất        | Cao                 | Khá            | Cao                      | Rất cao            |
| Khả năng mở rộng | Thấp                | Trung bình     | Rất cao                  | Phụ thuộc nền tảng |
| Tự động hóa      | Thấp                | Trung bình     | Cao                      | Trung bình         |
| Phù hợp cho      | Dev, triển khai nhỏ | Triển khai vừa | Production, Doanh nghiệp | Mọi môi trường     |
| Bảo trì          | Thủ công            | Dễ dàng        | Tự động                  | Dễ dàng            |

### Các kịch bản triển khai theo quy mô

#### 1. Triển khai nhỏ (<500 người dùng)

* **Khuyến nghị**: Standalone hoặc Docker
* **Cấu hình**: 2 CPU, 2GB RAM, PostgreSQL
* **Ví dụ**: Startup, dự án nội bộ nhỏ

#### 2. Triển khai vừa (500-5000 người dùng)

* **Khuyến nghị**: Docker Swarm hoặc Kubernetes nhỏ
* **Cấu hình**: 2-3 instance, mỗi instance 4 CPU, 4GB RAM
* **Ví dụ**: Doanh nghiệp vừa, ứng dụng web có quy mô vừa

#### 3. Triển khai lớn (>5000 người dùng)

* **Khuyến nghị**: Kubernetes/OpenShift
* **Cấu hình**: 3+ instance, mỗi instance 8+ CPU, 8+ GB RAM, DB cluster
* **Ví dụ**: Doanh nghiệp lớn, ứng dụng public quy mô lớn

#### 4. Triển khai High Availability

* **Khuyến nghị**: Kubernetes/OpenShift với multi-zone
* **Cấu hình**: 6+ instance trên 3 zone, DB cluster với standby
* **Ví dụ**: Hệ thống ngân hàng, bảo hiểm, y tế

### Các vấn đề cần lưu ý

#### Bảo mật:

* Luôn cấu hình HTTPS
* Thay đổi mật khẩu admin mặc định
* Cấu hình Brute Force Detection
* Thiết lập chính sách mật khẩu mạnh
* Hạn chế truy cập console quản trị
* Cập nhật thường xuyên

#### Hiệu năng:

* Tùy chỉnh JVM heap size
* Cấu hình connection pool phù hợp
* Sử dụng caching (Infinispan)
* Giám sát tài nguyên hệ thống
* Tối ưu hóa database queries

#### Backup và khôi phục:

* Sao lưu database định kỳ
* Sao lưu cấu hình và certificates
* Kiểm tra quy trình khôi phục
* Lưu trữ sao lưu an toàn

### Kết luận

Keycloak là một giải pháp mạnh mẽ và linh hoạt cho quản lý xác thực và ủy quyền. Với nhiều phương pháp cài đặt khác nhau, bạn có thể chọn cách tiếp cận phù hợp nhất với yêu cầu và nguồn lực của mình.

Hãy cân nhắc kỹ các yếu tố sau khi lựa chọn phương pháp triển khai:

* Quy mô người dùng và tải hệ thống
* Yêu cầu về tính sẵn sàng cao
* Kiến thức và kỹ năng của đội ngũ vận hành
* Ngân sách và tài nguyên sẵn có
* Kế hoạch phát triển trong tương lai

Mỗi phương pháp đều có ưu và nhược điểm riêng, nhưng với cách tiếp cận đúng đắn, Keycloak sẽ là nền tảng bảo mật vững chắc cho ứng dụng của bạn.
