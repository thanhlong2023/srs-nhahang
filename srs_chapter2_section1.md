# CHƯƠNG 2: KIẾN TRÚC & TỔNG QUAN HỆ THỐNG

## 2.1. Kiến trúc 3 Tầng (3-Tier Architecture)

### 2.1.1. Tổng quan Kiến trúc

Hệ thống PTIT-RMS được thiết kế theo mô hình **kiến trúc 3 tầng (3-Tier Architecture)** nhằm đảm bảo:

✅ **Tách biệt rõ ràng** giữa giao diện người dùng, logic nghiệp vụ và dữ liệu  
✅ **Dễ bảo trì và nâng cấp** từng tầng độc lập  
✅ **Khả năng mở rộng** khi tăng số lượng người dùng hoặc chức năng  
✅ **Bảo mật tốt hơn** với việc kiểm soát truy cập theo từng tầng  
✅ **Tái sử dụng code** thông qua các service và module

### 2.1.2. Sơ đồ Kiến trúc Tổng thể

```
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT DEVICES                                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│  │   Desktop    │  │   Tablet     │  │   Mobile     │              │
│  │  (1920x1080) │  │  (1024x768)  │  │  (375x667)   │              │
│  └──────────────┘  └──────────────┘  └──────────────┘              │
│         │                  │                  │                      │
│         └──────────────────┴──────────────────┘                      │
│                            │                                         │
│                     HTTPS / WebSocket                                │
└─────────────────────────────┬───────────────────────────────────────┘
                              │
┌─────────────────────────────▼───────────────────────────────────────┐
│                    PRESENTATION TIER (Tầng Giao diện)               │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                    React Application                           │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │ │
│  │  │   UI Layer   │  │  Redux State │  │   Router     │        │ │
│  │  │  Components  │  │  Management  │  │  (React      │        │ │
│  │  │  (Ant Design)│  │   (Zustand)  │  │   Router)    │        │ │
│  │  └──────────────┘  └──────────────┘  └──────────────┘        │ │
│  │                                                                 │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │ │
│  │  │  API Client  │  │  WebSocket   │  │   Auth       │        │ │
│  │  │   (Axios)    │  │   Client     │  │   Manager    │        │ │
│  │  └──────────────┘  └──────────────┘  └──────────────┘        │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────┬───────────────────────────────────────┘
                              │
                         REST API / WS
                              │
┌─────────────────────────────▼───────────────────────────────────────┐
│               BUSINESS LOGIC TIER (Tầng Logic Nghiệp vụ)            │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                    Node.js Application                         │ │
│  │  ┌──────────────────────────────────────────────────────────┐ │ │
│  │  │              Express.js Framework                        │ │ │
│  │  └──────────────────────────────────────────────────────────┘ │ │
│  │                                                                 │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │ │
│  │  │ Controllers  │  │   Services   │  │ Middleware   │        │ │
│  │  │  (Routes)    │  │  (Business   │  │  - Auth      │        │ │
│  │  │              │  │    Logic)    │  │  - RBAC      │        │ │
│  │  └──────────────┘  └──────────────┘  │  - Logger    │        │ │
│  │                                       └──────────────┘        │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │ │
│  │  │ Repositories │  │   Models     │  │   Utils      │        │ │
│  │  │ (Data Access)│  │ (Sequelize)  │  │  - Validator │        │ │
│  │  └──────────────┘  └──────────────┘  │  - Formatter │        │ │
│  │                                       └──────────────┘        │ │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │ │
│  │  │  WebSocket   │  │   Scheduler  │  │   Queue      │        │ │
│  │  │   Server     │  │   (Cron)     │  │   (Bull)     │        │ │
│  │  └──────────────┘  └──────────────┘  └──────────────┘        │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────┬───────────────────────────────────────┘
                              │
                         SQL / Cache
                              │
┌─────────────────────────────▼───────────────────────────────────────┐
│                    DATA TIER (Tầng Dữ liệu)                          │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  ┌──────────────────────────────────────────────────────────┐ │ │
│  │  │              PostgreSQL Database                         │ │ │
│  │  │  ┌────────────┐  ┌────────────┐  ┌────────────┐        │ │ │
│  │  │  │   Tables   │  │   Views    │  │  Triggers  │        │ │ │
│  │  │  │  (20+)     │  │            │  │            │        │ │ │
│  │  │  └────────────┘  └────────────┘  └────────────┘        │ │ │
│  │  │  ┌────────────┐  ┌────────────┐  ┌────────────┐        │ │ │
│  │  │  │  Indexes   │  │   Stored   │  │ Functions  │        │ │ │
│  │  │  │            │  │ Procedures │  │            │        │ │ │
│  │  │  └────────────┘  └────────────┘  └────────────┘        │ │ │
│  │  └──────────────────────────────────────────────────────────┘ │ │
│  └────────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  ┌──────────────────────────────────────────────────────────┐ │ │
│  │  │                    Redis Cache                           │ │ │
│  │  │  ┌────────────┐  ┌────────────┐  ┌────────────┐        │ │ │
│  │  │  │  Session   │  │   Cache    │  │   Queue    │        │ │ │
│  │  │  │   Store    │  │    Data    │  │   Jobs     │        │ │ │
│  │  │  └────────────┘  └────────────┘  └────────────┘        │ │ │
│  │  └──────────────────────────────────────────────────────────┘ │ │
│  └────────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  ┌──────────────────────────────────────────────────────────┐ │ │
│  │  │                File Storage (Optional)                   │ │ │
│  │  │  ┌────────────┐  ┌────────────┐                         │ │ │
│  │  │  │   Images   │  │   Backups  │                         │ │ │
│  │  │  │  (Menu)    │  │    (DB)    │                         │ │ │
│  │  │  └────────────┘  └────────────┘                         │ │ │
│  │  └──────────────────────────────────────────────────────────┘ │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                      EXTERNAL SERVICES                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│  │   Payment    │  │     SMS      │  │    Email     │              │
│  │   Gateway    │  │   Service    │  │   Service    │              │
│  │ (VNPay/Momo) │  │              │  │   (SMTP)     │              │
│  └──────────────┘  └──────────────┘  └──────────────┘              │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2.1.3. Mô tả Chi tiết Từng Tầng

### A. PRESENTATION TIER (Tầng Giao diện)

#### **Vai trò:**
Tầng giao diện chịu trách nhiệm **tương tác trực tiếp với người dùng**, hiển thị dữ liệu và thu thập input từ người dùng.

#### **Công nghệ Sử dụng:**

| Công nghệ | Phiên bản | Mục đích | Lý do lựa chọn |
|-----------|-----------|----------|----------------|
| **React** | 18.2+ | UI Framework | - Component-based architecture<br/>- Virtual DOM cho performance cao<br/>- Hệ sinh thái phong phú<br/>- Dễ bảo trì và test |
| **TypeScript** | 5.0+ | Type Safety | - Phát hiện lỗi sớm<br/>- Autocomplete tốt<br/>- Dễ refactor<br/>- Documentation tự động |
| **React Router** | 6.x | Client-side Routing | - SPA routing<br/>- Lazy loading pages<br/>- Nested routes<br/>- Protected routes |
| **Zustand** hoặc **Redux Toolkit** | Latest | State Management | - Quản lý state global<br/>- Dễ debug<br/>- Middleware support<br/>- DevTools |
| **Ant Design** | 5.x | UI Component Library | - Ready-to-use components<br/>- Responsive design<br/>- Customizable theme<br/>- Vietnamese support |
| **Axios** | 1.x | HTTP Client | - Promise-based<br/>- Interceptors<br/>- Auto JSON transform<br/>- Cancel requests |
| **Socket.io Client** | 4.x | Real-time Communication | - WebSocket support<br/>- Auto reconnection<br/>- Room support<br/>- Fallback transports |
| **React Query** | 4.x | Server State Management | - Caching<br/>- Auto refetch<br/>- Optimistic updates<br/>- Pagination |
| **Recharts** | 2.x | Data Visualization | - React-native charts<br/>- Responsive<br/>- Customizable<br/>- SVG-based |
| **Day.js** | 1.x | Date/Time Handling | - Lightweight (2KB)<br/>- Locale support<br/>- Plugin system<br/>- Timezone support |
| **React Hook Form** | 7.x | Form Management | - Performance (uncontrolled)<br/>- Built-in validation<br/>- Less re-renders<br/>- TypeScript support |
| **Yup** hoặc **Zod** | Latest | Schema Validation | - Type-safe validation<br/>- Reusable schemas<br/>- Error messages<br/>- Integration với RHF |

#### **Cấu trúc Thư mục:**

```
/src
├── /components          # Reusable UI components
│   ├── /common          # Button, Input, Modal, Card...
│   ├── /layout          # Header, Sidebar, Footer
│   └── /features        # Feature-specific components
├── /pages               # Page components (Routes)
│   ├── /dashboard       # Dashboard page
│   ├── /tables          # Table management
│   ├── /menu            # Menu management
│   ├── /orders          # Order management
│   ├── /inventory       # Inventory management
│   ├── /reports         # Reports & analytics
│   └── /settings        # System settings
├── /hooks               # Custom React hooks
├── /store               # State management (Zustand/Redux)
├── /services            # API service layer
│   ├── api.ts           # Axios instance
│   ├── authService.ts   # Authentication APIs
│   ├── orderService.ts  # Order APIs
│   └── ...
├── /utils               # Utility functions
│   ├── validators.ts    # Validation helpers
│   ├── formatters.ts    # Data formatting
│   └── constants.ts     # Constants
├── /types               # TypeScript type definitions
├── /styles              # Global styles, theme
└── /assets              # Images, icons, fonts
```

#### **Chức năng Chính:**

**1. Quản lý Hiển thị (View Management)**
- Render các trang theo route
- Responsive design (Desktop, Tablet, Mobile)
- Theme customization (Light/Dark mode)
- Multi-language support (Vietnamese, English)

**2. Quản lý State (State Management)**
- **Local State**: Component-level state (useState, useReducer)
- **Global State**: App-wide state (Zustand/Redux)
  - User authentication state
  - Current table/order state
  - Notification queue
  - UI preferences
- **Server State**: Data từ API (React Query)
  - Tables, Orders, Menu items
  - Auto caching & refetching
  - Optimistic updates

**3. Giao tiếp với Backend (API Communication)**
- **REST API Calls** (qua Axios)
  - GET: Fetch data
  - POST: Create new records
  - PUT/PATCH: Update records
  - DELETE: Remove records
- **WebSocket Connection** (qua Socket.io)
  - Real-time order updates
  - Table status changes
  - Notifications
  - Kitchen display updates

**4. Xác thực & Phân quyền (Authentication & Authorization)**
- JWT token management (localStorage)
- Auto refresh token
- Protected routes (HOC/Router guards)
- Role-based UI rendering
  - Admin: Full access
  - Manager: Reports, approvals
  - Server: Order creation
  - Chef: Kitchen view
  - Cashier: Payment processing

**5. Form Handling & Validation**
- React Hook Form cho performance
- Yup/Zod schema validation
- Real-time validation feedback
- Error message display

**6. Error Handling**
- Global error boundary
- API error handling (Axios interceptor)
- User-friendly error messages
- Retry mechanism
- Offline detection

---

### B. BUSINESS LOGIC TIER (Tầng Logic Nghiệp vụ)

#### **Vai trò:**
Tầng logic nghiệp vụ chứa **toàn bộ logic xử lý**, quy tắc nghiệp vụ, và điều phối giữa frontend và database.

#### **Công nghệ Sử dụng:**

| Công nghệ | Phiên bản | Mục đích | Lý do lựa chọn |
|-----------|-----------|----------|----------------|
| **Node.js** | 18 LTS+ | Runtime Environment | - JavaScript everywhere<br/>- Non-blocking I/O<br/>- Large ecosystem (npm)<br/>- Good performance |
| **Express.js** | 4.x | Web Framework | - Minimal và flexible<br/>- Middleware support<br/>- Routing<br/>- Large community |
| **TypeScript** | 5.0+ | Type Safety | - Giống frontend<br/>- Shared types<br/>- Better IDE support |
| **Sequelize** | 6.x | ORM (Object-Relational Mapping) | - Support PostgreSQL<br/>- Migration system<br/>- Association handling<br/>- Transaction support |
| **Socket.io** | 4.x | Real-time Server | - Bidirectional communication<br/>- Room/namespace support<br/>- Auto reconnection<br/>- Fallback to polling |
| **jsonwebtoken** | 9.x | JWT Authentication | - Stateless auth<br/>- Token-based<br/>- Standard (RFC 7519) |
| **bcrypt** | 5.x | Password Hashing | - Secure hashing<br/>- Salt generation<br/>- Slow by design<br/>- Industry standard |
| **Joi** hoặc **Zod** | Latest | Input Validation | - Schema validation<br/>- Type inference<br/>- Custom validators |
| **node-cron** | 3.x | Job Scheduling | - Cron syntax<br/>- Timezone support<br/>- No external dependencies |
| **Bull** | 4.x | Job Queue | - Redis-based<br/>- Priority queue<br/>- Delayed jobs<br/>- Retry mechanism |
| **Winston** | 3.x | Logging | - Multiple transports<br/>- Log levels<br/>- Format customization<br/>- Production-ready |
| **Nodemailer** | 6.x | Email Sending | - SMTP support<br/>- Template support<br/>- Attachments<br/>- HTML emails |
| **Axios** | 1.x | HTTP Client (External APIs) | - Payment gateway integration<br/>- SMS API calls |

#### **Cấu trúc Thư mục:**

```
/src
├── /controllers         # Request handlers (Routes)
│   ├── authController.ts
│   ├── tableController.ts
│   ├── orderController.ts
│   ├── menuController.ts
│   └── ...
├── /services            # Business logic layer
│   ├── orderService.ts       # Order processing logic
│   ├── inventoryService.ts   # Inventory auto-deduction
│   ├── paymentService.ts     # Payment processing
│   ├── notificationService.ts
│   └── ...
├── /repositories        # Data access layer
│   ├── orderRepository.ts
│   ├── tableRepository.ts
│   └── ...
├── /models              # Sequelize models
│   ├── User.ts
│   ├── Table.ts
│   ├── Order.ts
│   ├── MenuItem.ts
│   ├── Ingredient.ts
│   └── ...
├── /middleware          # Express middleware
│   ├── authMiddleware.ts     # JWT verification
│   ├── rbacMiddleware.ts     # Role-based access control
│   ├── validationMiddleware.ts
│   ├── errorMiddleware.ts
│   └── loggerMiddleware.ts
├── /routes              # Route definitions
│   ├── authRoutes.ts
│   ├── tableRoutes.ts
│   └── ...
├── /config              # Configuration files
│   ├── database.ts
│   ├── redis.ts
│   ├── jwt.ts
│   └── app.ts
├── /utils               # Utility functions
│   ├── validators.ts
│   ├── formatters.ts
│   ├── errors.ts        # Custom error classes
│   └── constants.ts
├── /jobs                # Cron jobs & Queue workers
│   ├── dailyReportJob.ts
│   ├── inventoryCheckJob.ts
│   └── emailQueue.ts
├── /websocket           # WebSocket handlers
│   ├── orderEvents.ts
│   ├── notificationEvents.ts
│   └── tableEvents.ts
└── /types               # TypeScript type definitions
```

#### **Kiến trúc Layer:**

```
┌─────────────────────────────────────────────────────────┐
│                    Controllers Layer                     │
│  - Nhận HTTP request                                     │
│  - Validate input (với Joi/Zod)                         │
│  - Gọi Service layer                                     │
│  - Trả response (JSON)                                   │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│                    Services Layer                        │
│  - Business logic chính                                  │
│  - Orchestrate nhiều repositories                        │
│  - Transaction management                                │
│  - Áp dụng business rules                               │
│  - Gọi external services (Payment, SMS, Email)          │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│                  Repositories Layer                      │
│  - Data access logic                                     │
│  - CRUD operations với database                          │
│  - Query building (Sequelize)                           │
│  - Không có business logic                              │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│                     Models Layer                         │
│  - Sequelize models                                      │
│  - Schema definition                                     │
│  - Associations (hasMany, belongsTo...)                 │
│  - Hooks (beforeCreate, afterUpdate...)                 │
└─────────────────────────────────────────────────────────┘
```

#### **Luồng xử lý Request điển hình:**

```
Client Request → Express Router
                     ↓
              Middleware Stack
              ├─ Logger Middleware
              ├─ Auth Middleware (JWT verify)
              ├─ RBAC Middleware (role check)
              └─ Validation Middleware (Joi/Zod)
                     ↓
                 Controller
              (Parse request, call service)
                     ↓
                  Service
         (Business logic, transaction)
         ├─ Call Repository 1
         ├─ Call Repository 2
         ├─ Call External API (optional)
         └─ Emit WebSocket event (optional)
                     ↓
                 Repository
         (Query database via Sequelize)
                     ↓
               Database (PostgreSQL)
                     ↓
         ◄──── Response flow ─────
                     ↓
              Controller formats response
                     ↓
                Client receives JSON
