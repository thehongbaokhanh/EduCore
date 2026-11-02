# EduCore — Cấu trúc dự án

> Phạm vi: cấu trúc được người dùng phê duyệt và áp dụng ngày 02/11/2026. Chức năng nghiệp vụ chưa được triển khai chỉ vì đã có thư mục nền.

## Cấu trúc gốc

```text
EduCore/
├── .git/                         # Một repository chung, nhánh main
├── .github/                      # Cấu hình GitHub hiện có; CI bổ sung khi cần
├── AGENTS.md                     # Quy tắc dành cho coding agent
├── README.md                     # Điểm bắt đầu và lệnh chạy/kiểm tra
├── .gitignore                    # Loại cache, output, bí mật và bản sao cục bộ
├── docs/                         # Yêu cầu, quyết định, cấu trúc, tiến độ
├── frontend/                     # Angular workspace
├── backend/                      # Spring Boot, Gradle project trực tiếp
└── infra/
    ├── docker/                   # Docker Compose khi triển khai hạ tầng
    └── nginx/                    # Nginx khi triển khai reverse proxy
```

`.codegraph/` là index cục bộ. `.local-backups/` giữ dữ liệu khôi phục trước khi đổi Git/nâng Angular và bản ZIP backend ban đầu. Hai thư mục này được Git bỏ qua.

## Frontend

```text
frontend/
├── public/                       # Ảnh, icon và tài nguyên tĩnh
├── src/
│   ├── app/
│   │   ├── core/                 # Dịch vụ cấp ứng dụng: auth, guard, interceptor
│   │   ├── layouts/              # Khung trang: menu, header, vùng nội dung
│   │   ├── shared/               # Component, directive, pipe dùng chung
│   │   ├── features/             # Chức năng theo nghiệp vụ
│   │   ├── app.config.ts
│   │   ├── app.routes.ts
│   │   └── app.*                 # Component gốc
│   ├── styles/                   # Tokens, theme và style nền khi có Design System
│   └── styles.scss               # Điểm vào stylesheet toàn ứng dụng
├── angular.json
├── package.json
└── package-lock.json
```

- Component standalone, route chức năng được lazy-load khi triển khai.
- Tạo chức năng trong `features/<ten-chuc-nang>/`, ví dụ `students`, `enrollments`, `grades`. Tách `pages/`, `components/`, `services/`, `models/` khi chức năng thực sự cần.
- Component gọi service/facade; service quản lý việc giao tiếp API. Model chỉ phục vụ một chức năng được đặt trong chức năng đó.
- `core/` chứa hành vi cấp ứng dụng, `shared/` chứa thành phần tái sử dụng. Tránh gom toàn bộ service/model của dự án vào hai thư mục này.
- Unit/component test đặt gần file source tương ứng với đuôi `.spec.ts`.
- Tôn trọng quy tắc bổ sung trong `frontend/AGENTS.md`.

## Backend

```text
backend/
├── src/
│   ├── main/
│   │   ├── java/com/educare/backend/
│   │   │   ├── BackendApplication.java
│   │   │   ├── config/           # Cấu hình tích hợp và hạ tầng dùng chung
│   │   │   ├── shared/           # Thành phần dùng chung có nhu cầu thực tế
│   │   │   └── <module>/         # Tạo khi bắt đầu nghiệp vụ tương ứng
│   │   └── resources/
│   │       ├── application.properties
│   │       └── db/migration/     # SQL migration khi tích hợp Flyway
│   └── test/java/com/educare/backend/
├── gradle/wrapper/
├── gradlew
├── gradlew.bat
├── build.gradle
└── settings.gradle
```

- Một ứng dụng Spring Boot theo Modular Monolith, phân package theo nghiệp vụ trước: ví dụ `students`, `courses`, `coursesections`, `enrollments`, `grades`, `reports`, `auth`.
- Trong từng module, tách `controller/`, `service/`, `repository/`, `dto/`, `entity/` khi cần. Luồng chính: controller → service → repository.
- Module khác trao đổi qua service/DTO rõ ràng; tránh truy cập tùy tiện repository nội bộ của nhau.
- Validation, phân quyền, transaction và quy tắc nghiệp vụ thuộc backend. Test đặt theo package tương ứng trong `src/test/java/`.
- Giữ namespace hiện có `com.educare.backend`; tên workspace EduCore không tự động đổi package Java.
- Thư mục migration hiện là vị trí chuẩn bị; Flyway chưa được cài và chưa có schema/migration được phê duyệt.

## Quy tắc duy trì

- File `.gitkeep` giữ các thư mục nền trong Git; có thể bỏ khi thư mục đã chứa file thực.
- Dependency, cache, kết quả build, credentials và backup cục bộ được loại khỏi Git. `package-lock.json`, Gradle Wrapper và source cần được quản lý phiên bản.
- `docs/PROJECT_REQUIREMENTS.md`: nghiệp vụ; `docs/TECH_STACK.md`: quyết định kỹ thuật; Design System: UI/UX; `docs/PROJECT_PROGRESS.md`: tiến độ đã kiểm chứng.
- Cập nhật tiến độ cùng phạm vi công việc đã được giao; không xem thư mục hay tài liệu là bằng chứng chức năng đã chạy.
