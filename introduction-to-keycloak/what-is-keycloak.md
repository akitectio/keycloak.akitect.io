---
icon: question
---

# What is Keycloak?

Keycloak là một giải pháp quản lý nhận dạng và truy cập (Identity and Access Management - IAM) mã nguồn mở, cung cấp khả năng xác thực người dùng (authentication) và ủy quyền (authorization) cho các ứng dụng web hiện đại và API. Được tạo ra để giải quyết các thách thức trong việc bảo mật ứng dụng, Keycloak cung cấp các tính năng SSO (Single Sign-On), quản lý người dùng, tích hợp với các nhà cung cấp danh tính bên thứ ba, và nhiều công cụ bảo mật khác.

### Các tính năng chính của Keycloak

1. **Single Sign-On (SSO)**: Cho phép người dùng đăng nhập một lần và truy cập nhiều ứng dụng khác nhau mà không cần đăng nhập lại.
2. **Hỗ trợ các giao thức chuẩn**: Triển khai đầy đủ các giao thức bảo mật như:
   * OpenID Connect
   * OAuth 2.0
   * SAML 2.0
   * LDAP
   * Kerberos
3. **Quản lý người dùng**: Giao diện quản trị toàn diện cho việc quản lý người dùng, nhóm, vai trò và quyền.
4. **Social Login**: Tích hợp dễ dàng với các nhà cung cấp danh tính xã hội như Google, Facebook, Twitter, GitHub.
5. **Xác thực đa yếu tố (MFA)**: Hỗ trợ nhiều hình thức xác thực đa yếu tố bao gồm OTP, TOTP, và SMS.
6. **Identity Brokering**: Cho phép sử dụng danh tính từ các nhà cung cấp danh tính bên ngoài.
7. **Client Adapters**: Cung cấp thư viện tích hợp cho nhiều ngôn ngữ và frameworks như Java, JavaScript, Node.js, Python, .NET.
8. **Tùy biến giao diện**: Khả năng tùy biến giao diện đăng nhập, đăng ký và các màn hình khác.
9. **Quản lý phiên**: Giám sát và quản lý các phiên người dùng với khả năng đăng xuất từ xa.
10. **Truy vấn và ủy quyền dựa trên thuộc tính**: Hệ thống phân quyền dựa trên vai trò (RBAC) mạnh mẽ và linh hoạt.

### Lịch sử phát triển từ JBoss đến Red Hat SSO

Keycloak có một hành trình phát triển thú vị, từ một dự án mã nguồn mở đến một giải pháp bảo mật cấp doanh nghiệp:

#### Giai đoạn ban đầu (2014-2015)

* **Khởi đầu**: Keycloak được phát triển bởi một nhóm kỹ sư tại JBoss, một phần của Red Hat, vào năm 2014.
* **Tầm nhìn**: Dự án được tạo ra với mục tiêu cung cấp giải pháp IAM mã nguồn mở mạnh mẽ, dễ triển khai và tùy biến.
* **Phiên bản đầu tiên**: Keycloak 1.0 được phát hành vào tháng 9 năm 2014, cung cấp các tính năng SSO cơ bản và hỗ trợ OpenID Connect.

#### Phát triển dưới JBoss (2015-2017)

* **Tăng trưởng nhanh chóng**: Keycloak nhanh chóng mở rộng tính năng và cộng đồng người dùng.
* **Tích hợp với JBoss**: Trở thành một phần quan trọng trong hệ sinh thái JBoss middleware.
* **Phiên bản 2.0 và 3.0**: Giới thiệu các tính năng mới như SAML 2.0, quản lý người dùng nâng cao, và xác thực đa yếu tố.

#### Chuyển đổi sang Red Hat SSO (2016-hiện tại)

* **Red Hat Single Sign-On (RH-SSO)**: Vào năm 2016, Red Hat ra mắt Red Hat Single Sign-On, phiên bản doanh nghiệp của Keycloak.
* **Hai hướng phát triển song song**:
  * **Keycloak**: Tiếp tục là dự án mã nguồn mở, phát triển nhanh với các tính năng mới.
  * **RH-SSO**: Phiên bản doanh nghiệp, tập trung vào tính ổn định, hỗ trợ dài hạn và tích hợp với các sản phẩm Red Hat khác.
