---
icon: alien-8bit
---

# Keycloak Key Features

### Quản lý người dùng và tài khoản

#### Đăng ký và tự quản lý tài khoản

* **Đăng ký linh hoạt**:
  * Tự đăng ký với xác minh email/điện thoại
  * Yêu cầu reCAPTCHA để ngăn chặn bot
  * Quy trình xác minh email tùy chỉnh
  * Có thể kích hoạt/vô hiệu hóa tự đăng ký theo realm
* **Giao diện quản lý tài khoản**:
  * Bảng điều khiển thân thiện với người dùng
  * Quản lý thông tin cá nhân và mật khẩu
  * Thiết lập xác thực hai yếu tố
  * Xem và quản lý các ứng dụng được cấp quyền
  * Theo dõi lịch sử đăng nhập
  * Xem và hủy phiên đang hoạt động
  * Quản lý các thiết bị đã đăng nhập
* **Hỗ trợ khôi phục tài khoản**:
  * Quy trình đặt lại mật khẩu tùy chỉnh
  * Các câu hỏi bảo mật để khôi phục
  * Link đặt lại mật khẩu có thời hạn
  * Khôi phục thông qua OTP qua SMS/email

#### Quản lý tập trung

* **Console quản trị viên toàn diện**:
  * Quản lý người dùng theo batch
  * Giao diện quản trị trực quan
  * Tìm kiếm và lọc người dùng nâng cao
  * Quản lý theo cấu trúc tổ chức (nhóm/phòng ban)
* **Quản lý thuộc tính người dùng**:
  * Thuộc tính tùy chỉnh không giới hạn
  * Ánh xạ thuộc tính vào token
  * Điều kiện xác thực dựa trên thuộc tính
  * Tùy chỉnh trường bắt buộc/tùy chọn
* **Vòng đời người dùng**:
  * Tạo/cập nhật/xóa/vô hiệu hóa tài khoản
  * Khóa tài khoản tạm thời hoặc vĩnh viễn
  * Quản lý tài khoản hàng loạt
  * Nhập/xuất người dùng qua CSV/JSON

#### Chính sách mật khẩu

* **Quy tắc mật khẩu mạnh**:
  * Độ dài tối thiểu/tối đa
  * Yêu cầu ký tự đặc biệt, chữ hoa/thường, số
  * Kiểm tra mật khẩu phổ biến/yếu
  * Kiểm tra mật khẩu bị lộ (haveibeenpwned)
  * Quy tắc tùy chỉnh qua script
* **Quản lý vòng đời mật khẩu**:
  * Thời gian hết hạn mật khẩu tùy chỉnh
  * Thông báo hết hạn trước
  * Lịch sử mật khẩu (ngăn tái sử dụng)
  * Thời gian tối thiểu giữa các lần đổi mật khẩu
* **Bảo vệ tài khoản**:
  * Khóa tài khoản sau n lần đăng nhập thất bại
  * Thời gian khóa tăng dần
  * Giải pháp CAPTCHA sau các lần thất bại
  * Thông báo về hoạt động đáng ngờ

#### Hệ thống vai trò và phân quyền

* **Quản lý vai trò phân cấp**:
  * Vai trò realm toàn cục
  * Vai trò client cụ thể
  * Vai trò tổng hợp (composite roles)
  * Thừa kế vai trò
* **Quản lý nhóm**:
  * Cấu trúc nhóm phân cấp không giới hạn
  * Thừa kế vai trò từ nhóm cha
  * Quản lý thuộc tính cấp nhóm
  * Quản lý default groups
* **Các loại phân quyền**:
  * Role-based Access Control (RBAC)
  * Attribute-based Access Control (ABAC)
  * Context-based access control
  * Time-based access control
  * Resource-based permissions

### Tích hợp nhiều Identity Providers

#### Social Login

* **Nhà cung cấp được tích hợp sẵn**:
  * Google
  * Facebook
  * Twitter
  * GitHub
  * LinkedIn
  * Microsoft/Office 365
  * Instagram
  * PayPal
  * Stack Overflow
  * GitLab
  * BitBucket
  * Apple
  * Discord
  * Twitch
* **Tính năng xã hội nâng cao**:
  * Ánh xạ thuộc tính tùy chỉnh
  * Cập nhật hồ sơ người dùng từ mạng xã hội
  * Liên kết nhiều tài khoản xã hội với một tài khoản
  * First Login Flow tùy chỉnh
  * Yêu cầu thông tin bổ sung sau đăng nhập xã hội

