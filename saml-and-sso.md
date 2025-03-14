---
icon: universal-access
---

# SAML & SSO

### SAML: Khái niệm, workflow và cấu trúc message

#### Khái niệm SAML

SAML (Security Assertion Markup Language) là một chuẩn mở dựa trên XML được sử dụng để trao đổi dữ liệu xác thực và ủy quyền giữa các bên, đặc biệt là giữa nhà cung cấp danh tính (Identity Provider - IdP) và nhà cung cấp dịch vụ (Service Provider - SP). SAML được phát triển bởi tổ chức OASIS (Organization for the Advancement of Structured Information Standards) và phiên bản được sử dụng phổ biến nhất hiện nay là SAML 2.0, ra mắt vào năm 2005.

SAML giải quyết nhiều vấn đề trong quản lý danh tính, nhưng mục tiêu chính là:

* Cho phép người dùng đăng nhập một lần và truy cập nhiều ứng dụng (Single Sign-On)
* Phân tách việc quản lý danh tính ra khỏi các ứng dụng riêng lẻ
* Cung cấp cơ chế trao đổi thông tin xác thực một cách an toàn giữa các miền khác nhau

#### Workflow của SAML

Luồng SAML cơ bản (Web Browser SSO Profile) bao gồm các bước sau:

1. **Người dùng yêu cầu truy cập tài nguyên**: Người dùng cố gắng truy cập vào một ứng dụng hoặc tài nguyên được bảo vệ tại Service Provider.
2. **SP tạo yêu cầu SAML**: Service Provider nhận thấy người dùng chưa có phiên làm việc, nên tạo một SAML Authentication Request và chuyển hướng trình duyệt người dùng đến Identity Provider.
3. **IdP xác thực người dùng**: Identity Provider kiểm tra xem người dùng đã có phiên làm việc chưa. Nếu chưa, IdP sẽ yêu cầu người dùng cung cấp thông tin đăng nhập.
4. **IdP tạo SAML Assertion**: Sau khi xác thực thành công, IdP tạo một SAML Assertion chứa thông tin xác thực của người dùng, ký số và (tùy chọn) mã hóa nó.
5. **IdP chuyển hướng người dùng trở lại SP**: IdP gửi SAML Response chứa SAML Assertion về cho Service Provider thông qua trình duyệt của người dùng (thường qua HTTP POST).
6. **SP xác thực SAML Assertion**: Service Provider kiểm tra chữ ký của SAML Assertion để đảm bảo nó đến từ IdP đáng tin cậy, và xác minh các điều kiện khác như thời gian hiệu lực.
7. **SP cấp quyền truy cập**: Nếu xác thực thành công, Service Provider tạo phiên làm việc cho người dùng và cho phép truy cập vào tài nguyên được yêu cầu.

#### Cấu trúc message SAML

SAML sử dụng XML làm định dạng để trao đổi thông tin. Có ba loại thành phần chính trong SAML:

**1. SAML Assertions**

Assertions là các tuyên bố chứa thông tin về một đối tượng (thường là người dùng). Có ba loại assertions:

* **Authentication Assertions**: Xác nhận một chủ thể đã được xác thực bằng phương thức cụ thể tại một thời điểm nhất định
* **Attribute Assertions**: Chứa các thuộc tính cụ thể về chủ thể
* **Authorization Decision Assertions**: Xác định liệu một chủ thể có được phép truy cập vào tài nguyên hay không

Ví dụ về SAML Assertion:

