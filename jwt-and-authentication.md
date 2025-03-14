---
icon: jet-fighter-up
---

# JWT & authentication

### Cấu trúc JWT: Header, Payload, Signature

JSON Web Token (JWT) là một chuẩn mở (RFC 7519) định nghĩa một cách nhỏ gọn và khép kín để truyền thông tin an toàn giữa các bên dưới dạng đối tượng JSON. Thông tin này có thể được xác minh và tin cậy vì nó được ký số. JWT có thể được ký bằng một bí mật (sử dụng thuật toán HMAC) hoặc một cặp khóa công khai/riêng tư (sử dụng RSA hoặc ECDSA).

JWT bao gồm ba phần, được phân tách bằng dấu chấm (.):

* Header
* Payload
* Signature

Cấu trúc điển hình: `xxxxx.yyyyy.zzzzz`

#### 1. Header

Header thường bao gồm hai phần:

* Loại token (typ), giá trị là "JWT"
* Thuật toán ký được sử dụng (alg), như HMAC SHA256 hoặc RSA

Ví dụ:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Header này sau đó được mã hóa Base64Url để tạo thành phần đầu tiên của JWT.

#### 2. Payload

Payload chứa các claims. Claims là các tuyên bố về một thực thể (thường là người dùng) và các dữ liệu bổ sung. Có ba loại claims:

* **Registered claims**: Tập hợp các claims được định nghĩa trước, không bắt buộc nhưng được khuyến nghị sử dụng:
  * `iss` (issuer): Người phát hành token
  * `sub` (subject): Chủ thể của token
  * `aud` (audience): Đối tượng dự định nhận token
  * `exp` (expiration time): Thời điểm hết hạn của token
  * `nbf` (not before): Token không hợp lệ trước thời điểm này
  * `iat` (issued at): Thời điểm token được phát hành
  * `jti` (JWT ID): Định danh duy nhất cho token
* **Public claims**: Claims được định nghĩa tùy ý bởi những người sử dụng JWT. Để tránh xung đột, chúng nên được định nghĩa trong [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) hoặc sử dụng URI chứa không gian tên chống xung đột.
* **Private claims**: Claims tùy chỉnh được tạo để chia sẻ thông tin giữa các bên đã đồng ý sử dụng chúng.

Ví dụ:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true,
  "iat": 1516239022,
  "exp": 1516242622
}
```

Payload này sau đó được mã hóa Base64Url để tạo thành phần thứ hai của JWT.

**Lưu ý quan trọng**: Mặc dù JWT có thể được mã hóa để bảo vệ thông tin (JWE), nhưng JWT thông thường (JWS) chỉ được ký, không được mã hóa. Điều này có nghĩa là bất kỳ ai cũng có thể đọc thông tin trong JWT bằng cách giải mã Base64. Không nên đặt thông tin nhạy cảm trong payload trừ khi bạn sử dụng JWT được mã hóa (JWE).

#### 3. Signature

Để tạo phần chữ ký, bạn cần:

* Header đã mã hóa
* Payload đã mã hóa
* Một bí mật
* Thuật toán đã chỉ định trong header

Ví dụ, nếu bạn sử dụng thuật toán HMAC SHA256, chữ ký sẽ được tạo ra như sau:

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

Chữ ký này được sử dụng để xác minh rằng thông điệp không bị thay đổi trong quá trình truyền tải, và trong trường hợp token được ký bằng khóa riêng tư, nó cũng có thể xác minh rằng người gửi JWT là người nói đúng.

### Ký và xác thực JWT

#### Quy trình ký JWT

1. **Tạo header và payload**: Định nghĩa các claims cần thiết và thuật toán ký.
2. **Mã hóa Base64Url**: Mã hóa cả header và payload bằng Base64Url.
3. **Tạo chữ ký**: Sử dụng thuật toán được chỉ định để ký chuỗi `encodedHeader.encodedPayload` với một bí mật hoặc khóa riêng tư.
4. **Tạo JWT**: Kết hợp các phần đã mã hóa và chữ ký theo định dạng `encodedHeader.encodedPayload.signature`.

#### Quy trình xác thực JWT

1. **Tách JWT**: Tách JWT thành ba phần: header, payload và chữ ký.
2. **Giải mã header và payload**: Giải mã Base64Url của header và payload để lấy thông tin gốc.
3. **Kiểm tra chữ ký**:
   * Lấy thuật toán từ header
   * Tính toán lại chữ ký bằng cách sử dụng header, payload và bí mật hoặc khóa công khai
   * So sánh chữ ký được tính toán với chữ ký trong JWT
4. **Kiểm tra các claims**: Kiểm tra các claims có hiệu lực không:
   * Thời gian hiện tại có nằm giữa `nbf` và `exp` không?
   * `iss`, `aud` và các trường khác có chính xác không?

#### Ví dụ về xác thực JWT trong Node.js (sử dụng thư viện jsonwebtoken)

```javascript
const jwt = require('jsonwebtoken');