```

#### **Chức năng Chính:**

**1. API Endpoints (RESTful)**

**A. Authentication APIs**
```
POST   /api/auth/register      # Đăng ký tài khoản
POST   /api/auth/login         # Đăng nhập
POST   /api/auth/logout        # Đăng xuất
POST   /api/auth/refresh       # Refresh JWT token
GET    /api/auth/me            # Lấy thông tin user hiện tại
```

**B. Table Management APIs**
```
GET    /api/tables             # Danh sách bàn
GET    /api/tables/:id         # Chi tiết bàn
POST   /api/tables             # Tạo bàn mới
PUT    /api/tables/:id         # Cập nhật bàn
DELETE /api/tables/:id         # Xóa bàn
PATCH  /api/tables/:id/status  # Cập nhật trạng thái bàn
```

**C. Order Management APIs**
```
GET    /api/orders             # Danh sách đơn hàng
GET    /api/orders/:id         # Chi tiết đơn hàng
POST   /api/orders             # Tạo đơn hàng mới
PUT    /api/orders/:id         # Cập nhật đơn hàng
DELETE /api/orders/:id         # Hủy đơn hàng
PATCH  /api/orders/:id/status  # Cập nhật trạng thái đơn
POST   /api/orders/:id/items   # Thêm món vào đơn
DELETE /api/orders/:id/items/:itemId  # Xóa món khỏi đơn
```

**D. Menu Management APIs**
```
GET    /api/menu               # Danh sách món
GET    /api/menu/:id           # Chi tiết món
POST   /api/menu               # Tạo món mới
PUT    /api/menu/:id           # Cập nhật món
DELETE /api/menu/:id           # Xóa món
PATCH  /api/menu/:id/status    # Đổi trạng thái món (available/unavailable)
POST   /api/menu/:id/image     # Upload ảnh món
```

**E. Inventory APIs**
```
GET    /api/inventory          # Danh sách nguyên liệu
GET    /api/inventory/:id      # Chi tiết nguyên liệu
POST   /api/inventory          # Thêm nguyên liệu
PUT    /api/inventory/:id      # Cập nhật nguyên liệu
POST   /api/inventory/import   # Nhập kho
POST   /api/inventory/export   # Xuất kho
GET    /api/inventory/alerts   # Cảnh báo hết hàng
```

**F. Payment APIs**
```
POST   /api/payments           # Thanh toán
GET    /api/payments/:id       # Chi tiết thanh toán
POST   /api/payments/vnpay     # Tích hợp VNPay
POST   /api/payments/momo      # Tích hợp Momo
```

**G. Report APIs**
```
GET    /api/reports/revenue    # Báo cáo doanh thu
GET    /api/reports/inventory  # Báo cáo tồn kho
GET    /api/reports/menu       # Báo cáo món bán chạy
GET    /api/reports/staff      # Báo cáo hiệu suất nhân viên
GET    /api/reports/dashboard  # Dashboard tổng quan
```

**2. WebSocket Events (Real-time)**

**Server → Client Events:**
```javascript
// Order events
'order:created'       // Đơn mới được tạo
'order:updated'       // Đơn được cập nhật
'order:status_changed' // Trạng thái đơn thay đổi
'order:cancelled'     // Đơn bị hủy