```xml
<saml:Assertion
    xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
    ID="_3c39bc0fe7ab" Version="2.0"
    IssueInstant="2023-04-01T12:00:00Z">
    <saml:Issuer>https://idp.example.org</saml:Issuer>
    <saml:Subject>
        <saml:NameID>john.doe@example.com</saml:NameID>
        <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                NotOnOrAfter="2023-04-01T12:10:00Z"
                Recipient="https://sp.example.com/acs" />
        </saml:SubjectConfirmation>
    </saml:Subject>
    <saml:Conditions
        NotBefore="2023-04-01T11:55:00Z"
        NotOnOrAfter="2023-04-01T12:10:00Z">
        <saml:AudienceRestriction>
            <saml:Audience>https://sp.example.com</saml:Audience>
        </saml:AudienceRestriction>
    </saml:Conditions>
    <saml:AuthnStatement
        AuthnInstant="2023-04-01T11:58:00Z"
        SessionIndex="_be9967abd904e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
    <saml:AttributeStatement>
        <saml:Attribute Name="FirstName">
            <saml:AttributeValue>John</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute Name="LastName">
            <saml:AttributeValue>Doe</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute Name="Email">
            <saml:AttributeValue>john.doe@example.com</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute Name="Role">
            <saml:AttributeValue>Admin</saml:AttributeValue>
            <saml:AttributeValue>User</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

**2. SAML Protocols**

SAML Protocols định nghĩa cách thức tạo và xử lý các SAML requests và responses. Các protocols phổ biến bao gồm:

* **Authentication Request Protocol**: Được sử dụng để yêu cầu assertions xác thực
* **Single Logout Protocol**: Cho phép đăng xuất đồng thời khỏi tất cả các ứng dụng
* **Assertion Query and Request Protocol**: Cho phép truy vấn assertions từ IdP

Ví dụ về SAML Authentication Request:

```xml
<samlp:AuthnRequest
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
    xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
    ID="_bec424fa5103" Version="2.0"
    IssueInstant="2023-04-01T11:57:00Z"
    Destination="https://idp.example.org/SAML2/SSO/Browser"
    AssertionConsumerServiceURL="https://sp.example.com/acs"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST">
    <saml:Issuer>https://sp.example.com</saml:Issuer>
    <samlp:NameIDPolicy
        Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress"
        AllowCreate="true" />