// Tạo JWT
function createToken(user) {
  const payload = {
    sub: user.id,
    name: user.name,
    admin: user.isAdmin,
    iat: Math.floor(Date.now() / 1000),
    exp: Math.floor(Date.now() / 1000) + (60 * 60) // Hết hạn sau 1 giờ
  };
  
  return jwt.sign(payload, 'your-secret-key', { algorithm: 'HS256' });
}

// Xác thực JWT
function verifyToken(token) {
  try {
    const decoded = jwt.verify(token, 'your-secret-key');
    return { valid: true, data: decoded };
  } catch (error) {
    return { valid: false, error: error.message };
  }
}

// Sử dụng trong Express middleware
function authMiddleware(req, res, next) {
  const authHeader = req.headers.authorization;
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ message: 'Authorization header missing or invalid' });
  }
  
  const token = authHeader.split(' ')[1];
  const result = verifyToken(token);
  
  if (!result.valid) {
    return res.status(401).json({ message: 'Invalid token', error: result.error });
  }
  
  req.user = result.data;
  next();
}
```

### Các loại thuật toán mã hóa (HMAC, RSA, ECDSA)

JWT hỗ trợ nhiều thuật toán mã hóa khác nhau để ký và xác thực token. Lựa chọn thuật toán phụ thuộc vào yêu cầu bảo mật, hiệu suất và trường hợp sử dụng cụ thể.

#### 1. HMAC (Hash-based Message Authentication Code)

HMAC kết hợp một hàm băm mật mã (như SHA-256) với một khóa bí mật để tạo mã xác thực tin nhắn.

**Thuật toán phổ biến**:

* **HS256** (HMAC với SHA-256): Thuật toán ký JWT phổ biến nhất
* **HS384** (HMAC với SHA-384): Cung cấp mức bảo mật cao hơn HS256
* **HS512** (HMAC với SHA-512): Cung cấp mức bảo mật cao nhất trong số các thuật toán HMAC

**Đặc điểm**:

* Sử dụng cùng một khóa bí mật cho cả ký và xác thực
* Khóa bí mật phải được bảo vệ cẩn thận và chia sẻ an toàn giữa các bên
* Hiệu suất nhanh hơn so với thuật toán chữ ký bất đối xứng
* Thích hợp cho các ứng dụng nội bộ hoặc khi cả người tạo và người xác minh token đều đáng tin cậy

**Ví dụ ký với HS256 (Node.js)**:

```javascript
const jwt = require('jsonwebtoken');