// Table events
'table:status_changed' // Trạng thái bàn thay đổi
'table:assigned'      // Bàn được gán cho khách

// Inventory events
'inventory:low_stock' // Cảnh báo hết hàng
'inventory:updated'   // Nguyên liệu được cập nhật

// Notification events
'notification:new'    // Thông báo mới
```

**Client → Server Events:**
```javascript
// Kitchen events
'kitchen:order_ready' // Bếp báo món đã xong
'kitchen:order_start' // Bếp bắt đầu làm món

// Server events
'server:order_served' // Phục vụ xác nhận đã mang món ra
```

**3. Business Logic Processing**

**A. Xử lý Đơn hàng**
```typescript
async createOrder(orderData) {
  // 1. Validate input
  // 2. Check table availability
  // 3. Check menu item availability
  // 4. Check ingredient stock
  // 5. Begin transaction
  //    - Create order record
  //    - Create order_details records
  //    - Deduct ingredient stock
  //    - Update table status
  // 6. Commit transaction
  // 7. Emit WebSocket event
  // 8. Return order
}
```

**B. Tự động Trừ Nguyên liệu**
```typescript
async deductIngredients(orderItems) {
  for (const item of orderItems) {
    // 1. Get recipe (item_ingredients)
    const recipe = await getRecipe(item.menuItemId);
    
    // 2. Calculate total ingredients needed
    const ingredientsNeeded = recipe.map(r => ({
      ingredientId: r.ingredientId,
      quantity: r.quantity * item.quantity
    }));
    
    // 3. Check stock availability
    for (const ing of ingredientsNeeded) {
      const stock = await getStock(ing.ingredientId);
      if (stock < ing.quantity) {
        throw new Error('Insufficient stock');
      }
    }
    
    // 4. Deduct stock
    await updateStock(ingredientsNeeded);
    
    // 5. Log transaction
    await createInventoryTransaction({
      type: 'EXPORT',
      orderId: order.id,
      items: ingredientsNeeded
    });
    
    // 6. Check low stock alert
    await checkLowStockAlerts();
  }
}
```

**C. Xử lý Thanh toán**
```typescript
async processPayment(orderId, paymentData) {
  // 1. Get order details
  const order = await getOrderWithDetails(orderId);
  
  // 2. Calculate total
  const subtotal = order.items.reduce((sum, item) => 
    sum + (item.price * item.quantity), 0
  );
  const tax = subtotal * 0.08; // VAT 8%
  const discount = await applyDiscount(order.customerId, paymentData.voucherCode);
  const total = subtotal + tax - discount;
  
  // 3. Process payment based on method
  let paymentResult;
  switch(paymentData.method) {
    case 'CASH':
      paymentResult = { success: true };
      break;
    case 'CARD':
    case 'VNPAY':
    case 'MOMO':
      paymentResult = await processOnlinePayment(paymentData);
      break;
  }
  
  // 4. Begin transaction
  if (paymentResult.success) {
    await transaction(async (t) => {
      // Update order status
      await updateOrderStatus(orderId, 'COMPLETED', t);
      
      // Create payment record
      await createPayment({
        orderId,
        amount: total,
        method: paymentData.method,
        status: 'COMPLETED'
      }, t);
      
      // Update table status
      await updateTableStatus(order.tableId, 'AVAILABLE', t);
      
      // Log audit
      await createAuditLog({
        action: 'PAYMENT_COMPLETED',
        orderId,
        amount: total
      }, t);
    });
    
    // 5. Generate invoice
    const invoice = await generateInvoice(orderId);
    
    // 6. Send email/SMS
    await sendInvoiceEmail(order.customer.email, invoice);
    
    // 7. Emit WebSocket event
    io.emit('order:completed', { orderId });
    
    return { success: true, invoice };
  }
}
```

**D. Scheduled Jobs (Cron)**
```typescript
// 1. Daily Report Job (Every day at 23:00)
cron.schedule('0 23 * * *', async () => {
  const report = await generateDailyReport();
  await sendEmailToManagers(report);
});

// 2. Inventory Check Job (Every 2 hours)
cron.schedule('0 */2 * * *', async () => {
  const lowStockItems = await checkLowStock();
  if (lowStockItems.length > 0) {
    await notifyManagers('LOW_STOCK', lowStockItems);
  }
});

// 3. Expired Reservation Cleanup (Every 15 minutes)
cron.schedule('*/15 * * * *', async () => {
  const expiredReservations = await findExpiredReservations();
  for (const res of expiredReservations) {
    await cancelReservation(res.id, 'AUTO_EXPIRED');
    await updateTableStatus(res.tableId, 'AVAILABLE');
  }
});