#### SAML 2.0 Identity Providers

* **Tích hợp với IdP doanh nghiệp**:
  * Active Directory Federation Services (ADFS)
  * Shibboleth
  * PingFederate
  * Okta
  * OneLogin
  * Auth0
  * SimpleSAMLphp
  * ForgeRock
* **Tính năng SAML IdP**:
  * Metadata import/export
  * Signature và encryption settings
  * Session và binding customization
  * ValidatingX509 certificates
  * Thuộc tính ánh xạ và điều kiện

#### OpenID Connect Providers

* **Tích hợp với nhà cung cấp OIDC**:
  * Google
  * Microsoft Entra ID (Azure AD)
  * Auth0
  * Okta
  * Amazon Cognito
  * Ping Identity
  * Tùy chỉnh OIDC providers khác
* **Tính năng OIDC nâng cao**:
  * Client Authentication Methods
  * JWT/JWS/JWE verification
  * Claim mapping
  * Token validation

#### Kerberos/LDAP/Active Directory

* **Tích hợp Kerberos**:
  * Hỗ trợ SPNEGO
  * Windows desktop SSO
  * Hỗ trợ MIT Kerberos
  * Active Directory Domain Services integration
* **Tích hợp LDAP toàn diện**:
  * Hỗ trợ nhiều LDAP servers
  * Federated và external LDAP
  * LDAP Group mapping
  * Hỗ trợ TLS/SSL
  * User Federation Storage
  * LDAP filters tùy chỉnh
* **Active Directory**:
  * Đồng bộ hóa người dùng và nhóm
  * Ánh xạ vai trò AD
  * Tìm kiếm qua nhiều domain
  * Group Sync tùy chỉnh
  * Cross-domain authentication

#### Identity Brokering

* **Broker capabilities**:
  * First Login Flow tùy chỉnh
  * Account Linking
  * Forced Identity Provider
  * IDP Discovery
  * Conditional IDP Routing
* **Mapper configurations**:
  * Hardcoded role mappers
  * JavaScript mappers
  * Attribute to role mappers
  * Advanced claim mappings
* **Session management**:
  * Broker cookie settings
  * Store tokens
  * Passthrough authentication
  * Session inheritance

#### User Federation

* **Đồng bộ hóa người dùng**:
  * Periodic synchronization
  * Changed users import
  * Just-in-time provisioning
  * Định kỳ đồng bộ toàn bộ hoặc chỉ thay đổi
  * Cấu hình cache để tối ưu hiệu suất
* **Hệ thống Storage SPI**:
  * Giải pháp linh hoạt cho lưu trữ người dùng
  * Kết nối với cơ sở dữ liệu hiện có
  * Phân tầng user federation providers
  * Triển khai UserStorageProvider tùy chỉnh

### Hỗ trợ nhiều protocols

#### OpenID Connect (OIDC)

* **OIDC Core implementation**:
  * Authorization Code Flow
  * Implicit Flow
  * Hybrid Flow
  * Client Credentials Flow
  * Resource Owner Password Credentials Flow
  * Device Authorization Grant
  * Token Exchange
  * Certificate Bound Access Tokens
* **OIDC Discovery**:
  * Well-known configuration endpoint
  * JWKS endpoint
  * Dynamic registration
* **OIDC Endpoints**:
  * Authorization endpoint
  * Token endpoint
  * UserInfo endpoint
  * End session endpoint
  * Revocation endpoint
  * Introspection endpoint
  * Device authorization endpoint
* **Token management**:
  * Access token
  * ID token
  * Refresh token
  * Proof Key for Code Exchange (PKCE)
  * Request object
  * Signed JWT
  * Request parameter support

#### OAuth 2.0

* **OAuth 2.0 Core**:
  * Tất cả loại grant tiêu chuẩn
  * Token introspection
  * Token revocation
  * Client credentials caching
  * Token Exchange
  * Resource Indicators
* **OAuth 2.0 nâng cao**:
  * Token customization
  * Audience restrictions
  * Scope alignment
  * Resource Owner verification
  * Client Credentials caching
  * Custom consent screen

#### SAML 2.0

* **SAML Service Provider**:
  * SP-initiated flow
  * IdP-initiated flow
  * POST và Redirect bindings
  * Signature và encryption
  * Attribute statements
  * Artifact binding
* **SAML Identity Provider**:
  * Metadata management
  * X.509 certificate validation
  * SAML assertion customization
  * Client adapter
  * Identity Provider Discovery Service
  * Entity ID configuration
