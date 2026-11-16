# BlueMoon Apartment Management System

Hệ thống Quản lý Chung cư BlueMoon (IT3180 - Dự án Nhóm 11).

---

## 📁 Cấu trúc Thư mục Dự án

```text
bluemoon-apartment-management/
├── .github/
│   └── workflows/                  # CI/CD tự động chạy pytest & kiểm tra gộp nhánh
├── docs/
│   ├── architecture/               # Sơ đồ kiến trúc ERD, Class Diagram (PlantUML)
│   ├── api-specs/                  # Đặc tả OpenAPI / Swagger xuất từ FastAPI
│   └── user-guides/                # Tài liệu HDSD cho Ban Quản trị (BQT) & Cư dân [Mạnh - Task 4.5]
├── infra/
│   ├── docker/                     # Dockerfile cho Backend (Python) & Frontend (Node/Nginx)
│   ├── nginx/                      # Cấu hình Reverse Proxy & SSL/TLS [Vitou - Task 4.4]
│   └── scripts/                    # Script backup DB tự động hằng ngày (RAW-038) & rollback
│
├── backend/                        # Python (FastAPI + SQLAlchemy 2.0 + Pydantic v2)
│   ├── alembic/                    # Quản lý lịch sử thay đổi cấu trúc bảng CSDL (DB Migrations)
│   │   └── versions/               # Chứa các bản ghi nâng cấp/hạ cấp schema tự động
│   ├── scripts/                    # Script nạp dữ liệu mẫu & chuẩn hóa 500 căn hộ [Phong - Task 4.3]
│   ├── app/
│   │   ├── core/                   # Hạ tầng cốt lõi dùng chung toàn hệ thống
│   │   │   ├── config/             # Quản lý biến môi trường (Pydantic Settings)
│   │   │   ├── database/           # Khởi tạo AsyncEngine, SessionMaker & quản lý Transaction
│   │   │   ├── security/           # JWT, băm mật khẩu BCrypt, đếm 5 lần sai khóa nick (RAW-028, 029)
│   │   │   ├── audit/              # Middleware/Decorator ghi vết Audit Log tự động
│   │   │   └── exceptions/         # Bắt lỗi toàn cục & chuẩn hóa JSON lỗi trả về
│   │   ├── shared/                 # Tiện ích dùng chung giữa các phân hệ
│   │   │   ├── enums/              # Các hằng số trạng thái (Thường trú/Tạm trú, Đã đóng/Còn thiếu...)
│   │   │   ├── utils/              # Xử lý số tiền Decimal, xuất Excel (openpyxl), sinh QR code
│   │   │   └── notifications/      # Dịch vụ gửi Email / Push Notification (< 3s cho PRK03)
│   │   └── modules/                # Chia độc lập theo 6 phân hệ nghiệp vụ
│   │       ├── aas/                # [Phong / Sơn] Xác thực JWT (AAS01) & Phân quyền RBAC (AAS02)
│   │       │   ├── routers/        # (Controllers) Khai báo API endpoints
│   │       │   ├── services/       # (Business Logic) Xử lý đăng nhập, khóa tài khoản, phân quyền
│   │       │   ├── repositories/   # (Data Access) Truy vấn bảng users, roles, audit_logs
│   │       │   ├── models/         # (Entities) Khai báo bảng SQLAlchemy 2.0
│   │       │   └── schemas/        # (DTO) Khuôn kiểm duyệt dữ liệu Pydantic v2
│   │       ├── hom/                # [Doanh / Phong] Căn hộ (HOM01), Chủ hộ (HOM02), Nhân khẩu (HOM03), Biến động (HOM04)
│   │       │   ├── routers/
│   │       │   ├── services/
│   │       │   ├── repositories/
│   │       │   ├── models/
│   │       │   └── schemas/
│   │       ├── hrm/                # [Mạnh / Phong / Doanh] Hồ sơ BQT (HRM03), Ca trực (HRM01), Chấm công & Sự cố (HRM02)
│   │       │   ├── routers/
│   │       │   ├── services/
│   │       │   ├── repositories/
│   │       │   ├── models/
│   │       │   └── schemas/
│   │       ├── fin/                # [Bình] Biểu phí & Quyên góp (FIN01), Thu phí (FIN02), Đối soát công nợ (FIN03)
│   │       │   ├── routers/
│   │       │   ├── services/
│   │       │   ├── repositories/
│   │       │   ├── models/
│   │       │   └── schemas/
│   │       ├── prk/                # [Vitou] Phương tiện & Vị trí đỗ (PRK01), Thẻ xe (PRK02), Phí xe trực tuyến (PRK03)
│   │       │   ├── routers/
│   │       │   ├── services/
│   │       │   ├── repositories/
│   │       │   ├── models/
│   │       │   └── schemas/
│   │       └── rep/                # [Sprint 3] Tra cứu tổng hợp < 1s (REP01) & Báo cáo thống kê (REP02)
│   │           ├── routers/
│   │           ├── services/
│   │           ├── repositories/
│   │           └── schemas/        # Đọc dữ liệu tổng hợp từ các bảng khác
│   └── tests/                      # Phục vụ Milestone 1 & 2 UAT [Mai Hoàng Sơn - Task 2.8, 3.9, 4.2]
│       ├── unit/                   # Test độc lập từng hàm tính phí DECIMAL, logic khóa tài khoản
│       ├── integration/            # Test luồng liên phân hệ (HOM -> FIN, PRK -> FIN)
│       └── load_test/              # Kịch bản Locust/k6 kiểm thử chịu tải < 1s - 2s
│
└── frontend/                       # [Mạnh - Task 2.7] Giao diện Web React + TypeScript + Vite
    ├── public/                     # Static assets công khai
    └── src/
        ├── api/                    # Cấu hình Axios / API client
        ├── assets/                 # Hình ảnh, icons, font
        ├── components/             # Reusable UI components dùng chung
        ├── hooks/                  # Custom React hooks
        ├── layouts/                # Layout khung sườn (Header, Sidebar, Footer)
        ├── routes/                 # Định tuyến ứng dụng React Router
        ├── store/                  # Quản lý state toàn cục (Redux Toolkit / Zustand)
        ├── types/                  # TypeScript interface / types dùng chung
        ├── utils/                  # Hàm tiện ích helpers
        └── features/               # Các module chức năng theo nghiệp vụ
            ├── aas-auth/           # Đăng nhập, xác thực, phân quyền
            ├── hom-apartments/     # Quản lý căn hộ, cư dân, nhân khẩu
            ├── hrm-operations/     # Quản lý nhân sự BQT, ca trực, chấm công
            ├── fin-billing/        # Quản lý thu phí, biểu phí, công nợ
            ├── prk-parking/        # Quản lý bãi đỗ xe, thẻ xe, phí gửi xe
            └── rep-analytics/      # Báo cáo, thống kê dữ liệu
```