// 4. Backup Database (Every day at 02:00)
cron.schedule('0 2 * * *', async () => {
  await backupDatabase();
  await cleanOldBackups(30); // Keep 30 days
});
```

**4. Middleware Chain**

**A. Authentication Middleware**
```typescript
export const authMiddleware = async (req, res, next) => {
  try {
    // 1. Get token from header
    const token = req.headers.authorization?.split(' ')[1];
    
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    // 2. Verify token
    const decoded = jwt.verify(token, JWT_SECRET);
    
    // 3. Check if user still exists
    const user = await User.findByPk(decoded.userId);
    if (!user) {
      return res.status(401).json({ error: 'User not found' });
    }
    
    // 4. Attach user to request
    req.user = user;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid token' });
  }
};
```

**B. Role-Based Access Control Middleware**
```typescript
export const rbacMiddleware = (allowedRoles: string[]) => {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Unauthorized' });
    }
    
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ 
        error: 'Forbidden: Insufficient permissions' 
      });
    }
    
    next();
  };
};

// Usage:
app.get('/api/reports/revenue', 
  authMiddleware,
  rbacMiddleware(['ADMIN', 'MANAGER']),
  getRevenueReport
);
```

**C. Validation Middleware**
```typescript
export const validateRequest = (schema: Joi.Schema) => {
  return (req, res, next) => {
    const { error, value } = schema.validate(req.body);
    
    if (error) {
      return res.status(400).json({
        error: 'Validation failed',
        details: error.details.map(d => d.message)
      });
    }
    
    req.body = value; // Use validated & sanitized value
    next();
  };
};
```

**D. Error Handling Middleware**
```typescript
export const errorMiddleware = (err, req, res, next) => {
  logger.error(err);
  
  // Sequelize validation error
  if (err.name === 'SequelizeValidationError') {
    return res.status(400).json({
      error: 'Validation error',
      details: err.errors.map(e => e.message)
    });
  }
  
  // JWT error
  if (err.name === 'JsonWebTokenError') {
    return res.status(401).json({ error: 'Invalid token' });
  }
  
  // Custom business error
  if (err.isOperational) {
    return res.status(err.statusCode).json({
      error: err.message
    });
  }
  
  // Unknown error
  return res.status(500).json({
    error: 'Internal server error'
  });
};
```

**5. Transaction Management**
```typescript
// Example: Create order with inventory deduction
async createOrderWithInventory(orderData) {
  return await sequelize.transaction(async (t) => {
    // 1. Create order
    const order = await Order.create(orderData, { transaction: t });
    
    // 2. Create order details
    for (const item of orderData.items) {
      await OrderDetail.create({
        orderId: order.id,
        menuItemId: item.menuItemId,
        quantity: item.quantity,
        price: item.price
      }, { transaction: t });
      
      // 3. Deduct ingredients
      const recipe = await ItemIngredient.findAll({
        where: { menuItemId: item.menuItemId }
      });
      
      for (const ing of recipe) {
        const totalNeeded = ing.quantity * item.quantity;
        
        // Check stock
        const ingredient = await Ingredient.findByPk(
          ing.ingredientId, 
          { transaction: t, lock: true }
        );
        
        if (ingredient.stockQuantity < totalNeeded) {
          throw new Error(`Insufficient stock for ${ingredient.name}`);
        }
        
        // Deduct
        await ingredient.update({
          stockQuantity: ingredient.stockQuantity - totalNeeded
        }, { transaction: t });
        
        // Log transaction
        await InventoryTransaction.create({
          ingredientId: ing.ingredientId,
          quantity: -totalNeeded,
          type: 'EXPORT',
          orderId: order.id
        }, { transaction: t });
      }
    }
    
    // 4. Update table status
    await Table.update(
      { status: 'OCCUPIED' },
      { where: { id: orderData.tableId }, transaction: t }
    );
    
    return order;
  });
}
```

---

### C. DATA TIER (Tầng Dữ liệu)

#### **Vai trò:**
Tầng dữ liệu chịu trách nhiệm **lưu trữ và quản lý dữ liệu** của hệ thống, đảm bảo tính toàn vẹn, nhất quán và hiệu năng truy vấn.

#### **Công nghệ Sử dụng:**

| Công nghệ | Phiên bản | Mục đích | Lý do lựa chọn |
|-----------|-----------|----------|----------------|
| **PostgreSQL** | 13+ | Relational Database | - ACID compliance<br/>- Complex queries support<br/>- JSON support<br/>- Full-text search<br/>- Triggers & Stored Procedures<br/>- Strong data integrity |
| **Redis** | 6+ | In-memory Cache & Queue | - Extremely fast (in-memory)<br/>- Session storage<br/>- Cache layer<br/>- Pub/Sub for WebSocket<br/>- Job queue (Bull) |
| **File System** hoặc **S3** | - | File Storage | - Menu item images<br/>- Database backups<br/>- Invoice PDFs |

#### **PostgreSQL - Cấu trúc Database**

**A. Nhóm Table theo Module:**

```
┌─────────────────────────────────────────────────────────┐
│                Authentication & User Management          │
├─────────────────────────────────────────────────────────┤
│  - employees                                             │
│  - roles                                                 │
│  - permissions                                           │
│  - role_permissions                                      │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                  Customer Management                     │
├─────────────────────────────────────────────────────────┤
│  - customers                                             │
│  - customer_types (VIP, Regular, Enterprise)            │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                   Table Management                       │
├─────────────────────────────────────────────────────────┤
│  - tables                                                │
│  - areas (Tầng 1, Tầng 2, VIP...)                       │
│  - reservations                                          │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                    Menu Management                       │
├─────────────────────────────────────────────────────────┤
│  - categories (Khai vị, Món chính...)                   │
│  - menu_items                                            │
│  - menu_item_images                                      │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                 Inventory Management                     │
├─────────────────────────────────────────────────────────┤
│  - ingredients                                           │
│  - item_ingredients (Recipe - định lượng)               │
│  - inventory_transactions (Nhập/Xuất kho)               │
│  - suppliers                                             │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                   Order Management                       │
├─────────────────────────────────────────────────────────┤
│  - orders                                                │
│  - order_details                                         │
│  - order_status_history                                  │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                  Payment Management                      │
├─────────────────────────────────────────────────────────┤
│  - payments                                              │
│  - payment_methods                                       │
│  - invoices                                              │
│  - discounts (Voucher/Promo)                            │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                System & Audit                            │
├─────────────────────────────────────────────────────────┤
│  - audit_logs                                            │
│  - notifications                                         │
│  - system_settings                                       │
└─────────────────────────────────────────────────────────┘
```

**B. Các Index quan trọng:**

```sql
-- Performance indexes
CREATE INDEX idx_orders_table_id ON orders(table_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at);
CREATE INDEX idx_order_details_order_id ON order_details(order_id);
CREATE INDEX idx_order_details_menu_item_id ON order_details(menu_item_id);
CREATE INDEX idx_inventory_transactions_ingredient_id 
  ON inventory_transactions(ingredient_id);
CREATE INDEX idx_inventory_transactions_created_at 
  ON inventory_transactions(created_at);
CREATE INDEX idx_customers_phone ON customers(phone);
CREATE INDEX idx_customers_email ON customers(email);

-- Composite indexes
CREATE INDEX idx_orders_status_created 
  ON orders(status, created_at DESC);
CREATE INDEX idx_reservations_date_status 
  ON reservations(reservation_date, status);
```

**C. Triggers quan trọng:**

```sql
-- 1. Auto update updated_at timestamp
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$ LANGUAGE plpgsql;

CREATE TRIGGER update_orders_updated_at
  BEFORE UPDATE ON orders
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

-- 2. Validate inventory before order
CREATE OR REPLACE FUNCTION check_ingredient_stock()
RETURNS TRIGGER AS $
DECLARE
  ingredient_id UUID;
  required_qty DECIMAL;
  available_qty DECIMAL;
BEGIN
  -- Get ingredients for this menu item
  FOR ingredient_id, required_qty IN
    SELECT ii.ingredient_id, ii.quantity * NEW.quantity
    FROM item_ingredients ii
    WHERE ii.menu_item_id = NEW.menu_item_id
  LOOP
    SELECT stock_quantity INTO available_qty
    FROM ingredients
    WHERE id = ingredient_id;
    
    IF available_qty < required_qty THEN
      RAISE EXCEPTION 'Insufficient stock for ingredient %', ingredient_id;
    END IF;
  END LOOP;
  
  RETURN NEW;
END;
$ LANGUAGE plpgsql;