</samlp:AuthnRequest>
```

**3. SAML Bindings**

SAML Bindings xác định cách các thông điệp SAML được ánh xạ lên các giao thức truyền thông chuẩn. Các bindings phổ biến bao gồm:

* **HTTP Redirect Binding**: Sử dụng HTTP redirect để truyền thông điệp
* **HTTP POST Binding**: Thông điệp SAML được nhúng vào HTML form và gửi đi bằng HTTP POST
* **HTTP Artifact Binding**: Chỉ tham chiếu đến thông điệp (artifact) được truyền qua HTTP, thông điệp thực sự được lấy qua kênh back-channel
* **SOAP Binding**: Sử dụng SOAP để truyền thông điệp SAML

### So sánh SAML với OpenID Connect

Cả SAML và OpenID Connect (OIDC) đều là các giao thức xác thực và quản lý danh tính được sử dụng rộng rãi, nhưng chúng có những khác biệt đáng kể:

#### 1. Độ tuổi và độ phổ biến

* **SAML**: Ra mắt năm 2005, được sử dụng chủ yếu trong các doanh nghiệp lớn và môi trường enterprise
* **OIDC**: Ra mắt năm 2014, được sử dụng phổ biến trong các ứng dụng web và mobile hiện đại

#### 2. Định dạng và độ phức tạp

* **SAML**: Sử dụng XML, phức tạp hơn, cần nhiều xử lý hơn
* **OIDC**: Sử dụng JSON, nhẹ hơn, dễ sử dụng với JavaScript và API hiện đại

#### 3. Các đặc điểm kỹ thuật

| Đặc điểm           | SAML                           | OpenID Connect          |
| ------------------ | ------------------------------ | ----------------------- |
| Định dạng          | XML                            | JSON                    |
| Token cơ bản       | SAML Assertion                 | ID Token (JWT)          |
| Giao thức nền tảng | Độc lập                        | Xây dựng trên OAuth 2.0 |
| Binding/Transport  | HTTP Redirect, HTTP POST, SOAP | HTTP/REST               |
| Mã hóa/Ký số       | XML Signature, XML Encryption  | JWS/JWE                 |
| Trạng thái phiên   | Thường là stateful             | Stateless với JWT       |
| Discovery          | Metadata XML                   | JSON Discovery document |

#### 4. Trường hợp sử dụng

* **SAML**: Phù hợp với các ứng dụng web truyền thống trong doanh nghiệp, đặc biệt là khi đã có hạ tầng SAML
  * Ưu điểm: Hỗ trợ phiên làm việc phức tạp, tính năng logout đồng bộ mạnh mẽ
* **OIDC**: Phù hợp với ứng dụng web hiện đại, ứng dụng di động, và API
  * Ưu điểm: Đơn giản, hiệu quả về mặt băng thông, tích hợp tốt với JavaScript, dễ triển khai

#### 5. Bảng so sánh chi tiết

| Tính năng          | SAML                        | OpenID Connect                              |
| ------------------ | --------------------------- | ------------------------------------------- |
| Single Sign-On     | Có                          | Có                                          |
| Single Logout      | Có, bền vững                | Hạn chế hơn                                 |
| Mô hình bảo mật    | XML-DSIG, XML-ENC           | JWT, JWS, JWE                               |
| API access         | Không được thiết kế cho API | Được thiết kế cho API (qua OAuth 2.0)       |
| Hỗ trợ mobile      | Hạn chế                     | Tốt                                         |
| Phát hiện dịch vụ  | Metadata XML                | OpenID Discovery                            |
| Thiết lập động     | Giới hạn                    | Hỗ trợ đăng ký client động                  |
| Session management | Có                          | Có, qua "OpenID Connect Session Management" |

### Nguyên tắc Single Sign-On và hoạt động

Single Sign-On (SSO) là một cơ chế xác thực cho phép người dùng đăng nhập một lần và truy cập nhiều ứng dụng hoặc dịch vụ khác nhau mà không cần đăng nhập lại. SSO giải quyết vấn đề "mệt mỏi với mật khẩu" (password fatigue) và tăng cường bảo mật bằng cách tập trung việc quản lý danh tính.

#### Nguyên tắc cơ bản của SSO

1. **Tập trung hóa xác thực**: Một hệ thống trung tâm (IdP) chịu trách nhiệm xác thực người dùng
2. **Tin cậy giữa các bên**: Các ứng dụng tin tưởng vào kết quả xác thực từ IdP
3. **Trao đổi token**: Thay vì gửi thông tin đăng nhập đến từng ứng dụng, người dùng nhận token xác thực từ IdP
4. **Chủ quyền danh tính**: Người dùng kiểm soát quá trình xác thực và biết thông tin nào được chia sẻ

#### Cách thức hoạt động của SSO

Quy trình SSO cơ bản hoạt động như sau:

1. **Truy cập ứng dụng đầu tiên**:
   * Người dùng truy cập một ứng dụng (SP)
   * SP kiểm tra và phát hiện người dùng chưa có phiên làm việc
   * SP chuyển hướng người dùng đến IdP với yêu cầu xác thực
2. **Xác thực tại IdP**:
   * IdP kiểm tra xem người dùng đã có phiên làm việc hay chưa
   * Nếu chưa, IdP yêu cầu người dùng đăng nhập
   * Sau khi xác thực thành công, IdP thiết lập phiên làm việc và tạo token xác thực
3. **Quay lại ứng dụng đầu tiên**:
   * IdP chuyển hướng người dùng trở lại SP với token xác thực
   * SP xác minh token và tạo phiên làm việc cục bộ cho người dùng
   * Người dùng có thể truy cập ứng dụng
4. **Truy cập ứng dụng khác**:
   * Người dùng truy cập ứng dụng thứ hai (SP2)
   * SP2 chuyển hướng người dùng đến IdP
   * IdP nhận ra người dùng đã có phiên làm việc (cookie)
   * IdP tạo token mới cho SP2 mà không yêu cầu đăng nhập lại
   * SP2 xác minh token và tạo phiên làm việc cục bộ

#### Các khái niệm phiên làm việc trong SSO

SSO liên quan đến nhiều loại phiên làm việc:

1. **Global Session (IdP Session)**: Phiên làm việc tại Identity Provider, thường được lưu trong cookie
2. **Local Sessions (SP Sessions)**: Phiên làm việc riêng tại từng Service Provider
3. **Session Timeout**: Thời gian hiệu lực của các phiên làm việc

#### Single Logout (SLO)

Single Logout là phần quan trọng của hệ thống SSO, cho phép người dùng đăng xuất khỏi tất cả các ứng dụng và IdP chỉ với một hành động. SLO có thể được khởi tạo bởi:

* **IdP-initiated SLO**: Đăng xuất bắt đầu từ IdP và lan truyền đến tất cả các SP
* **SP-initiated SLO**: Đăng xuất bắt đầu từ một SP và lan truyền thông qua IdP đến các SP khác

Quy trình SLO điển hình:

1. Người dùng bắt đầu đăng xuất từ một SP hoặc IdP
2. Nếu bắt đầu từ SP, hệ thống gửi yêu cầu đăng xuất đến IdP
3. IdP vô hiệu hóa phiên IdP của người dùng
4. IdP gửi thông báo đăng xuất đến tất cả các SP đã tham gia phiên làm việc
5. Mỗi SP vô hiệu hóa phiên cục bộ của mình
6. Người dùng được chuyển hướng đến trang đích sau khi đăng xuất

### Các mô hình triển khai SSO phổ biến

Có nhiều cách để triển khai SSO trong môi trường doanh nghiệp hoặc hệ thống phân tán. Dưới đây là các mô hình phổ biến:

#### 1. Hub and Spoke (Centralized IdP)

Đây là mô hình SSO phổ biến nhất, trong đó một Identity Provider trung tâm phục vụ nhiều Service Providers.

**Đặc điểm**:

* Một IdP trung tâm quản lý tất cả danh tính người dùng
* Tất cả SP đều tin tưởng IdP trung tâm
* Người dùng chỉ cần tương tác với một điểm xác thực

**Ưu điểm**:

* Quản lý tập trung và nhất quán
* Triển khai và cấu hình đơn giản
* Kiểm soát bảo mật tốt

**Nhược điểm**:

* Single point of failure
* Có thể gặp vấn đề về hiệu suất khi mở rộng

**Trường hợp sử dụng**: Doanh nghiệp vừa và nhỏ, hoặc các hệ thống có một domain quản trị duy nhất.

#### 2. Identity Federation (Federated Identity)

Mô hình này cho phép người dùng từ một miền an ninh (security domain) sử dụng danh tính của họ để truy cập vào các tài nguyên trong miền khác.

**Đặc điểm**:

* Nhiều IdP từ các miền khác nhau hợp tác với nhau
* Các IdP thiết lập quan hệ tin tưởng lẫn nhau
* Người dùng có thể sử dụng danh tính từ tổ chức của họ để truy cập các dịch vụ bên ngoài

**Ưu điểm**:

* Hỗ trợ hợp tác giữa các tổ chức
* Người dùng không cần tài khoản riêng cho mỗi dịch vụ
* Mở rộng quy mô dễ dàng

**Nhược điểm**:

* Phức tạp trong cấu hình và quản lý
* Vấn đề về quyền riêng tư và sự đồng bộ dữ liệu

**Trường hợp sử dụng**: Hợp tác giữa các trường đại học, doanh nghiệp đối tác, hoặc các nhóm tổ chức liên quan.

#### 3. Identity as a Service (IDaaS)

Mô hình này sử dụng dịch vụ danh tính dựa trên đám mây của bên thứ ba như Okta, Azure AD, Auth0, OneLogin, v.v.

**Đặc điểm**:

* IdP được cung cấp và quản lý bởi nhà cung cấp dịch vụ đám mây
* Hỗ trợ nhiều giao thức (SAML, OIDC, WS-Federation)
* Thường có các tính năng bổ sung như MFA, quản lý danh tính, giám sát

**Ưu điểm**:

* Giảm chi phí triển khai và bảo trì
* Mở rộng dễ dàng
* Cập nhật tự động về các tính năng và bản vá bảo mật

**Nhược điểm**:

* Phụ thuộc vào bên thứ ba
* Vấn đề về kiểm soát dữ liệu và quyền riêng tư
* Chi phí có thể tăng khi mở rộng quy mô

**Trường hợp sử dụng**: Doanh nghiệp muốn triển khai nhanh, tổ chức có nhiều ứng dụng SaaS, hoặc các công ty không muốn quản lý hạ tầng IdP.

#### 4. Web Access Management (WAM)

Mô hình này sử dụng một lớp trung gian để bảo vệ các ứng dụng web và quản lý quyền truy cập.

**Đặc điểm**:

* Tích hợp với các cổng ứng dụng hoặc proxy ngược
* Tập trung vào việc bảo vệ các ứng dụng web
* Thường sử dụng agent hoặc filter tại tầng web

**Ưu điểm**:

* Bảo vệ các ứng dụng cũ không hỗ trợ các giao thức SSO hiện đại
* Kiểm soát truy cập chi tiết
* Tích hợp với các hệ thống quản lý danh tính hiện có

**Nhược điểm**:

* Có thể tạo ra "nút thắt cổ chai" về hiệu suất
* Triển khai phức tạp hơn

**Trường hợp sử dụng**: Bảo vệ các ứng dụng di sản (legacy), môi trường với yêu cầu bảo mật cao.

#### 5. Social Login / External IdP

Sử dụng các nhà cung cấp danh tính bên ngoài như Google, Facebook, Apple, Microsoft để xác thực người dùng.

**Đặc điểm**:

* Dựa vào các nhà cung cấp danh tính phổ biến
* Thường sử dụng OAuth 2.0 và OpenID Connect
* Cho phép người dùng sử dụng tài khoản hiện có

**Ưu điểm**:

* Trải nghiệm người dùng thuận tiện
* Giảm gánh nặng quản lý mật khẩu
* Triển khai nhanh chóng

**Nhược điểm**:

* Phụ thuộc vào bên thứ ba
* Mức độ kiểm soát thông tin người dùng hạn chế
* Vấn đề về quyền riêng tư và tin tưởng

**Trường hợp sử dụng**: Các ứng dụng tiêu dùng, các startup, trang web và ứng dụng tập trung vào người dùng cuối.