const token = jwt.sign({ sub: 'user123' }, 'shared-secret-key', { algorithm: 'HS256' });
```

#### 2. RSA (Rivest–Shamir–Adleman)

RSA là một hệ thống mật mã bất đối xứng sử dụng cặp khóa công khai/riêng tư. Token được ký bằng khóa riêng tư và xác thực bằng khóa công khai tương ứng.

**Thuật toán phổ biến**:

* **RS256** (RSA Signature với SHA-256): Thuật toán RSA phổ biến nhất cho JWT
* **RS384** (RSA Signature với SHA-384)
* **RS512** (RSA Signature với SHA-512)

**Đặc điểm**:

* Sử dụng khóa riêng tư để ký token và khóa công khai để xác thực
* Khóa công khai có thể được phân phối rộng rãi mà không gây ra rủi ro bảo mật
* Khóa riêng tư phải được bảo vệ cẩn thận
* Phù hợp cho các ứng dụng phân tán và kiến trúc có nhiều bên tin cậy
* Kích thước token lớn hơn so với HMAC và ECDSA
* Hiệu suất chậm hơn so với HMAC nhưng mang lại lợi ích về bảo mật

**Ví dụ ký với RS256 (Node.js)**:

```javascript
const jwt = require('jsonwebtoken');
const fs = require('fs');

const privateKey = fs.readFileSync('private.key');
const token = jwt.sign({ sub: 'user123' }, privateKey, { algorithm: 'RS256' });

// Xác thực
const publicKey = fs.readFileSync('public.key');
const verified = jwt.verify(token, publicKey);
```

#### 3. ECDSA (Elliptic Curve Digital Signature Algorithm)

ECDSA là một biến thể của thuật toán chữ ký số (DSA) sử dụng mật mã đường cong elliptic. Giống như RSA, ECDSA sử dụng cặp khóa công khai/riêng tư.

**Thuật toán phổ biến**:

* **ES256** (ECDSA với P-256 và SHA-256)
* **ES384** (ECDSA với P-384 và SHA-384)
* **ES512** (ECDSA với P-521 và SHA-512)

**Đặc điểm**:

* Sử dụng khóa riêng tư để ký token và khóa công khai để xác thực
* Cung cấp mức bảo mật tương đương với RSA nhưng với khóa nhỏ hơn
* Tạo ra chữ ký nhỏ hơn so với RSA, dẫn đến kích thước token nhỏ hơn
* Hiệu quả hơn về mặt tính toán so với RSA
* Là lựa chọn tốt cho các ứng dụng di động hoặc IoT cần bảo mật cao nhưng có tài nguyên hạn chế

**Ví dụ ký với ES256 (Node.js)**:

```javascript
const jwt = require('jsonwebtoken');
const fs = require('fs');

const privateKey = fs.readFileSync('ec-private.key');
const token = jwt.sign({ sub: 'user123' }, privateKey, { algorithm: 'ES256' });