CREATE TRIGGER validate_order_detail_stock
  BEFORE INSERT ON order_details
  FOR EACH ROW
  EXECUTE FUNCTION check_ingredient_stock();

-- 3. Log order status changes
CREATE OR REPLACE FUNCTION log_order_status_change()
RETURNS TRIGGER AS $
BEGIN
  IF NEW.status <> OLD.status THEN
    INSERT INTO order_status_history (
      order_id, old_status, new_status, changed_by
    ) VALUES (
      NEW.id, OLD.status, NEW.status, NEW.updated_by
    );
  END IF;
  RETURN NEW;
END;
$ LANGUAGE plpgsql;

CREATE TRIGGER track_order_status
  AFTER UPDATE ON orders
  FOR EACH ROW
  WHEN (OLD.status IS DISTINCT FROM NEW.status)
  EXECUTE FUNCTION log_order_status_change();
```

**D. Views hữu ích:**

```sql
-- 1. View: Current table status
CREATE OR REPLACE VIEW v_table_status AS
SELECT 
  t.id,
  t.table_number,
  t.area,
  t.capacity,
  t.status,
  o.id AS current_order_id,
  o.created_at AS order_start_time,
  EXTRACT(EPOCH FROM (NOW() - o.created_at))/60 AS minutes_occupied
FROM tables t
LEFT JOIN orders o ON t.id = o.table_id 
  AND o.status IN ('PREPARING', 'READY', 'SERVING');

-- 2. View: Daily revenue
CREATE OR REPLACE VIEW v_daily_revenue AS
SELECT 
  DATE(p.created_at) AS date,
  COUNT(DISTINCT o.id) AS total_orders,
  SUM(p.amount) AS total_revenue,
  AVG(p.amount) AS avg_order_value,
  SUM(CASE WHEN p.method = 'CASH' THEN p.amount ELSE 0 END) AS cash_revenue,
  SUM(CASE WHEN p.method IN ('CARD', 'VNPAY', 'MOMO') 
    THEN p.amount ELSE 0 END) AS online_revenue
FROM payments p
JOIN orders o ON p.order_id = o.id
WHERE p.status = 'COMPLETED'
GROUP BY DATE(p.created_at);

-- 3. View: Low stock items
CREATE OR REPLACE VIEW v_low_stock_items AS
SELECT 
  id,
  name,
  stock_quantity,
  unit,
  alert_threshold,
  (alert_threshold - stock_quantity) AS deficit
FROM ingredients
WHERE stock_quantity < alert_threshold
ORDER BY deficit DESC;

-- 4. View: Popular menu items
CREATE OR REPLACE VIEW v_popular_menu_items AS
SELECT 
  mi.id,
  mi.name,
  mi.category,
  COUNT(od.id) AS times_ordered,
  SUM(od.quantity) AS total_quantity,
  SUM(od.quantity * od.price) AS total_revenue,
  AVG(od.price) AS avg_price
FROM menu_items mi
JOIN order_details od ON mi.id = od.menu_item_id
JOIN orders o ON od.order_id = o.id
WHERE o.status = 'COMPLETED'
  AND o.created_at >= NOW() - INTERVAL '30 days'
GROUP BY mi.id, mi.name, mi.category
ORDER BY times_ordered DESC;
```

**E. Stored Procedures:**

```sql
-- 1. Calculate order total
CREATE OR REPLACE FUNCTION calculate_order_total(
  p_order_id UUID,
  p_discount_code VARCHAR DEFAULT NULL
)
RETURNS TABLE(
  subtotal DECIMAL,
  tax DECIMAL,
  discount DECIMAL,
  total DECIMAL
) AS $
DECLARE
  v_subtotal DECIMAL;
  v_tax DECIMAL := 0.08;
  v_discount DECIMAL := 0;
  v_discount_percent DECIMAL;
BEGIN
  -- Calculate subtotal
  SELECT COALESCE(SUM(quantity * price), 0) INTO v_subtotal
  FROM order_details
  WHERE order_id = p_order_id;
  
  -- Calculate tax
  v_tax := v_subtotal * v_tax;
  
  -- Apply discount
  IF p_discount_code IS NOT NULL THEN
    SELECT discount_percent INTO v_discount_percent
    FROM discounts
    WHERE code = p_discount_code
      AND valid_from <= NOW()
      AND valid_to >= NOW()
      AND is_active = true;
    
    IF FOUND THEN
      v_discount := v_subtotal * v_discount_percent / 100;
    END IF;
  END IF;
  
  RETURN QUERY SELECT 
    v_subtotal,
    v_tax,
    v_discount,
    v_subtotal + v_tax - v_discount AS total;
END;
$ LANGUAGE plpgsql;

-- Usage:
-- SELECT * FROM calculate_order_total('uuid-here', 'WELCOME10');
```

#### **Redis - Caching Strategy**

**A. Cache Keys Structure:**

```
# Session storage
session:{userId}           → User session data
session:{sessionId}        → JWT token data

# Cache layer
cache:tables               → List of all tables
cache:tables:{id}          → Specific table details
cache:menu                 → Menu items list
cache:menu:{id}            → Specific menu item
cache:categories           → Categories list
cache:ingredients          → Ingredients list

# Real-time data
realtime:orders:active     → List of active order IDs
realtime:tables:occupied   → List of occupied table IDs
realtime:notifications     → Notification queue

# Job queue (Bull)
bull:email:*               → Email jobs
bull:notification:*        → Notification jobs
bull:report:*              → Report generation jobs
```

**B. Cache Invalidation Strategy:**

```typescript
// When updating menu item
async updateMenuItem(id, data) {
  // 1. Update database
  const updated = await MenuItem.update(data, { where: { id } });
  
  // 2. Invalidate cache
  await redis.del(`cache:menu:${id}`);
  await redis.del('cache:menu');
  
  // 3. Emit event
  io.emit('menu:updated', { id });
  
  return updated;
}

// Auto-refresh cache with TTL
await redis.setex(
  'cache:menu',
  3600, // 1 hour
  JSON.stringify(menuItems)
);
```

**C. Pub/Sub for WebSocket:**

```typescript
// Publisher (in order service)
await redis.publish('order:created', JSON.stringify({
  orderId: order.id,
  tableId: order.tableId,
  status: 'PREPARING'
}));

// Subscriber (in WebSocket server)
redis.subscribe('order:created');
redis.on('message', (channel, message) => {
  const data = JSON.parse(message);
  io.to(`table:${data.tableId}`).emit('order:created', data);
});
```

---

## 2.1.4. Giao tiếp Giữa Các Tầng

### A. Request/Response Flow

```
┌──────────────────────────────────────────────────────────────┐
│                   1. USER ACTION                              │
│  User clicks "Tạo đơn hàng" button                           │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│              2. PRESENTATION TIER                             │
│  - Validate form inputs (React Hook Form + Yup)              │
│  - Show loading spinner                                       │
│  - Call API: POST /api/orders                                │
└────────────────────────┬─────────────────────────────────────┘
                         │
                      HTTP POST
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          3. BUSINESS LOGIC TIER - Express Router             │
│  POST /api/orders                                            │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          4. MIDDLEWARE CHAIN                                  │
│  a) loggerMiddleware     → Log request                       │
│  b) authMiddleware       → Verify JWT token                  │
│  c) rbacMiddleware       → Check role (SERVER, ADMIN)        │
│  d) validateRequest      → Validate body with Joi            │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          5. CONTROLLER                                        │
│  orderController.createOrder()                               │
│  - Parse request body                                         │
│  - Call orderService.createOrder()                           │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          6. SERVICE LAYER                                     │
│  orderService.createOrder()                                  │
│  - Begin transaction                                          │
│  - Validate table availability                               │
│  - Check menu item availability                              │
│  - Calculate total price                                      │
│  - Call orderRepository.create()                             │
│  - Call inventoryService.deductIngredients()                 │
│  - Commit transaction                                         │
│  - Emit WebSocket event                                       │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          7. REPOSITORY LAYER                                  │
│  orderRepository.create()                                    │
│  - Build Sequelize query                                      │
│  - Execute INSERT statements                                  │
└────────────────────────┬─────────────────────────────────────┘
                         │
                      SQL Query
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          8. DATA TIER - PostgreSQL                            │
│  - Execute INSERT INTO orders...                             │
│  - Execute INSERT INTO order_details...                      │
│  - Execute UPDATE ingredients SET stock_quantity...          │
│  - Execute INSERT INTO inventory_transactions...             │
│  - Return inserted records                                    │
└────────────────────────┬─────────────────────────────────────┘
                         │
               ◄───── Response Flow ─────
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          9. REPOSITORY returns data to SERVICE                │
└────────────────────────┬─────────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          10. SERVICE formats data, emits WebSocket            │
│  io.emit('order:created', orderData)                         │
└────────────────────────┬─────────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          11. CONTROLLER returns JSON response                 │
│  res.status(201).json({ success: true, order })              │
└────────────────────────┬─────────────────────────────────────┘
                         │
                      HTTP 201
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          12. PRESENTATION TIER receives response              │
│  - Hide loading spinner                                       │
│  - Update local state (Redux/Zustand)                        │
│  - Show success notification                                  │
│  - Navigate to order detail page (optional)                  │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          13. USER SEES RESULT                                 │
│  "Đơn hàng #12345 đã được tạo thành công!"                  │
└──────────────────────────────────────────────────────────────┘
```

### B. WebSocket Real-time Flow

```
┌──────────────────────────────────────────────────────────────┐
│          KITCHEN DISPLAY (Chef Interface)                     │
│  WebSocket Client connected to room "kitchen"                │
└──────────────────────────┬───────────────────────────────────┘
                           │
                        Listening...
                           │