* **Quan hệ giữa hai phiên bản**: RH-SSO dựa trên Keycloak nhưng thường chậm hơn một vài phiên bản để đảm bảo ổn định. Ví dụ, RH-SSO 7.4 dựa trên Keycloak 9.0.x.

#### Các cột mốc quan trọng

* **2014**: Phát hành Keycloak 1.0
* **2016**: Ra mắt Red Hat Single Sign-On (RH-SSO) 7.0, dựa trên Keycloak 1.9.8
* **2018**: Keycloak 4.0 với cải tiến lớn về hiệu suất và khả năng mở rộng
* **2019**: Keycloak đạt 7.0 với hỗ trợ tốt hơn cho kiến trúc microservices
* **2020**: Keycloak 11.0 với giao diện quản trị mới
* **2022**: Keycloak 18.0+ chuyển sang Quarkus làm nền tảng cơ bản, cải thiện đáng kể hiệu suất và giảm footprint
* **2023-2024**: Tập trung vào cải thiện trải nghiệm nhà phát triển, hiệu suất và khả năng mở rộng

#### Sự phát triển về công nghệ

* **Từ Wildfly đến Quarkus**: Keycloak ban đầu dựa trên Wildfly (JBoss AS), nhưng đã chuyển sang Quarkus, một framework Java hiện đại hơn tối ưu hóa cho Kubernetes và môi trường đám mây.
* **Kiến trúc Microservices**: Phát triển từ một ứng dụng monolithic sang kiến trúc microservices linh hoạt hơn.
* **Hỗ trợ Kubernetes/OpenShift**: Cải thiện đáng kể khả năng vận hành trong môi trường container và Kubernetes.

### Vị trí của Keycloak trong hệ sinh thái IAM

Keycloak đóng một vai trò quan trọng trong hệ sinh thái IAM, nổi bật bởi những đặc điểm sau:

#### 1. Vị trí trong thị trường IAM

* **Giải pháp mã nguồn mở hàng đầu**: Keycloak là một trong những giải pháp IAM mã nguồn mở phổ biến nhất, với cộng đồng lớn và sự phát triển tích cực.
* **Cầu nối giữa mã nguồn mở và doanh nghiệp**: Cung cấp cả phiên bản cộng đồng miễn phí và phiên bản doanh nghiệp có hỗ trợ thương mại (qua Red Hat SSO).
* **Phổ biến trong các doanh nghiệp vừa và nhỏ**: Được sử dụng rộng rãi trong các tổ chức không có ngân sách cho các giải pháp thương mại cao cấp.

#### 2. Mối quan hệ với các công nghệ khác

* **Hệ sinh thái Java/Jakarta EE**: Tích hợp đặc biệt tốt với các ứng dụng Java.
* **Kubernetes & Container Orchestration**: Hỗ trợ tốt cho triển khai trong môi trường container.
* **Microservices Architecture**: Cung cấp bảo mật cho kiến trúc microservices với mô hình ủy quyền phân tán.
* **API Management**: Thường được sử dụng cùng với các giải pháp quản lý API như Kong, 3scale, hoặc Apigee.

#### 3. Các trường hợp sử dụng chính

* **Bảo mật ứng dụng nội bộ**: Cung cấp SSO cho các ứng dụng nội bộ doanh nghiệp.
* **Bảo mật API**: Sử dụng OAuth 2.0 để bảo vệ APIs.
* **Customer Identity and Access Management (CIAM)**: Quản lý danh tính của khách hàng cho các ứng dụng hướng người dùng.
* **B2B Identity Federation**: Cho phép đối tác doanh nghiệp truy cập tài nguyên một cách an toàn.
* **Hiện đại hóa hệ thống cũ**: Bổ sung xác thực hiện đại cho các ứng dụng legacy.

#### 4. Thế mạnh trong hệ sinh thái IAM

* **Linh hoạt và có thể mở rộng**: Hỗ trợ nhiều use cases từ đơn giản đến phức tạp.
* **Không phụ thuộc vào nhà cung cấp**: Là giải pháp mã nguồn mở, tránh lock-in với nhà cung cấp cụ thể.
* **Triển khai linh hoạt**: Có thể triển khai trên cơ sở hạ tầng của riêng bạn, đám mây công cộng, hoặc đám mây riêng.
* **Cộng đồng mạnh mẽ**: Hỗ trợ từ cộng đồng lớn và tích cực.