* **SAML Extensions**:
  * SAML protocol extensions
  * Attribute support
  * Auth context reference classes
  * Custom authentication statements

#### Advanced Protocol Features

* **User-Managed Access (UMA 2.0)**:
  * Resource registration
  * Permission tickets
  * Policy evaluation
  * UMA grants
  * Requesting party claims
* **Financial-grade API (FAPI)**:
  * FAPI-RW profile
  * JARM support
  * PAR support
  * JWT secured authorization requests
  * Mutual TLS client authentication
* **Cross-domain and mobile support**:
  * CORS support
  * PKCE for mobile apps
  * Silent authentication
  * Session iframe
  * Cross-device logout

### Tùy biến UI và branding

#### Hệ thống theme

* **Kiến trúc theme**:
  * Base themes (keycloak, base, keycloak-preview)
  * Theme inheritance
  * Caching control
  * Hot deployment
  * Tổ chức cấu trúc thư mục theme
* **Các loại theme**:
  * Account theme (self-service portal)
  * Admin theme (admin console)
  * Email theme (email templates)
  * Login theme (login pages)
  * Welcome theme (landing page)
* **Công nghệ front-end**:
  * Hỗ trợ FreeMarker templates
  * CSS customization
  * JavaScript extensions
  * Responsive layouts
  * i18n support

#### Branding chi tiết

* **Phần tử thương hiệu tùy chỉnh**:
  * Logo
  * Favicon
  * Background images
  * Login page layout
  * CSS variables
  * Typography
  * Color schemes
  * Buttons và form elements
* **Tùy chỉnh theo realm/client**:
  * Realm-specific themes
  * Client-specific themes
  * Theme selector
  * Theme by hostname
  * Default theme setting
* **White labeling**:
  * Loại bỏ Keycloak branding
  * Tùy chỉnh page title
  * Tùy chỉnh term of service
  * Tùy chỉnh error pages
  * Tùy chỉnh success messages

#### Đa ngôn ngữ

* **Hỗ trợ quốc tế hóa**:
  * Hỗ trợ nhiều ngôn ngữ mặc định
  * Ngôn ngữ dựa trên browser locale
  * Language selector trên giao diện
  * Kế thừa bản dịch
  * Override cụ thể cho từng ngôn ngữ
* **Cấu hình dịch**:
  * Tập tin properties cho mỗi ngôn ngữ
  * Message bundles
  * Thêm ngôn ngữ mới
  * Ghi đè messages có sẵn
  * RTL language support

#### Tùy chỉnh Email và thông báo

* **Email templates**:
  * Verification email
  * Password recovery
  * Execution notification
  * Event notification
  * HTML và text format
  * Tùy chỉnh email sender
  * Macros và biến trong template
* **SMS integration**:
  * Tích hợp nhà cung cấp SMS
  * Template cho SMS
  * SMS cho two-factor authentication
  * SMS cho verification

### Admin APIs và Client Adapters

#### RESTful Admin API

* **Quản trị API toàn diện**:
  * API endpoints cho tất cả chức năng quản trị
  * User management
  * Client management
  * Role và group management
  * Identity provider configuration
  * Authentication flow management
  * Realm settings
  * Event configuration
* **Bảo mật API**:
  * Dựa trên OAuth 2.0
  * Role-based authorization
  * Fine-grained permissions
  * Token-based authentication
  * CORS configuration
* **Implementation**:
  * RESTful API design
  * JSON payloads
  * Response filtering
  * Pagination
  * Sorting
  * Search functionality

#### Account Management API

* **User self-service API**:
  * Tài khoản linked
  * Thông tin người dùng
  * Credentials
  * Sessions
  * Applications
  * Client consents
* **Bảo mật Account API**:
  * Permissions model
  * Consent required
  * JWT authenticated
  * Verification workflows

#### Client Adapters

* **Java adapters**:
  * WildFly/JBoss EAP
  * Spring Boot
  * Tomcat
  * Jetty
  * JavaEE/Jakarta EE
  * Servlet Filter adapter
  * JAX-RS integration
* **JavaScript adapters**:
  * Native JavaScript
  * Angular
  * React
  * Vue.js
  * Silent check-sso
  * Cordova support
* **Multi-platform adapters**:
  * .NET/ASP.NET Core
  * Python
  * Node.js
  * PHP
  * Ruby
  * Go
  * C++
  * Rust
* **Mobile adapters**:
  * Android
  * iOS
  * React Native
  * Flutter
  * Xamarin