// Xác thực
const publicKey = fs.readFileSync('ec-public.key');
const verified = jwt.verify(token, publicKey);
```

#### So sánh các thuật toán

| Thuật toán                  | Loại khóa                     | Kích thước chữ ký | Hiệu suất  | Trường hợp sử dụng                                            |
| --------------------------- | ----------------------------- | ----------------- | ---------- | ------------------------------------------------------------- |
| HMAC (HS256, HS384, HS512)  | Đối xứng (shared secret)      | Nhỏ               | Nhanh nhất | Hệ thống nội bộ, một bên duy nhất xác thực token              |
| RSA (RS256, RS384, RS512)   | Bất đối xứng (public/private) | Lớn               | Chậm nhất  | Hệ thống phân tán, nhiều bên xác thực token                   |
| ECDSA (ES256, ES384, ES512) | Bất đối xứng (public/private) | Trung bình        | Trung bình | Ứng dụng di động, IoT, cần cân bằng giữa bảo mật và hiệu suất |

#### Lựa chọn thuật toán phù hợp

* **Sử dụng HMAC** khi:
  * Bạn kiểm soát cả phía tạo và xác thực token
  * Khóa bí mật có thể được chia sẻ an toàn
  * Hiệu suất là ưu tiên hàng đầu
* **Sử dụng RSA** khi:
  * Có nhiều bên xác thực token
  * Người tạo token và người xác thực không hoàn toàn tin tưởng lẫn nhau
  * Cần phân phối khóa công khai rộng rãi
* **Sử dụng ECDSA** khi:
  * Cần lợi ích của RSA nhưng với hiệu suất tốt hơn
  * Kích thước token là vấn đề quan trọng
  * Làm việc với thiết bị có tài nguyên hạn chế

### Bảo mật JWT và các lỗ hổng phổ biến

Mặc dù JWT là một tiêu chuẩn an toàn khi được sử dụng đúng cách, nhưng vẫn có nhiều lỗ hổng bảo mật phổ biến cần lưu ý:

#### 1. Lưu trữ không an toàn của khóa bí mật hoặc khóa riêng tư

**Vấn đề**: Khóa bí mật bị lộ sẽ cho phép kẻ tấn công tạo token giả mạo.

**Giải pháp**:

* Sử dụng các dịch vụ quản lý khóa (KMS) như AWS KMS hoặc Google Cloud KMS
* Không bao giờ nhúng khóa bí mật trực tiếp vào mã nguồn
* Sử dụng biến môi trường hoặc dịch vụ lưu trữ bí mật an toàn
* Xoay khóa thường xuyên
* Sử dụng khóa đủ mạnh (ít nhất 256 bit cho HMAC)

#### 2. Không xác thực thuật toán (Lỗ hổng "none" algorithm)

**Vấn đề**: Một số thư viện JWT cho phép sử dụng thuật toán "none", cho phép kẻ tấn công bỏ qua xác thực chữ ký hoàn toàn.

**Giải pháp**:

* Luôn kiểm tra thuật toán trong token trước khi xác thực
* Chỉ chấp nhận các thuật toán cụ thể mà bạn mong đợi
* Sử dụng thư viện JWT hiện đại đã vá lỗi này

```javascript
// Cách làm đúng
jwt.verify(token, secretOrPublicKey, { algorithms: ['HS256'] });
```

#### 3. Lỗ hổng do chuyển đổi thuật toán (Algorithm Confusion Attack)

**Vấn đề**: Kẻ tấn công có thể thay đổi thuật toán từ bất đối xứng (như RS256) sang đối xứng (như HS256) và sử dụng khóa công khai làm bí mật.

**Giải pháp**:

* Luôn xác thực thuật toán trong token
* Chỉ sử dụng một loại thuật toán, hoặc quản lý chúng cẩn thận
* Sử dụng các thư viện được cập nhật để ngăn chặn lỗi này

#### 4. Không xác thực claims quan trọng

**Vấn đề**: Bỏ qua việc kiểm tra các claims như `exp` (expiration time), `nbf` (not before), `aud` (audience) cho phép sử dụng token hết hạn hoặc không đúng đối tượng.

**Giải pháp**:

* Luôn kiểm tra tất cả các claims liên quan
* Sử dụng thời gian hết hạn ngắn (short expiration times)
* Thêm các claims như `jti` (JWT ID) để ngăn chặn tái sử dụng token

```javascript
jwt.verify(token, secretOrPublicKey, {
  algorithms: ['RS256'],
  audience: 'your-api',
  issuer: 'your-auth-server'
});
```

#### 5. Lưu trữ JWT không an toàn ở client

**Vấn đề**: Lưu trữ JWT trong localStorage hoặc sessionStorage khiến chúng dễ bị tấn công XSS.

**Giải pháp**:

* Ưu tiên lưu trữ trong cookie với các cờ HttpOnly, Secure và SameSite
* Nếu phải lưu trữ trong JavaScript, xem xét cơ chế làm mờ (token obfuscation)
* Sử dụng thời gian hết hạn ngắn để giảm thiểu tác động nếu token bị đánh cắp

```javascript
// Thiết lập cookie an toàn
res.cookie('access_token', token, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 3600000 // 1 giờ
});
```

#### 6. Thông tin nhạy cảm trong Payload

**Vấn đề**: JWT thông thường (JWS) không được mã hóa, chỉ được ký. Bất kỳ ai cũng có thể đọc payload bằng cách giải mã Base64.

**Giải pháp**:

* Không bao giờ đặt thông tin nhạy cảm trong JWT
* Sử dụng JWE (JSON Web Encryption) nếu cần mã hóa dữ liệu
* Chỉ lưu trữ các định danh trong JWT, không phải dữ liệu người dùng đầy đủ

#### 7. Không có cơ chế thu hồi (Revocation)

**Vấn đề**: JWT được thiết kế để tự xác thực, không yêu cầu trạng thái phía máy chủ, khiến việc thu hồi token trước khi hết hạn trở nên khó khăn.

**Giải pháp**:

* Sử dụng thời gian hết hạn ngắn với token refresh
* Triển khai danh sách token bị từ chối (token blacklist)
* Sử dụng cơ chế vô hiệu hóa người dùng nếu cần
* Xem xét Redis hoặc các cơ sở dữ liệu trong bộ nhớ để lưu trữ token bị thu hồi

```javascript
// Kiểm tra token trong blacklist
async function verifyToken(token) {
  // Kiểm tra xem token có trong blacklist không
  const isBlacklisted = await redis.exists(`blacklist:${token}`);
  if (isBlacklisted) {
    throw new Error('Token has been revoked');
  }
  
  // Xác thực token như bình thường
  return jwt.verify(token, secretOrPublicKey);
}