┌──────────────────────────▼───────────────────────────────────┐
│          BUSINESS LOGIC TIER - WebSocket Server              │
│  Socket.io server running on port 3001                       │
└────────────────────────┬─────────────────────────────────────┘
                         │
              Emit: 'order:created'
                         │
┌────────────────────────▼─────────────────────────────────────┐
│          Redis Pub/Sub (Optional)                            │
│  Distribute events across multiple server instances          │
└────────────────────────┬─────────────────────────────────────┘
                         │
                         ▼
┌──────────────────────────────────────────────────────────────┐
│          ALL CONNECTED CLIENTS IN "kitchen" ROOM             │
│  - Kitchen display screen                                     │
│  - Manager dashboard                                          │
│  - Mobile app for chef                                        │
└──────────────────────────────────────────────────────────────┘
```

---

## 2.1.5. Technology Stack Summary

### Frontend Stack
```yaml
Core:
  - React 18.2+
  - TypeScript 5.0+
  - Vite (Build tool)

UI/UX:
  - Ant Design 5.x
  - Tailwind CSS (optional for custom styling)
  - Recharts (Data visualization)
  - React Icons / Lucide Icons

State Management:
  - Zustand or Redux Toolkit
  - React Query (Server state)

Routing:
  - React Router 6.x

Forms:
  - React Hook Form
  - Yup or Zod (Validation)

HTTP & Real-time:
  - Axios
  - Socket.io Client

Utilities:
  - Day.js (Date manipulation)
  - Lodash (Utility functions)
```

### Backend Stack
```yaml
Core:
  - Node.js 18 LTS+
  - Express.js 4.x
  - TypeScript 5.0+

Database:
  - PostgreSQL 13+
  - Sequelize 6.x (ORM)
  - Redis 6+ (Cache & Queue)

Authentication:
  - jsonwebtoken (JWT)
  - bcrypt (Password hashing)

Validation:
  - Joi or Zod

Real-time:
  - Socket.io 4.x

Jobs:
  - node-cron (Scheduling)
  - Bull (Queue)

Logging:
  - Winston

Email:
  - Nodemailer

Testing:
  - Jest (Unit tests)
  - Supertest (API tests)
```

### DevOps Stack
```yaml
Version Control:
  - Git
  - GitHub/GitLab

CI/CD:
  - GitHub Actions or GitLab CI

Containerization:
  - Docker
  - Docker Compose

Deployment:
  - AWS EC2 or DigitalOcean Droplet
  - Nginx (Reverse proxy)
  - PM2 (Process manager)

Monitoring:
  - PM2 monitoring
  - PostgreSQL slow query log
  - Application logs (Winston)
```

---

## 2.1.6. Deployment Architecture

```
                        ┌─────────────────────┐
                        │   USERS / CLIENTS   │
                        └──────────┬──────────┘
                                   │
                            HTTPS (443)
                                   │
                        ┌──────────▼──────────┐
                        │   NGINX (Web Server) │
                        │   - SSL Termination  │
                        │   - Load Balancer    │
                        │   - Reverse Proxy    │
                        └──────────┬──────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    │                             │
            HTTP (3000)                    WS (3001)
                    │                             │
         ┌──────────▼──────────┐      ┌──────────▼──────────┐
         │   React App         │      │  Socket.io Server   │
         │   (Static Files)    │      │  (WebSocket)        │
         │   Served by Nginx   │      │  Node.js            │
         └─────────────────────┘      └──────────┬──────────┘
                                                  │
                                                  │
                              ┌───────────────────▼───────────────────┐
                              │   Express.js API Server (Port 4000)   │
                              │   - Business Logic                     │
                              │   - Authentication                     │
                              │   - API Endpoints                      │
                              └───────────┬───────────────────────────┘
                                          │
                          ┌───────────────┼───────────────┐
                          │               │               │
                   ┌──────▼──────┐  ┌────▼─────┐  ┌──────▼──────┐
                   │ PostgreSQL  │  │  Redis   │  │ File Storage│
                   │   (5432)    │  │  (6379)  │  │   (Local/S3)│
                   │  - Database │  │  - Cache │  │  - Images   │
                   │  - Backup   │  │  - Queue │  │  - Backups  │
                   └─────────────┘  └──────────┘  └─────────────┘
```

---

## 2.1.7. Ưu điểm của Kiến trúc 3 Tầng

### A. Tách biệt Rõ ràng (Separation of Concerns)

| Tầng | Trách nhiệm | Lợi ích |
|------|-------------|---------|
| **Presentation** | Chỉ lo hiển thị & tương tác | - UI có thể thay đổi mà không ảnh hưởng logic<br/>- Dễ test UI riêng biệt<br/>- Có thể có nhiều UI (Web, Mobile) dùng chung Backend |
| **Business Logic** | Chỉ lo xử lý nghiệp vụ | - Logic nghiệp vụ tập trung một chỗ<br/>- Tái sử dụng được<br/>- Dễ test business logic |
| **Data** | Chỉ lo lưu trữ & truy vấn | - Database có thể thay đổi (PostgreSQL → MySQL)<br/>- Optimization độc lập<br/>- Backup/restore dễ dàng |

### B. Khả năng Mở rộng (Scalability)

**Horizontal Scaling:**
```
                    ┌──────────────┐
                    │ Load Balancer│
                    └──────┬───────┘
                           │
            ┌──────────────┼──────────────┐
            │              │              │
    ┌───────▼──────┐ ┌────▼─────┐ ┌──────▼──────┐
    │ API Server 1 │ │API Server│ │ API Server 3│
    └──────────────┘ └──────────┘ └─────────────┘
            │              │              │
            └──────────────┼──────────────┘
                           │
                    ┌──────▼───────┐
                    │  PostgreSQL  │
                    │  (Master)    │
                    └──────┬───────┘
                           │
            ┌──────────────┼──────────────┐
            │              │              │
    ┌───────▼──────┐ ┌────▼─────┐ ┌──────▼──────┐
    │  Read Replica│ │   Read   │ │ Read Replica│
    │      1       │ │  Replica │ │      3      │
    └──────────────┘ └──────────┘ └─────────────┘
```

**Vertical Scaling:**
- Tăng RAM, CPU cho từng tầng độc lập
- Database có thể nâng cấp server riêng
- Cache (Redis) có thể scale riêng

### C. Bảo mật Tốt hơn (Enhanced Security)

**1. Network Isolation:**
```
┌─────────────────────────────────────────────────────────┐
│  DMZ (Demilitarized Zone)                               │
│  ┌──────────────┐                                       │
│  │  Web Server  │  Public-facing                        │
│  │   (Nginx)    │  Exposed to internet                  │
│  └──────┬───────┘                                       │
└─────────┼───────────────────────────────────────────────┘
          │
          │ Firewall
          │
┌─────────▼───────────────────────────────────────────────┐
│  Application Tier (Private Network)                     │
│  ┌──────────────┐                                       │
│  │  API Server  │  Not directly accessible              │
│  │  (Express)   │  from internet                        │
│  └──────┬───────┘                                       │
└─────────┼───────────────────────────────────────────────┘
          │
          │ Firewall
          │
