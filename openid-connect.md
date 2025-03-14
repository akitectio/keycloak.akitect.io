---
icon: openid
---

# OpenID Connect

### OpenID Connect là gì và sự mở rộng từ OAuth 2.0

OpenID Connect (OIDC) là một lớp nhận dạng được xây dựng trên nền tảng của OAuth 2.0, giúp đơn giản hóa quá trình xác thực người dùng và cung cấp thông tin về danh tính. Trong khi OAuth 2.0 chỉ tập trung vào việc cấp quyền truy cập (authorization), OpenID Connect bổ sung khả năng xác thực (authentication).

#### Sự khác biệt giữa OAuth 2.0 và OpenID Connect:

* **OAuth 2.0**: Tập trung vào việc ủy quyền - cho phép ứng dụng thứ ba truy cập vào tài nguyên được bảo vệ của người dùng mà không cần chia sẻ mật khẩu
* **OpenID Connect**: Bổ sung lớp xác thực - xác minh danh tính người dùng và cung cấp thông tin cơ bản về họ

OIDC giải quyết vấn đề mà nhiều nhà phát triển đã lạm dụng OAuth 2.0 - sử dụng giao thức ủy quyền để thực hiện xác thực. OIDC chuẩn hóa cách thực hiện xác thực một cách an toàn và đáng tin cậy.

### ID Token và các claims

Một trong những thành phần quan trọng nhất của OpenID Connect là **ID Token**. Đây là một JSON Web Token (JWT) được mã hóa, chứa thông tin về việc xác thực người dùng và các thông tin cơ bản về họ.

#### Cấu trúc của ID Token:

1. **Header**: Chứa thông tin về loại token và thuật toán mã hóa
2. **Payload**: Chứa các claims (thông tin) về người dùng
3. **Signature**: Chữ ký số đảm bảo tính toàn vẹn của token

#### Các claims phổ biến trong ID Token:

* `iss` (issuer): URL của nhà cung cấp danh tính đã phát hành token
* `sub` (subject): Định danh duy nhất của người dùng
* `aud` (audience): ID của ứng dụng nhận token
* `exp` (expiration time): Thời điểm token hết hạn
* `iat` (issued at): Thời điểm token được phát hành
* `auth_time`: Thời điểm người dùng được xác thực
* `nonce`: Giá trị ngẫu nhiên để ngăn chặn tấn công tái phát
* `acr` (Authentication Context Class Reference): Mức độ bảo mật của quá trình xác thực
* `amr` (Authentication Methods References): Phương thức xác thực đã sử dụng

Ngoài ra, ID Token có thể chứa các claims tùy chọn về người dùng như `name`, `email`, `picture`, tùy thuộc vào scope được yêu cầu.

#### Ví dụ về ID Token:

```
// Header
{
  "alg": "RS256",
  "typ": "JWT",
  "kid": "1e9gdk7"
}

// Payload
{
  "iss": "https://auth.example.com",
  "sub": "user123456",
  "aud": "client_id_abc",
  "exp": 1616239022,
  "iat": 1616235422,
  "auth_time": 1616235400,
  "nonce": "n-0S6_WzA2Mj",
  "name": "John Doe",
  "email": "john@example.com"
}

// Signature
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

### Các loại flows: Authorization Code Flow, Implicit Flow, Hybrid Flow

OpenID Connect định nghĩa nhiều luồng xác thực khác nhau để phù hợp với các loại ứng dụng và yêu cầu bảo mật khác nhau.

#### 1. Authorization Code Flow

Flow này phù hợp cho các ứng dụng server-side có khả năng bảo vệ client secret.

**Các bước thực hiện:**

1. Ứng dụng client chuyển hướng người dùng đến Authorization Server với các tham số yêu cầu
2. Người dùng xác thực và cấp quyền
3. Authorization Server trả về Authorization Code cho client
4. Client đổi code này lấy ID Token và Access Token (tùy chọn) thông qua kênh back-channel an toàn
5. Client xác thực ID Token và lấy thông tin người dùng

**Ưu điểm:**

* Bảo mật cao vì tokens không bao giờ tiếp xúc với trình duyệt
* Hỗ trợ refresh tokens để duy trì phiên làm việc dài

#### 2. Implicit Flow

Flow này được thiết kế cho các Single Page Applications (SPAs) hoặc ứng dụng JavaScript không có back-end server.

**Các bước thực hiện:**

1. Ứng dụng client chuyển hướng người dùng đến Authorization Server
2. Người dùng xác thực và cấp quyền
3. Authorization Server trả về ID Token (và Access Token nếu yêu cầu) trực tiếp trong URL fragment
4. Client xác thực ID Token và sử dụng thông tin

**Ưu điểm:**

* Đơn giản, không cần back-channel
* Phù hợp cho ứng dụng client-side

**Nhược điểm:**

* Kém bảo mật hơn vì tokens được truyền qua URL fragment
* Không hỗ trợ refresh tokens

**Lưu ý**: Implicit Flow hiện đang được khuyến nghị thay thế bằng Authorization Code Flow với PKCE cho các ứng dụng SPA.

#### 3. Hybrid Flow

Flow này kết hợp các đặc điểm của cả Authorization Code Flow và Implicit Flow.

**Các bước thực hiện:**

1. Ứng dụng client chuyển hướng người dùng đến Authorization Server
2. Người dùng xác thực và cấp quyền
3. Authorization Server trả về Authorization Code cùng với ID Token (và có thể cả Access Token) trong URL fragment
4. Client xác thực ID Token và sử dụng Authorization Code để lấy thêm tokens qua back-channel

**Ưu điểm:**

* Cho phép ứng dụng client truy cập ngay vào ID Token trong khi vẫn duy trì bảo mật của Authorization Code
* Phù hợp cho các ứng dụng native và web applications phức tạp

#### Bảng so sánh các Flow:

| Flow               | Phù hợp với                          | Tokens trả về từ Authorization Endpoint               | Tokens trả về từ Token Endpoint       |
| ------------------ | ------------------------------------ | ----------------------------------------------------- | ------------------------------------- |
| Authorization Code | Web apps với server                  | Authorization Code                                    | ID Token, Access Token, Refresh Token |
| Implicit           | Single Page Apps (không khuyến nghị) | ID Token, Access Token                                | Không có                              |
| Hybrid             | Native apps, phức tạp                | Authorization Code, ID Token, (tùy chọn) Access Token | ID Token, Access Token, Refresh Token |

### Phương thức xác thực và discovery endpoints

#### Discovery Endpoints

OpenID Connect cung cấp cơ chế tự động khám phá các endpoints và tính năng được hỗ trợ thông qua một tài liệu cấu hình đã biết.

**OpenID Connect Discovery (/.well-known/openid-configuration)**:

Một nhà cung cấp danh tính (identity provider) sẽ công bố một tài liệu JSON tại URL `/.well-known/openid-configuration` chứa tất cả thông tin cần thiết để client có thể tương tác với dịch vụ OpenID Connect, bao gồm:

* URL các endpoints: authorization, token, userinfo, revocation, JWKS
* Các scopes và claims được hỗ trợ
* Các thuật toán mã hóa được hỗ trợ
* Các tính năng khác như session management, revocation, dynamic registration

#### Ví dụ về tài liệu Discovery:

```json
{
  "issuer": "https://auth.example.com",
  "authorization_endpoint": "https://auth.example.com/auth",
  "token_endpoint": "https://auth.example.com/token",
  "userinfo_endpoint": "https://auth.example.com/userinfo",
  "jwks_uri": "https://auth.example.com/jwks",
  "registration_endpoint": "https://auth.example.com/register",
  "scopes_supported": ["openid", "profile", "email", "address", "phone"],
  "response_types_supported": ["code", "token", "id_token", "code token", "code id_token", "token id_token", "code token id_token"],
  "grant_types_supported": ["authorization_code", "implicit", "refresh_token"],
  "subject_types_supported": ["public", "pairwise"],
  "id_token_signing_alg_values_supported": ["RS256", "ES256", "HS256"],
  "token_endpoint_auth_methods_supported": ["client_secret_basic", "client_secret_post", "client_secret_jwt", "private_key_jwt"]
}
```

#### Phương thức xác thực client

Khi client giao tiếp với token endpoint, nó cần xác thực mình với Authorization Server. OIDC hỗ trợ nhiều phương thức:

1. **client\_secret\_basic**: Client ID và secret được gửi trong HTTP Basic Authentication header
2. **client\_secret\_post**: Client ID và secret được gửi trong body của HTTP POST request
3. **client\_secret\_jwt**: Client tạo một JWT được ký bằng client secret
4. **private\_key\_jwt**: Client tạo một JWT được ký bằng khóa riêng tư
5. **none**: Không có xác thực client (chỉ cho public clients)

#### UserInfo Endpoint

Ngoài ID Token, OpenID Connect còn cung cấp UserInfo Endpoint, nơi client có thể lấy thêm thông tin về người dùng bằng cách gửi Access Token:

```
GET /userinfo HTTP/1.1
Host: auth.example.com
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjFlOWdkazcifQ...
```

Response từ UserInfo Endpoint chứa các claims về người dùng tương ứng với scopes đã được cấp quyền:

```json
{
  "sub": "user123456",
  "name": "John Doe",
  "given_name": "John",
  "family_name": "Doe",
  "email": "john@example.com",
  "email_verified": true,
  "picture": "https://example.com/profile/john.jpg"
}
```

***

OpenID Connect đã trở thành tiêu chuẩn toàn cầu cho việc xác thực người dùng trong các ứng dụng hiện đại. Việc hiểu rõ giao thức này sẽ giúp bạn triển khai các hệ thống xác thực an toàn và linh hoạt cho ứng dụng của mình.

Trong các bài viết tiếp theo, chúng ta sẽ đi sâu vào việc triển khai thực tế OpenID Connect với các nhà cung cấp danh tính phổ biến và các kịch bản tích hợp nâng cao. Hãy theo dõi blog của tôi để không bỏ lỡ những thông tin hữu ích!
