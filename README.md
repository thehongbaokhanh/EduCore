# EduCore

Workspace phát triển ứng dụng quản lý điểm sinh viên: Angular ở `frontend/`, Spring Boot ở `backend/`, tài liệu ở `docs/`, cấu hình triển khai ở `infra/`.

## Tài liệu làm việc

- [Yêu cầu nghiệp vụ](docs/PROJECT_REQUIREMENTS.md)
- [Công nghệ và quyết định kiến trúc](docs/TECH_STACK.md)
- [Cấu trúc và quy ước đặt source](docs/PROJECT_STRUCTURE.md)
- [Tiến độ và việc còn mở](docs/PROJECT_PROGRESS.md)
- [Quy tắc cộng tác với coding agent](AGENTS.md)

Design System `EduCore_Design_System_v1.0.md` chưa có trong workspace. Cần bổ sung tài liệu gốc trước khi triển khai giao diện nghiệp vụ.

## Môi trường

- Node.js 24 LTS, tối thiểu 24.15.0 cho Angular 22; npm theo `frontend/package.json`.
- JDK 21, đặt `JAVA_HOME` trỏ đến JDK 21. Dùng Gradle Wrapper có sẵn trong backend.
- Phiên bản dependency thực tế nằm trong `frontend/package-lock.json` và `backend/build.gradle`.

## Chạy trên máy phát triển

Frontend, mở PowerShell tại thư mục gốc:

```powershell
Set-Location frontend
npm ci
npm start
```

Backend, mở một PowerShell khác tại thư mục gốc:

```powershell
Set-Location backend
.\gradlew.bat bootRun
```

Hiện backend là starter, chưa có REST controller hay kết nối PostgreSQL/Redis. Hạ tầng và các chức năng nghiệp vụ sẽ được triển khai theo tiến độ.

## Kiểm tra

```powershell
# Chạy tại frontend/
npm run build

# Chạy tại backend/
.\gradlew.bat test bootJar --console=plain
```

Frontend chưa có bộ test chạy được: `src/app/app.spec.ts` và `tsconfig.spec.json` đã bị xóa từ trước khi tổ chức lại workspace. Xem trạng thái kiểm chứng cụ thể trong `docs/PROJECT_PROGRESS.md`.

## Git

Repository chung đặt tại gốc `EduCore/`, nhánh `main`. Chạy `git status` tại đây để xem thay đổi của cả frontend, backend và tài liệu. Chưa tạo commit hoặc kết nối remote trong lần tổ chức workspace này.

Metadata Git frontend trước đây được giữ tại `.local-backups/frontend-git-20261102/`; source frontend trước khi nâng Angular và file ZIP backend cũ cũng được giữ trong `.local-backups/`. Thư mục này chỉ dùng để khôi phục cục bộ và được loại khỏi Git.

`.gitignore` loại trừ cấu hình máy cá nhân, môi trường cục bộ, credentials, khóa bí mật, dữ liệu cục bộ và bản sao lưu. Chỉ đưa giá trị mẫu vào `.env.example` hoặc `.env.*.example`; cấu hình ứng dụng dùng biến môi trường cho thông tin nhạy cảm. Giữ migration SQL và lockfile dependency trong Git.

Trước mỗi commit, kiểm tra `git diff --cached` vì `.gitignore` chỉ lọc đường dẫn, không phát hiện bí mật nằm trong source/tài liệu hoặc loại bỏ file đã được Git theo dõi. Dùng email GitHub `noreply` cho metadata commit nếu cần giữ kín email cá nhân.

## CodeGraph

Chạy tại gốc workspace sau khi đã khởi tạo index:

```powershell
codegraph status .
codegraph explore "App BackendApplication" --path . --max-files 2
codegraph sync .
```

Ưu tiên `codegraph_explore` khi MCP đã kết nối; CLI `codegraph explore` là cách sử dụng trực tiếp cùng cơ chế truy vấn. Dùng `sync` sau khi sửa source nếu index chưa được cập nhật. Không cần khởi tạo lại index cho từng lần đọc code. File index trong `.codegraph/` được loại khỏi Git.