---

## 📖 Ý nghĩa & Chi tiết các thư mục

### 1. `.github/`
- **`workflows/`**: Cấu hình pipeline CI/CD với GitHub Actions, tự động chạy kiểm thử (`pytest`), linting code và kiểm tra hợp lệ khi tạo Pull Request vào nhánh chính.

### 2. `docs/` (Tài liệu dự án)
- **`architecture/`**: Bản vẽ thiết kế hệ thống, sơ đồ quan hệ thực thể (ERD), biểu đồ lớp (Class Diagram) định dạng PlantUML hoặc hình ảnh.
- **`api-specs/`**: Tài liệu đặc tả API chuẩn OpenAPI v3 / Swagger được trích xuất từ FastAPI.
- **`user-guides/`**: Tài liệu hướng dẫn sử dụng dành cho Ban Quản trị và Cư dân chung cư.

### 3. `infra/` (Hạ tầng triển khai)
- **`docker/`**: Chứa Dockerfile và cấu hình container đa dịch vụ (Backend FastAPI, Frontend React, PostgreSQL).
- **`nginx/`**: Thiết lập Reverse Proxy, điều hướng request và quản lý chứng chỉ SSL/TLS.
- **`scripts/`**: Kịch bản tự động hóa sao lưu (backup) cơ sở dữ liệu định kỳ hằng ngày và quy trình khôi phục (rollback).

### 4. `backend/` (Ứng dụng Backend - FastAPI)
Xây dựng theo kiến trúc phân tầng (Layered Architecture) kết hợp Module hóa theo Domain (Modular Monolith):
- **`alembic/versions/`**: Quản lý di chuyển và phiên bản hóa lược đồ cơ sở dữ liệu.
- **`scripts/`**: Script tiện ích nạp dữ liệu khởi tạo (seed data) và chuẩn hóa dữ liệu hơn 500 căn hộ mẫu.
- **`app/core/`**: Chứa các cấu phần nền tảng: cấu hình môi trường, khởi tạo DB async, xác thực bảo mật JWT/BCrypt, middleware ghi vết kiểm toán (Audit Log) và xử lý ngoại lệ tập trung.
- **`app/shared/`**: Chứa các enum trạng thái, tiện ích tính toán tiền tệ (Decimal), xuất Excel, sinh mã QR, và dịch vụ gửi thông báo.
- **`app/modules/`**: Chia theo 6 phân hệ cốt lõi:
  - **`aas`** (Authentication & Authorization): Xác thực người dùng, phân quyền RBAC.
  - **`hom`** (Household & Resident): Quản lý căn hộ, chủ hộ, nhân khẩu, biến động tạm trú/tạm vắng.
  - **`hrm`** (Human Resource Management): Quản lý ban quản trị, ca trực, chấm công và ghi nhận sự cố.
  - **`fin`** (Finance & Billing): Quản lý biểu phí, quyên góp, thu phí và đối soát công nợ.
  - **`prk`** (Parking Management): Quản lý vị trí đỗ xe, đăng ký thẻ xe và thu phí xe trực tuyến.
  - **`rep`** (Reporting & Analytics): Tra cứu tổng hợp nhanh và kết xuất báo cáo thống kê.
- **`tests/`**: Kiểm thử toàn diện bao gồm Unit Test (kiểm tra tính toán số học, logic nghiệp vụ), Integration Test (luồng liên phân hệ) và Load Test (chịu tải hệ thống).

### 5. `frontend/` (Ứng dụng Frontend - React + Vite + TypeScript)
Tổ chức theo mô hình **Feature-driven Architecture**:
- Các thư mục dùng chung: `api/`, `assets/`, `components/`, `hooks/`, `layouts/`, `routes/`, `store/`, `types/`, `utils/`.
- **`src/features/`**: Gom cụm mã nguồn (components, services, hooks, types) riêng biệt cho từng nghiệp vụ tương ứng với 6 phân hệ Backend (`aas-auth`, `hom-apartments`, `hrm-operations`, `fin-billing`, `prk-parking`, `rep-analytics`).