┌─────────▼───────────────────────────────────────────────┐
│  Data Tier (Isolated Network)                           │
│  ┌──────────────┐                                       │
│  │  Database    │  Only accessible from                 │
│  │ (PostgreSQL) │  Application Tier                     │
│  └──────────────┘                                       │
└─────────────────────────────────────────────────────────┘
```

**2. Role-based Access:**
- Frontend: User sees only what their role allows
- Backend: API endpoints protected by RBAC middleware
- Database: Row-level security policies

### D. Dễ Bảo trì (Maintainability)

**1. Independent Updates:**
```
Frontend Update:
  → Deploy new React build
  → Backend không cần thay đổi
  → Database không ảnh hưởng

Backend Update:
  → Deploy new Node.js version
  → Frontend chỉ cần update API calls (nếu có)
  → Database không ảnh hưởng

Database Update:
  → Chạy migration scripts
  → Backend có thể cần update models
  → Frontend không cần thay đổi
```

**2. Clear Code Organization:**
```
# Frontend Developer focus:
/frontend
  /src
    /components
    /pages
    /services  ← Chỉ cần biết API endpoints

# Backend Developer focus:
/backend
  /src
    /controllers
    /services   ← Business logic ở đây
    /repositories
    /models

# Database Developer focus:
/database
  /migrations
  /seeds
  /schemas
```

### E. Dễ Test (Testability)

**1. Unit Testing:**
```typescript
// Test Presentation tier
describe('OrderForm Component', () => {
  it('should validate form inputs', () => {
    // Test UI logic only
  });
});

// Test Business Logic tier
describe('OrderService', () => {
  it('should calculate order total correctly', () => {
    // Mock repository
    // Test business logic only
  });
});

// Test Data tier
describe('OrderRepository', () => {
  it('should create order record', () => {
    // Test database operations only
  });
});
```

**2. Integration Testing:**
```typescript
// Test API endpoints
describe('POST /api/orders', () => {
  it('should create order successfully', async () => {
    const response = await request(app)
      .post('/api/orders')
      .send(orderData)
      .expect(201);
    
    expect(response.body.order).toBeDefined();
  });
});
```

---

## 2.1.8. Xử lý Lỗi & Rollback

### A. Error Handling Strategy

**1. Frontend Error Handling:**
```typescript
// Global error boundary
class ErrorBoundary extends React.Component {
  componentDidCatch(error, errorInfo) {
    // Log to error tracking service
    logErrorToService(error, errorInfo);
    
    // Show user-friendly message
    this.setState({ hasError: true });
  }
  
  render() {
    if (this.state.hasError) {
      return <ErrorPage />;
    }
    return this.props.children;
  }
}

// API error handling
axios.interceptors.response.use(
  response => response,
  error => {
    if (error.response?.status === 401) {
      // Redirect to login
      window.location.href = '/login';
    } else if (error.response?.status === 403) {
      // Show permission denied
      toast.error('Bạn không có quyền thực hiện thao tác này');
    } else if (error.response?.status >= 500) {
      // Show server error
      toast.error('Lỗi server. Vui lòng thử lại sau.');
    }
    return Promise.reject(error);
  }
);
```

**2. Backend Error Handling:**
```typescript
// Custom error classes
class AppError extends Error {
  constructor(message, statusCode, isOperational = true) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = isOperational;
  }
}

class ValidationError extends AppError {
  constructor(message) {
    super(message, 400);
  }
}

class NotFoundError extends AppError {
  constructor(message) {
    super(message, 404);
  }
}

class UnauthorizedError extends AppError {
  constructor(message) {
    super(message, 401);
  }
}

// Error handling middleware (phải đặt cuối cùng)
app.use((err, req, res, next) => {
  // Log error
  logger.error(err);
  
  // Sequelize validation error
  if (err.name === 'SequelizeValidationError') {
    return res.status(400).json({
      error: 'Validation failed',
      details: err.errors.map(e => e.message)
    });
  }
  
  // Sequelize unique constraint error
  if (err.name === 'SequelizeUniqueConstraintError') {
    return res.status(409).json({
      error: 'Record already exists',
      field: err.errors[0].path
    });
  }
  
  // Operational errors (expected)
  if (err.isOperational) {
    return res.status(err.statusCode).json({
      error: err.message
    });
  }
  
  // Programming errors (unexpected)
  return res.status(500).json({
    error: 'Internal server error',
    ...(process.env.NODE_ENV === 'development' && { 
      stack: err.stack 
    })
  });
});
```

### B. Transaction Rollback

**1. Database Transaction với Sequelize:**
```typescript
async function createOrderSafely(orderData) {
  const transaction = await sequelize.transaction();
  
  try {
    // Step 1: Create order
    const order = await Order.create(orderData, { transaction });
    
    // Step 2: Create order details
    for (const item of orderData.items) {
      await OrderDetail.create({
        orderId: order.id,
        ...item
      }, { transaction });
    }
    
    // Step 3: Deduct inventory
    for (const item of orderData.items) {
      const recipe = await ItemIngredient.findAll({
        where: { menuItemId: item.menuItemId }
      });
      
      for (const ing of recipe) {
        const ingredient = await Ingredient.findByPk(
          ing.ingredientId,
          { transaction, lock: true }
        );
        
        const requiredQty = ing.quantity * item.quantity;
        
        if (ingredient.stockQuantity < requiredQty) {
          throw new ValidationError(
            `Insufficient stock for ${ingredient.name}`
          );
        }
        
        await ingredient.update({
          stockQuantity: ingredient.stockQuantity - requiredQty
        }, { transaction });
      }
    }
    
    // Step 4: Update table status
    await Table.update(
      { status: 'OCCUPIED' },
      { where: { id: orderData.tableId }, transaction }
    );
    
    // All steps successful → Commit
    await transaction.commit();
    
    return order;
    
  } catch (error) {
    // Any step fails → Rollback
    await transaction.rollback();
    throw error;
  }
}
```

**2. Distributed Transaction (Saga Pattern):**
```typescript
// For operations involving external services
class OrderSaga {
  async execute(orderData) {
    const compensations = [];
    
    try {
      // Step 1: Reserve inventory
      const inventoryReserved = await inventoryService.reserve(
        orderData.items
      );
      compensations.push(() => inventoryService.unreserve(inventoryReserved));
      
      // Step 2: Process payment
      const payment = await paymentService.charge(orderData.payment);
      compensations.push(() => paymentService.refund(payment));
      
      // Step 3: Create order
      const order = await orderService.create(orderData);
      compensations.push(() => orderService.cancel(order.id));
      
      // Step 4: Send notifications
      await notificationService.send(order);
      
      return order;
      
    } catch (error) {
      // Rollback in reverse order
      for (const compensate of compensations.reverse()) {
        try {
          await compensate();
        } catch (compensateError) {
          logger.error('Compensation failed:', compensateError);
        }
      }
      
      throw error;
    }
  }
}
```

---

## 2.1.9. Performance Optimization

### A. Frontend Optimization

**1. Code Splitting:**
```typescript
// Lazy load routes
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Orders = lazy(() => import('./pages/Orders'));
const Menu = lazy(() => import('./pages/Menu'));

// In router
<Suspense fallback={<LoadingSpinner />}>
  <Routes>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/orders" element={<Orders />} />
    <Route path="/menu" element={<Menu />} />
  </Routes>
</Suspense>
```

**2. Memoization:**
```typescript
// Memoize expensive computations
const orderTotal = useMemo(() => {
  return order.items.reduce((sum, item) => 
    sum + (item.price * item.quantity), 0
  );
}, [order.items]);

// Memoize components
const OrderItem = React.memo(({ item }) => {
  return <div>{item.name}</div>;
});
```

**3. Virtual Scrolling:**
```typescript
// For large lists (e.g., 1000+ menu items)
import { FixedSizeList } from 'react-window';

<FixedSizeList
  height={600}
  itemCount={menuItems.length}
  itemSize={80}
>
  {({ index, style }) => (
    <div style={style}>
      <MenuItem item={menuItems[index]} />
    </div>
  )}
</FixedSizeList>
```

### B. Backend Optimization

**1. Database Query Optimization:**
```typescript
// Bad: N+1 query problem
const orders = await Order.findAll();
for (const order of orders) {
  order.items = await OrderDetail.findAll({
    where: { orderId: order.id }
  });
}

// Good: Use include (JOIN)
const orders = await Order.findAll({
  include: [{
    model: OrderDetail,
    as: 'items',
    include: [{ model: MenuItem, as: 'menuItem' }]
  }]
});

