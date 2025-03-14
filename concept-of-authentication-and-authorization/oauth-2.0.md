---
icon: key-skeleton
---

# OAuth 2.0

_Chào các bạn! Hôm nay mình sẽ chia sẻ về một trong những mô hình xác thực và phân quyền quan trọng bậc nhất trong thế giới phát triển ứng dụng hiện đại - OAuth 2.0. Bài viết này sẽ giúp bạn hiểu rõ về các thành phần, luồng hoạt động và cách quản lý token trong OAuth 2.0._

### OAuth 2.0 là gì và tại sao cần nó?

OAuth 2.0 (Open Authorization) là một giao thức ủy quyền cho phép các ứng dụng bên thứ ba có thể truy cập vào tài nguyên của người dùng mà không cần biết thông tin đăng nhập của họ. Đây là một tiêu chuẩn mở được phát triển và duy trì bởi IETF OAuth Working Group.

Tưởng tượng bạn đang sử dụng một ứng dụng quản lý tài chính và muốn kết nối nó với tài khoản ngân hàng của mình. Nếu không có OAuth, bạn sẽ phải cung cấp tên người dùng và mật khẩu ngân hàng cho ứng dụng đó - điều này tiềm ẩn nhiều rủi ro bảo mật. Với OAuth 2.0, bạn có thể cho phép ứng dụng truy cập vào dữ liệu ngân hàng mà không cần chia sẻ thông tin đăng nhập.

### Các thành phần chính trong mô hình OAuth 2.0

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Các thành phần chính trong mô hình OAuth 2.0</p></figcaption></figure>

#### 1. Resource Owner (Chủ sở hữu tài nguyên)

Đây chính là người dùng - người sở hữu dữ liệu và có khả năng cấp quyền truy cập vào dữ liệu đó. Ví dụ, khi bạn cho phép một ứng dụng truy cập vào tài khoản Google Drive của mình, bạn chính là Resource Owner.

#### 2. Client (Ứng dụng khách)

Đây là ứng dụng muốn truy cập vào dữ liệu của Resource Owner. Client phải được đăng ký trước với Authorization Server và nhận được một định danh duy nhất (client\_id) và thường kèm theo một "bí mật" (client\_secret). Clients có thể là ứng dụng web, ứng dụng di động, hoặc thậm chí là ứng dụng chạy trên server.

#### 3. Authorization Server (Máy chủ xác thực)

Đây là server chịu trách nhiệm xác thực Resource Owner và cấp phát token ủy quyền cho Client. Authorization Server thực hiện hai công việc chính:

* Xác thực người dùng và lấy sự đồng ý
* Cấp phát access token và refresh token

Ví dụ về các Authorization Server phổ biến: Google OAuth 2.0, Facebook Login, Auth0, Okta.

#### 4. Resource Server (Máy chủ tài nguyên)

Đây là nơi lưu trữ và cung cấp tài nguyên mà Client muốn truy cập. Resource Server sẽ kiểm tra tính hợp lệ của access token trước khi cung cấp tài nguyên. Trong nhiều trường hợp, Resource Server và Authorization Server có thể là một phần của cùng một hệ thống.

### Các luồng ủy quyền (Grant Types) trong OAuth 2.0

OAuth 2.0 định nghĩa nhiều luồng ủy quyền khác nhau để phù hợp với các tình huống sử dụng khác nhau. Dưới đây là bốn luồng chính:

#### 1. Authorization Code Flow

Đây là luồng phổ biến nhất và an toàn nhất, thích hợp cho ứng dụng web có server-side:

1. Client chuyển hướng người dùng đến Authorization Server
2. Người dùng xác thực và đồng ý cấp quyền
3. Authorization Server trả về một authorization code cho Client
4. Client đổi authorization code lấy access token và refresh token bằng cách gửi request kèm client\_secret

**Ưu điểm:**

* Access token không bao giờ được tiết lộ cho trình duyệt
* Có thể sử dụng refresh token để duy trì phiên làm việc lâu dài
* Hỗ trợ xác thực hai chiều giữa Client và Authorization Server

**Code ví dụ:**

```javascript
// Bước 1: Chuyển hướng người dùng đến trang xác thực
const authUrl = 'https://auth-server.com/auth';
const redirectUri = 'https://my-app.com/callback';
const clientId = 'my-client-id';
const scope = 'read write';
window.location.href = `${authUrl}?client_id=${clientId}&redirect_uri=${redirectUri}&response_type=code&scope=${scope}`;

// Bước 3-4: Tại endpoint callback, đổi authorization code lấy token
// Server-side code (Node.js)
app.get('/callback', async (req, res) => {
  const code = req.query.code;
  
  const tokenResponse = await fetch('https://auth-server.com/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({
      grant_type: 'authorization_code',
      code: code,
      redirect_uri: 'https://my-app.com/callback',
      client_id: 'my-client-id',
      client_secret: 'my-client-secret'
    })
  });
  
  const tokens = await tokenResponse.json();
  // Lưu tokens và chuyển hướng người dùng
});
```