### So sánh Keycloak với các giải pháp như Auth0, Okta, Azure AD

Keycloak là một giải pháp IAM mạnh mẽ, nhưng cách nó so sánh với các dịch vụ thương mại như Auth0, Okta và Azure AD là điều quan trọng cần xem xét khi lựa chọn giải pháp:

#### Mô hình triển khai và sở hữu

| Giải pháp    | Mô hình          | Triển khai                          | Chi phí                                                             |
| ------------ | ---------------- | ----------------------------------- | ------------------------------------------------------------------- |
| **Keycloak** | Mã nguồn mở      | Self-hosted (on-premise hoặc cloud) | Miễn phí (cộng đồng), Trả phí cho Red Hat SSO (hỗ trợ doanh nghiệp) |
| **Auth0**    | SaaS, thương mại | Chủ yếu cloud-hosted                | Trả phí theo người dùng/tính năng, có gói miễn phí hạn chế          |
| **Okta**     | SaaS, thương mại | Chủ yếu cloud-hosted                | Trả phí theo người dùng, có gói miễn phí hạn chế                    |
| **Azure AD** | SaaS, thương mại | Microsoft Cloud                     | Miễn phí cơ bản với Office 365/Azure, premium tính phí              |

#### Tính năng và khả năng

| Tính năng                                 | Keycloak                    | Auth0                     | Okta                         | Azure AD                    |
| ----------------------------------------- | --------------------------- | ------------------------- | ---------------------------- | --------------------------- |
| **SSO (Single Sign-On)**                  | ✅                           | ✅                         | ✅                            | ✅                           |
| **Social Login**                          | ✅ Cần cấu hình              | ✅ Tích hợp sẵn            | ✅ Tích hợp sẵn               | ✅ Tích hợp sẵn              |
| **MFA (Multi-factor Authentication)**     | ✅ Cơ bản                    | ✅ Nâng cao                | ✅ Nâng cao                   | ✅ Nâng cao                  |
| **B2B Federation**                        | ✅                           | ✅ Các gói cao cấp         | ✅ Các gói cao cấp            | ✅                           |
| **Customization**                         | ✅ Cao (mã nguồn mở)         | ⚠️ Giới hạn               | ⚠️ Giới hạn                  | ⚠️ Giới hạn                 |
| **User Management**                       | ✅                           | ✅                         | ✅                            | ✅                           |
| **Identity Governance**                   | ⚠️ Cơ bản                   | ✅ Nâng cao                | ✅ Rất mạnh                   | ✅ Nâng cao                  |
| **API Access Management**                 | ✅                           | ✅                         | ✅                            | ✅                           |
| **Analytics & Reporting**                 | ⚠️ Hạn chế                  | ✅ Nâng cao                | ✅ Rất mạnh                   | ✅ Nâng cao                  |
| **Adaptive Authentication**               | ⚠️ Cơ bản                   | ✅ Nâng cao                | ✅ Nâng cao                   | ✅ Nâng cao                  |
| **Compliance Certifications**             | ⚠️ Phụ thuộc vào triển khai | ✅ SOC2, GDPR, HIPAA, etc. | ✅ SOC2, FedRAMP, HIPAA, etc. | ✅ SOC2, FedRAMP, GDPR, etc. |
| **DevOps Integration**                    | ✅ Nâng cao                  | ✅                         | ✅                            | ✅                           |
| **No-code Workflows**                     | ❌                           | ✅                         | ✅                            | ✅                           |
| **Password Policies**                     | ✅                           | ✅                         | ✅                            | ✅                           |
| **Passwordless Auth**                     | ✅ Cơ bản                    | ✅ Nâng cao                | ✅ Nâng cao                   | ✅ Nâng cao                  |
| **RBAC (Role-Based Access Control)**      | ✅                           | ✅                         | ✅                            | ✅                           |
| **ABAC (Attribute-Based Access Control)** | ✅                           | ✅ Nâng cao                | ✅ Nâng cao                   | ✅                           |
| **JWT Support**                           | ✅                           | ✅                         | ✅                            | ✅                           |
| **User Lifecycle Management**             | ⚠️ Cơ bản                   | ✅                         | ✅ Rất mạnh                   | ✅                           |
| **Brute Force Protection**                | ✅                           | ✅                         | ✅                            | ✅                           |
| **Anomaly Detection**                     | ❌                           | ✅                         | ✅                            | ✅                           |
| **Directory Integration**                 | ✅                           | ✅                         | ✅                            | ✅ Rất mạnh                  |
| **Self-service User Management**          | ✅                           | ✅                         | ✅                            | ✅                           |
| **Risk-based Authentication**             | ❌                           | ✅                         | ✅                            | ✅                           |
| **Brandable UI**                          | ✅                           | ✅                         | ✅                            | ⚠️ Giới hạn                 |