* **Adapter features**:
  * Single sign-on
  * Single sign-out
  * Role propagation
  * Identity and access token validation
  * Refresh token
  * Session management
  * Backchannel logout

#### Client Registration API

* **Client management**:
  * Register clients
  * Update client configuration
  * Remove clients
  * Retrieve client configuration
  * Disable/enable clients
* **Registration auth methods**:
  * Anonymous access
  * Bearer token
  * Client credentials
  * Registration access tokens
  * Initial access tokens
* **Client profiles**:
  * Default client settings
  * Client template
  * Auto-generated client configuration
  * Validation profiles
  * Dynamic client registration

#### Token Exchange Service

* **Token exchange support**:
  * Exchange token between clients
  * Identity token to access token
  * Impersonation
  * Delegation
  * Audience restricted tokens
  * Permission downgrade
* **Use cases**:
  * Microservices
  * Front-channel communication
  * Back-channel communication
  * In-browser communication
  * First-party và third-party apps
  * Service-to-service authentication

### Bảo mật và audit logs

#### Xác thực đa yếu tố (MFA)

* **Phương pháp MFA tích hợp sẵn**:
  * Time-based One-time Password (TOTP)
  * SMS authentication
  * Email verification
  * WebAuthn (FIDO2)
  * Security keys
  * Mobile authenticator apps
  * Recovery codes
* **Quản lý MFA**:
  * Required
  * Optional
  * Conditional
  * Per-user enablement
  * Per-client enablement
  * Remember device
* **Conditional MFA**:
  * Dựa trên IP address
  * Dựa trên user role
  * Dựa trên user attributes
  * Dựa trên client
  * Dựa trên resource
  * JavaScript điều kiện tùy chỉnh

#### Brute Force Detection

* **Phát hiện tấn công**:
  * Permanent lockout
  * Temporary lockout
  * Incremental lockout
  * Progressive delays
  * Notification alerts
* **Cấu hình bảo vệ**:
  * Max login failures
  * Failure reset time
  * Max lockout time
  * Quick login check
  * Notification emails

#### Advanced Security Features

* **CORS support**:
  * Origin validation
  * Pre-flight requests
  * Credential support
  * Allowed methods
  * Allowed headers
  * Per-client CORS configuration
* **Content Security Policy**:
  * CSP headers
  * Frame options
  * XSS protection
  * X-Content-Type-Options
  * Content-Security-Policy headers
* **Session Security**:
  * Session timeout settings
  * Idle session timeout
  * Session validation
  * Remember me
  * Different session handling per client
  * Revocation policies
  * Absolute session lifetime
  * Offline tokens

#### Fine-grained Authorization

* **Policy Enforcement Point (PEP)**:
  * Authorization services client
  * Claim-based authorization
  * Push vs. pull model
  * Time-limited permissions
  * Permission evaluation
  * Decision caching
* **Policy structure**:
  * Resources
  * Scopes
  * Permissions
  * Policies
  * Policy providers
  * JavaScript policies
  * Role policies
  * Time policies
  * User policies
  * Aggregated policies
* **Evaluation modes**:
  * All
  * Any
  * Decision strategy customization
  * Negative permissions
  * Permission UMA tickets

#### Audit Logging System

* **Login events**:
  * Success/failure tracking
  * IP address logging
  * User agent tracking
  * Timestamp
  * Authentication details
  * Identity provider information
  * Error details
* **Admin events**:
  * Resource types
  * Operation types
  * Auth details
  * Representation
  * Resource path
  * Error tracking
* **Storage và export**:
  * Database storage
  * JMS integration
  * Custom event listeners
  * Syslog integration
  * ELK integration
  * Splunk integration
  * Log rotation
  * Log pruning
  * Configurable retention policy
* **Compliance support**:
  * GDPR consent tracking
  * PII logging controls
  * Log filtering
  * Data minimization options
  * Privacy by design
  * Right to be forgotten implementation
  * Data access request handling
  * Data portability support

***

Với bộ tính năng chi tiết và toàn diện này, Keycloak không chỉ là một giải pháp xác thực đơn giản mà còn là một nền tảng quản lý danh tính và truy cập hoàn chỉnh, đáp ứng được các yêu cầu phức tạp nhất của doanh nghiệp hiện đại. Khả năng tùy biến cao và kiến trúc plugin mở rộng cho phép Keycloak thích ứng với hầu hết các trường hợp sử dụng, từ các ứng dụng nhỏ đến hệ thống doanh nghiệp quy mô lớn.