#### 2. Implicit Flow

Luồng này được thiết kế cho các ứng dụng client-side (SPA - Single Page Applications) không có server bảo mật:

1. Client chuyển hướng người dùng đến Authorization Server
2. Người dùng xác thực và đồng ý cấp quyền
3. Authorization Server trả về access token trực tiếp trong URL fragment (#)

**Lưu ý:** Luồng này không an toàn bằng Authorization Code và không hỗ trợ refresh token. Hiện nay, nó đã dần được thay thế bằng Authorization Code Flow with PKCE cho các ứng dụng SPA.

#### 3. Resource Owner Password Credentials Flow

Trong luồng này, Client trực tiếp thu thập thông tin đăng nhập của người dùng và sử dụng để lấy token:

1. Người dùng cung cấp username và password cho Client
2. Client gửi thông tin đăng nhập này cùng với client\_id và client\_secret đến Authorization Server
3. Authorization Server trả về access token và refresh token

**Cảnh báo:** Luồng này chỉ nên được sử dụng khi Client là ứng dụng đáng tin cậy cao (thường là ứng dụng chính thức của dịch vụ) và các luồng khác không khả thi.

#### 4. Client Credentials Flow

Luồng này được sử dụng cho giao tiếp machine-to-machine khi không có sự tham gia của người dùng:

1. Client gửi client\_id và client\_secret đến Authorization Server
2. Authorization Server trả về access token

Đây là luồng đơn giản nhất và thích hợp cho các API không liên quan đến dữ liệu người dùng cụ thể.

### Tokens trong OAuth 2.0: Định nghĩa và Vòng đời

#### 1. Access Token

Access token là chìa khóa cho Client để truy cập vào tài nguyên được bảo vệ trên Resource Server. Access token thường có thời gian sống ngắn (30-60 phút) để giảm thiểu rủi ro nếu bị lộ.

**Cấu trúc:** Access token có thể là một chuỗi ngẫu nhiên không trong suốt hoặc một JWT (JSON Web Token) có chứa thông tin về quyền truy cập.

```javascript
// Sử dụng access token để gọi API
fetch('https://api.example.com/user/profile', {
  headers: {
    'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'
  }
});
```

#### 2. Refresh Token

Refresh token được sử dụng để lấy access token mới khi access token hết hạn mà không yêu cầu người dùng xác thực lại. Refresh token thường có thời gian sống dài hơn nhiều so với access token (có thể vài ngày, vài tuần hoặc thậm chí vĩnh viễn).

**Vòng đời và quản lý:**

1. Lưu trữ an toàn: Refresh token cần được lưu trữ an toàn ở phía server hoặc trong secure storage
2. Sử dụng khi cần: Khi access token hết hạn, client sử dụng refresh token để lấy access token mới
3. Xoay vòng (rotation): Một số hệ thống áp dụng cơ chế "refresh token rotation" - mỗi lần sử dụng refresh token sẽ cấp một refresh token mới
4. Thu hồi: Authorization Server có thể thu hồi refresh token nếu phát hiện hành vi đáng ngờ

```javascript
// Sử dụng refresh token để lấy access token mới
async function refreshAccessToken() {
  const response = await fetch('https://auth-server.com/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({
      grant_type: 'refresh_token',
      refresh_token: 'stored-refresh-token',
      client_id: 'my-client-id',
      client_secret: 'my-client-secret'
    })
  });
  
  const tokens = await response.json();
  // Lưu access token mới và refresh token mới (nếu có)
}
```

### Các vấn đề bảo mật cần lưu ý

1. **CSRF Attack:** Sử dụng state parameter để ngăn chặn tấn công Cross-Site Request Forgery
2. **Token Storage:** Lưu trữ access token trong memory và refresh token trong HTTP-only cookies
3. **PKCE (Proof Key for Code Exchange):** Sử dụng cho ứng dụng di động và SPA để bảo vệ authorization code
4. **Scope giới hạn:** Luôn yêu cầu quyền truy cập tối thiểu cần thiết
5. **Kiểm tra thường xuyên:** Theo dõi và thu hồi token không sử dụng

### Tổng kết

OAuth 2.0 là một giao thức mạnh mẽ giúp phân quyền an toàn cho ứng dụng của bạn. Việc hiểu rõ các thành phần, luồng hoạt động và cách quản lý token sẽ giúp bạn triển khai hệ thống xác thực bảo mật, linh hoạt và đáp ứng nhu cầu của người dùng.

Trong các dự án thực tế, OAuth 2.0 thường được kết hợp với OpenID Connect để cung cấp cả chức năng xác thực (authentication) và phân quyền (authorization).