#### Tích hợp và Hỗ trợ

| Khía cạnh                 | Keycloak      | Auth0       | Okta        | Azure AD |
| ------------------------- | ------------- | ----------- | ----------- | -------- |
| **Số lượng tích hợp sẵn** | ⚠️ Trung bình | ✅ Rất nhiều | ✅ Rất nhiều | ✅ Nhiều  |
| **Hỗ trợ frameworks**     | ✅ Nhiều       | ✅ Rất nhiều | ✅ Rất nhiều | ✅ Nhiều  |
| **Tài liệu**              | ✅ Tốt         | ✅ Rất tốt   | ✅ Rất tốt   | ✅ Tốt    |
| **Hỗ trợ cộng đồng**      | ✅ Mạnh        | ✅           | ✅           | ✅        |
| **Hỗ trợ doanh nghiệp**   | ✅ Qua Red Hat | ✅ 24/7      | ✅ 24/7      | ✅ 24/7   |

#### Mở rộng quy mô và Hiệu suất

| Khía cạnh                 | Keycloak                     | Auth0        | Okta         | Azure AD     |
| ------------------------- | ---------------------------- | ------------ | ------------ | ------------ |
| **Khả năng mở rộng**      | ✅ Cần cấu hình và triển khai | ✅ Tự động    | ✅ Tự động    | ✅ Tự động    |
| **Hiệu suất cao**         | ✅ Phụ thuộc vào cấu hình     | ✅            | ✅            | ✅            |
| **Tính sẵn sàng cao**     | ✅ Cần cấu hình riêng         | ✅ SLA 99.9%+ | ✅ SLA 99.9%+ | ✅ SLA 99.9%+ |
| **Cân bằng tải toàn cầu** | ⚠️ Cần triển khai            | ✅            | ✅            | ✅            |

#### Ưu và nhược điểm

**Keycloak**

**Ưu điểm:**

* Hoàn toàn miễn phí (phiên bản cộng đồng)
* Kiểm soát tuyệt đối về triển khai và dữ liệu
* Khả năng tùy biến cao
* Mã nguồn mở với cộng đồng mạnh mẽ
* Phù hợp với môi trường container và Kubernetes
* Không phụ thuộc vào nhà cung cấp dịch vụ

**Nhược điểm:**

* Yêu cầu chuyên môn kỹ thuật để triển khai và bảo trì
* Phải tự quản lý cơ sở hạ tầng và bảo mật
* Ít tích hợp sẵn hơn các giải pháp SaaS
* Thiếu một số tính năng nâng cao có trong các sản phẩm thương mại
* Có thể phức tạp khi cấu hình cho trường hợp đặc biệt

**Auth0**

**Ưu điểm:**

* Dễ dàng triển khai và sử dụng
* Trải nghiệm nhà phát triển xuất sắc
* Tích hợp sẵn với nhiều dịch vụ và nền tảng
* Tài liệu và hướng dẫn chất lượng cao
* Nhiều tính năng cao cấp như phát hiện rủi ro

**Nhược điểm:**

* Chi phí cao khi mở rộng quy mô
* Ít kiểm soát hơn so với giải pháp tự triển khai
* Giới hạn tùy biến cho các gói cơ bản
* Phụ thuộc vào nhà cung cấp dịch vụ

**Okta**

**Ưu điểm:**