// Even better: Use pagination
const orders = await Order.findAll({
  include: [...],
  limit: 20,
  offset: (page - 1) * 20,
  order: [['createdAt', 'DESC']]
});
```

**2. Caching Strategy:**
```typescript
// Cache frequently accessed data
async function getMenuItems() {
  const cacheKey = 'cache:menu';
  
  // Try cache first
  const cached = await redis.get(cacheKey);
  if (cached) {
    return JSON.parse(cached);
  }
  
  // Cache miss → Query database
  const items = await MenuItem.findAll({
    where: { status: 'AVAILABLE' }
  });
  
  // Store in cache (1 hour TTL)
  await redis.setex(cacheKey, 3600, JSON.stringify(items));
  
  return items;
}

// Cache invalidation
async function updateMenuItem(id, data) {
  const updated = await MenuItem.update(data, { where: { id } });
  
  // Invalidate cache
  await redis.del('cache:menu');
  await redis.del(`cache:menu:${id}`);
  
  return updated;
}
```

**3. Connection Pooling:**
```typescript
// Sequelize connection pool
const sequelize = new Sequelize({
  dialect: 'postgres',
  pool: {
    max: 20,        // Maximum connections
    min: 5,         // Minimum connections
    acquire: 30000, // Max time to acquire connection
    idle: 10000     // Max idle time
  }
});

// Redis connection pool (via ioredis)
const redis = new Redis({
  port: 6379,
  host: '127.0.0.1',
  maxRetriesPerRequest: 3,
  enableReadyCheck: true,
  lazyConnect: false
});
```

### C. Monitoring & Profiling

**1. Application Monitoring:**
```typescript
// Winston logger with multiple transports
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    // Write all logs to console
    new winston.transports.Console(),
    
    // Write error logs to file
    new winston.transports.File({ 
      filename: 'logs/error.log', 
      level: 'error' 
    }),
    
    // Write all logs to file
    new winston.transports.File({ 
      filename: 'logs/combined.log' 
    })
  ]
});

// Log slow queries
sequelize.addHook('afterQuery', (options) => {
  if (options.duration > 1000) { // > 1 second
    logger.warn('Slow query detected', {
      sql: options.sql,
      duration: options.duration
    });
  }
});
```

**2. Performance Metrics:**
```typescript
// Middleware to track response time
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = Date.now() - start;
    
    logger.info('HTTP Request', {
      method: req.method,
      url: req.url,
      statusCode: res.statusCode,
      duration: `${duration}ms`
    });
    
    // Alert if slow
    if (duration > 2000) {
      logger.warn('Slow request detected', {
        method: req.method,
        url: req.url,
        duration: `${duration}ms`
      });
    }
  });
  
  next();
});
```

---

## 2.1.10. Security Best Practices

### A. Authentication Security

```typescript
// 1. Password hashing (bcrypt)
const hashedPassword = await bcrypt.hash(password, 10);

// 2. JWT with expiration
const token = jwt.sign(
  { userId: user.id, role: user.role },
  JWT_SECRET,
  { expiresIn: '24h' }
);

// 3. Refresh token mechanism
const refreshToken = jwt.sign(
  { userId: user.id },
  REFRESH_TOKEN_SECRET,
  { expiresIn: '7d' }
);

// Store refresh token in httpOnly cookie
res.cookie('refreshToken', refreshToken, {
  httpOnly: true,
  secure: true,    // HTTPS only
  sameSite: 'strict',
  maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
});
```

### B. Input Validation & Sanitization

```typescript
// Using Joi for validation
const orderSchema = Joi.object({
  tableId: Joi.string().uuid().required(),
  items: Joi.array().items(
    Joi.object({
      menuItemId: Joi.string().uuid().required(),
      quantity: Joi.number().integer().min(1).max(99).required(),
      note: Joi.string().max(500).optional()
    })
  ).min(1).required()
});

// Sanitize HTML to prevent XSS
import sanitizeHtml from 'sanitize-html';

const sanitizedNote = sanitizeHtml(note, {
  allowedTags: [],
  allowedAttributes: {}
});
```

### C. SQL Injection Prevention

```typescript
// Sequelize automatically prevents SQL injection
// through parameterized queries

// Safe (Sequelize)
const users = await User.findAll({
  where: { email: userInput }
});

// Dangerous (Raw query) - NEVER do this:
// const users = await sequelize.query(
//   `SELECT * FROM users WHERE email = '${userInput}'`
// );

// If raw query is needed, use replacements:
const users = await sequelize.query(
  'SELECT * FROM users WHERE email = :email',
  {
    replacements: { email: userInput },
    type: QueryTypes.SELECT
  }
);
```

### D. CORS Configuration

```typescript
import cors from 'cors';

app.use(cors({
  origin: process.env.FRONTEND_URL || 'http://localhost:3000',
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

### E. Rate Limiting

```typescript
import rateLimit from 'express-rate-limit';

// General API rate limit
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // 100 requests per window
  message: 'Too many requests, please try again later'
});

app.use('/api/', apiLimiter);

// Strict rate limit for authentication
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // 5 login attempts per 15 minutes
  skipSuccessfulRequests: true
});

app.use('/api/auth/login', authLimiter);
```

---

## 2.1.11. Tổng kết Kiến trúc 3 Tầng

### Bảng So sánh Vai trò

| Tiêu chí | Presentation Tier | Business Logic Tier | Data Tier |
|----------|------------------|---------------------|-----------|
| **Ngôn ngữ** | JavaScript/TypeScript | JavaScript/TypeScript | SQL |
| **Framework** | React | Express.js | PostgreSQL |
| **Trách nhiệm chính** | Hiển thị & UX | Xử lý nghiệp vụ | Lưu trữ dữ liệu |
| **Giao tiếp với** | Backend API | Frontend & Database | Backend only |
| **State management** | Local + Global | Stateless (JWT) | Persistent |
| **Scalability** | CDN, Caching | Horizontal + Vertical | Read replicas |
| **Security focus** | XSS, CSRF | Authentication, Authorization | SQL injection, Encryption |
| **Testing** | Unit + E2E | Unit + Integration | Migration + Queries |

### Checklist Hoàn thành

✅ **Presentation Tier:**
- [ ] React app với TypeScript
- [ ] Component library (Ant Design)
- [ ] State management (Zustand/Redux)
- [ ] API client (Axios)
- [ ] WebSocket client (Socket.io)
- [ ] Form validation (React Hook Form + Yup)
- [ ] Routing (React Router)
- [ ] Error handling & boundaries

✅ **Business Logic Tier:**
- [ ] Express.js server
- [ ] RESTful API endpoints
- [ ] WebSocket server
- [ ] Authentication middleware (JWT)
- [ ] Authorization middleware (RBAC)
- [ ] Input validation (Joi/Zod)
- [ ] Business logic services
- [ ] Repository pattern
- [ ] Error handling middleware
- [ ] Logging (Winston)
- [ ] Scheduled jobs (Cron)

✅ **Data Tier:**
- [ ] PostgreSQL database
- [ ] Sequelize ORM setup
- [ ] Database migrations
- [ ] Seed data
- [ ] Indexes optimization
- [ ] Triggers & stored procedures
- [ ] Redis cache setup
- [ ] Backup strategy

### Kết luận

Kiến trúc 3 tầng của hệ thống PTIT-RMS được thiết kế với những ưu điểm:

1. **Tách biệt rõ ràng**: Mỗi tầng có trách nhiệm riêng biệt
2. **Dễ bảo trì**: Thay đổi một tầng không ảnh hưởng tầng khác
3. **Khả năng mở rộng**: Có thể scale từng tầng độc lập
4. **Bảo mật tốt**: Multiple layers of security
5. **Hiệu năng cao**: Caching, connection pooling, query optimization
6. **Dễ test**: Unit test, integration test, E2E test

Kiến trúc này đáp ứng đầy đủ các yêu cầu phi chức năng:
- ⚡ **Performance**: Response time < 2s
- 🔒 **Security**: JWT, RBAC, encryption
- 📈 **Scalability**: Horizontal & vertical scaling
- 🛡️ **Reliability**: Transaction management, error handling
- 🔧 **Maintainability**: Clean code, clear structure

---

**Chương tiếp theo (2.2)** sẽ trình bày chi tiết về **Biểu đồ Use Case tổng thể** với đầy đủ Actors và mối quan hệ giữa các Use Cases.