// Thu hồi token
async function revokeToken(token) {
  const decoded = jwt.decode(token);
  const expiryTime = decoded.exp - Math.floor(Date.now() / 1000);
  
  // Thêm token vào blacklist cho đến khi nó hết hạn
  await redis.set(`blacklist:${token}`, '1', 'EX', expiryTime);
}
```

#### 8. Sử dụng JWT cho phiên làm việc (Sessions)

**Vấn đề**: JWT không phải lúc nào cũng phù hợp để thay thế phiên làm việc truyền thống, đặc biệt là khi cần vô hiệu hóa phiên ngay lập tức.

**Giải pháp**:

* Xem xét liệu JWTs có phải là sự lựa chọn đúng đắn cho trường hợp sử dụng của bạn không
* Đối với phiên làm việc người dùng thông thường, có thể sử dụng session cookies truyền thống
* Sử dụng JWT chủ yếu cho API stateless và các tình huống cụ thể

#### 9. Tấn công man-in-the-middle (MITM)

**Vấn đề**: Nếu JWT được truyền qua kênh không an toàn, chúng có thể bị đánh cắp.

**Giải pháp**:

* Luôn sử dụng HTTPS
* Sử dụng cờ "Secure" cho cookies
* Triển khai HSTS (HTTP Strict Transport Security)

#### 10. Lỗ hổng do triển khai JWT tùy chỉnh

**Vấn đề**: Tự triển khai xác thực JWT có thể dẫn đến các lỗ hổng bảo mật.

**Giải pháp**:

* Sử dụng các thư viện JWT đã được kiểm tra và cập nhật
* Tránh viết mã xác thực JWT từ đầu
* Theo dõi các bản vá và cập nhật cho thư viện JWT bạn sử dụng

#### Danh sách kiểm tra bảo mật JWT

1. ✅ Sử dụng HTTPS cho tất cả các yêu cầu
2. ✅ Bảo vệ khóa bí mật/riêng tư
3. ✅ Xác thực thuật toán chữ ký
4. ✅ Thiết lập thời gian hết hạn ngắn (15-60 phút)
5. ✅ Xác thực tất cả các claims quan trọng (exp, nbf, aud, iss)
6. ✅ Không lưu trữ thông tin nhạy cảm trong payload
7. ✅ Triển khai cơ chế làm mới token (refresh token)
8. ✅ Xem xét các chiến lược thu hồi token
9. ✅ Lưu trữ token an toàn ở phía client
10. ✅ Sử dụng các thư viện JWT đáng tin cậy và cập nhật

***

JWT cung cấp một cách linh hoạt và mạnh mẽ để xác thực và truyền thông tin giữa các bên. Tuy nhiên, việc sử dụng chúng an toàn đòi hỏi hiểu biết về các lỗ hổng tiềm ẩn và thực hành tốt nhất. Bằng cách tuân thủ các nguyên tắc bảo mật được nêu ở trên, bạn có thể tận dụng sức mạnh của JWT trong khi vẫn duy trì bảo mật cho ứng dụng của mình.