* Giải pháp IAM toàn diện cho doanh nghiệp
* Nhiều tích hợp sẵn với các ứng dụng doanh nghiệp
* Tính năng quản trị và báo cáo mạnh mẽ
* Quy trình workflow có thể tùy chỉnh
* Được chứng nhận tuân thủ nhiều tiêu chuẩn

**Nhược điểm:**

* Đắt đối với các tổ chức nhỏ
* Giao diện có thể khó sử dụng
* Triển khai phức tạp cho các tùy chỉnh nâng cao
* Phụ thuộc vào nhà cung cấp dịch vụ

**Azure AD**

**Ưu điểm:**

* Tích hợp chặt chẽ với các sản phẩm Microsoft
* Tính năng cơ bản miễn phí với Office 365
* Khả năng mở rộng toàn cầu
* Mạnh mẽ trong môi trường doanh nghiệp sử dụng Microsoft
* Nhiều chứng nhận tuân thủ

**Nhược điểm:**

* Gắn liền với hệ sinh thái Microsoft
* Giao diện có thể phức tạp
* Một số tính năng cao cấp yêu cầu gói trả phí
* Ít linh hoạt cho môi trường không phải Microsoft

#### Phân tích so sánh theo kịch bản sử dụng

**1. Doanh nghiệp vừa và nhỏ (SMB)**

| Giải pháp    | Phù hợp                  | Lý do                                                                 |
| ------------ | ------------------------ | --------------------------------------------------------------------- |
| **Keycloak** | ✅ Cao                    | Chi phí thấp, đủ tính năng cho nhu cầu cơ bản, nhưng cần nguồn lực IT |
| **Auth0**    | ✅ Trung bình             | Dễ sử dụng, nhưng chi phí tăng nhanh khi mở rộng                      |
| **Okta**     | ⚠️ Thấp                  | Tính năng phong phú nhưng chi phí cao, phức tạp                       |
| **Azure AD** | ✅ Cao nếu dùng Microsoft | Tích hợp tốt với Office 365, chi phí hợp lý                           |

**2. Doanh nghiệp lớn (Enterprise)**

| Giải pháp           | Phù hợp      | Lý do                                                      |
| ------------------- | ------------ | ---------------------------------------------------------- |
| **Keycloak/RH-SSO** | ✅ Trung bình | Kiểm soát cao, nhưng đòi hỏi đầu tư vào vận hành           |
| **Auth0**           | ✅ Cao        | Hỗ trợ tốt, nhiều tính năng doanh nghiệp                   |
| **Okta**            | ✅ Rất cao    | Thiết kế cho doanh nghiệp, quản trị mạnh mẽ                |
| **Azure AD**        | ✅ Rất cao    | Tích hợp tốt với Microsoft, quen thuộc với IT doanh nghiệp |

**3. Startup/Công ty phát triển phần mềm**

| Giải pháp    | Phù hợp       | Lý do                                         |
| ------------ | ------------- | --------------------------------------------- |
| **Keycloak** | ✅ Cao         | Miễn phí, linh hoạt, kiểm soát cao            |
| **Auth0**    | ✅ Cao         | Trải nghiệm nhà phát triển tốt, dễ triển khai |
| **Okta**     | ⚠️ Thấp       | Chi phí cao, phức tạp cho nhu cầu đơn giản    |
| **Azure AD** | ⚠️ Trung bình | Tốt nếu đã trong hệ sinh thái Microsoft       |

**4. Ứng dụng với yêu cầu tuân thủ/quy định cao**

| Giải pháp    | Phù hợp       | Lý do                                                        |
| ------------ | ------------- | ------------------------------------------------------------ |
| **Keycloak** | ⚠️ Trung bình | Cần đầu tư vào việc triển khai tuân thủ                      |
| **Auth0**    | ✅ Cao         | Nhiều chứng chỉ tuân thủ, quản lý rủi ro                     |
| **Okta**     | ✅ Rất cao     | Chứng chỉ toàn diện, quản trị mạnh mẽ                        |
| **Azure AD** | ✅ Rất cao     | Chứng chỉ toàn diện, tích hợp với công cụ tuân thủ Microsoft |

#### Khi nào nên chọn Keycloak?

Keycloak là lựa chọn phù hợp trong các trường hợp sau:

1. **Hạn chế về ngân sách**: Khi chi phí là yếu tố quan trọng và bạn cần một giải pháp IAM đầy đủ tính năng mà không phải trả phí theo người dùng.
2. **Yêu cầu kiểm soát dữ liệu**: Khi bạn cần lưu trữ dữ liệu người dùng nhạy cảm trong hạ tầng của riêng mình, đặc biệt là do yêu cầu về quy định hoặc tuân thủ.
3. **Môi trường Kubernetes/Container**: Keycloak hoạt động rất tốt trong môi trường container hóa, đặc biệt là với phiên bản Quarkus mới.
4. **Kỹ năng nội bộ**: Khi tổ chức của bạn có kỹ năng kỹ thuật để quản lý và điều chỉnh một giải pháp mã nguồn mở.
5. **Tùy biến sâu**: Khi bạn cần tùy chỉnh sâu rộng các quy trình xác thực, giao diện, và logic vượt ra ngoài những gì các nền tảng SaaS cho phép.
6. **Môi trường hybrid/offline**: Trong các tình huống cần triển khai trong môi trường không có kết nối internet ổn định hoặc môi trường air-gapped.
7. **Hệ sinh thái Red Hat/JBoss**: Nếu bạn đã sử dụng các sản phẩm Red Hat khác và muốn tích hợp chặt chẽ.
8. **Tránh lock-in**: Khi bạn muốn tránh phụ thuộc vào một nhà cung cấp dịch vụ cụ thể và muốn khả năng di chuyển giữa các môi trường.

#### Khi nào nên chọn các giải pháp SaaS (Auth0, Okta, Azure AD)?

Các giải pháp SaaS như Auth0, Okta hoặc Azure AD thường phù hợp hơn khi:

1. **Nguồn lực IT hạn chế**: Bạn không có đội ngũ hoặc thời gian để quản lý hạ tầng và bảo mật IAM.
2. **Triển khai nhanh**: Bạn cần một giải pháp hoạt động ngay lập tức mà không cần cấu hình phức tạp.
3. **Yêu cầu tuân thủ**: Bạn cần một giải pháp đã có sẵn các chứng chỉ tuân thủ (SOC2, HIPAA, FedRAMP, v.v.).
4. **Tích hợp đa dạng**: Bạn cần tích hợp với nhiều ứng dụng SaaS khác mà không phải xây dựng kết nối tùy chỉnh.
5. **Phân tích và báo cáo**: Bạn cần các công cụ báo cáo và phân tích nâng cao.
6. **Quản lý danh tính tiên tiến**: Bạn cần các tính năng như xác thực thích ứng (adaptive), đánh giá rủi ro, hoặc workflows phức tạp.
7. **Hỗ trợ SLA**: Bạn cần đảm bảo thời gian hoạt động với SLA được đảm bảo.
8. **Triển khai toàn cầu**: Bạn cần một giải pháp có hiệu suất tốt trên nhiều khu vực địa lý khác nhau.

### Kết luận

Keycloak là một giải pháp IAM mã nguồn mở mạnh mẽ đã phát triển từ một dự án JBoss thành một sản phẩm cấp doanh nghiệp (qua Red Hat SSO). Nó cung cấp một lựa chọn hấp dẫn với chi phí thấp và khả năng kiểm soát cao, đặc biệt phù hợp cho các tổ chức có nguồn lực kỹ thuật để quản lý việc triển khai.

Tuy nhiên, giải pháp SaaS như Auth0, Okta và Azure AD cung cấp sự thuận tiện, khả năng mở rộng tự động và các tính năng nâng cao mà Keycloak có thể không có sẵn. Quyết định lựa chọn giữa Keycloak và các giải pháp SaaS nên dựa trên các yếu tố như ngân sách, nguồn lực kỹ thuật, yêu cầu kiểm soát dữ liệu, và mức độ tuân thủ cần thiết.

Trong nhiều trường hợp, một chiến lược kết hợp có thể là lựa chọn tốt nhất - sử dụng Keycloak cho một số ứng dụng nội bộ hoặc nhạy cảm, trong khi sử dụng các giải pháp SaaS cho các tình huống tiếp xúc với khách hàng hoặc yêu cầu tích hợp rộng rãi với các dịch vụ đám mây khác.
