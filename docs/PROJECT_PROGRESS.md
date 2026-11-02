# EduCore — Tiến độ dự án

> Cập nhật: **02/11/2026**. Chỉ ghi nhận việc đã thực hiện và kiểm chứng; thư mục nền không đồng nghĩa chức năng đã hoàn thành.

## Trạng thái hiện tại

| Phạm vi | Trạng thái đã xác minh |
|---|---|
| Workspace | Đã tổ chức `frontend/`, `backend/`, `docs/`, `infra/`; backend không còn lồng `educore-backend/` |
| Git | Một repository tại gốc `EduCore/`, nhánh `main`, chưa có commit hoặc remote tại thời điểm tổ chức nền dự án |
| Frontend | Starter Angular 22.2.4, CLI/build 22.2.5, TypeScript 6.0.3; build đạt |
| Backend | Starter Spring Boot 4.1.2, Java toolchain 21, Gradle Wrapper 9.8.2; test và bootJar đạt |
| CodeGraph | CLI/MCP server 1.6.2 hoạt động; index tại gốc, truy vấn được frontend và backend |
| Nghiệp vụ/API | Chưa triển khai module, REST controller, auth, phân quyền, quản lý điểm hoặc đăng ký học phần |
| Database/hạ tầng | Chưa tích hợp PostgreSQL, Flyway, Redis, Cloudinary; chưa có Docker Compose/Nginx chạy được |
| UI/UX | Chưa triển khai giao diện nghiệp vụ; còn thiếu file Design System gốc |

## Nhật ký — 02/11/2026: tổ chức nền dự án

**Phạm vi đã được giao:** áp dụng cấu trúc đã duyệt, đưa Git về gốc, đánh giá phiên bản, bổ sung tài liệu tiến độ và bắt đầu dùng CodeGraph.

**Đã thực hiện:**

- Di chuyển toàn bộ Gradle project từ `backend/educore-backend/` lên `backend/`. Đối chiếu SHA256 của **12 file source/cấu hình** trước và sau di chuyển: nội dung giữ nguyên.
- Tạo các thư mục nền `frontend/src/app/{core,layouts,shared,features}`, `frontend/src/styles`, backend `config/`, `shared/`, `resources/db/migration/`, và `infra/{docker,nginx}`. Dùng `.gitkeep` để giữ các thư mục nền trong Git.
- Sao lưu metadata Git frontend vào `.local-backups/frontend-git-20261102/`; tạo Git mới tại gốc trên nhánh `main`. Chưa stage, commit, push hoặc cấu hình remote.
- Giữ ZIP backend ban đầu tại `.local-backups/educare-backend.zip`; lưu source/cấu hình frontend trước nâng cấp tại `.local-backups/frontend-before-angular22/`.
- Nâng Angular 21.2.x lên **22.2.4**, CLI/build **22.2.5**, TypeScript **6.0.3** bằng `ng update @angular/core@22 @angular/cli@22 --allow-dirty`. Không tạo commit tự động.
- Giữ Spring Boot **4.1.2 + Java 21** sau đối chiếu nguồn chính thức. JUnit Jupiter **6.0.3**, Mockito **5.23.0** theo dependency management hiện có của Boot; không thêm framework test.
- Rà soát migration Angular: component gốc dùng `OnPush` theo quy tắc frontend; giữ strict template checking và không giữ các suppress diagnostics không cần thiết. Bỏ reference `tsconfig.spec.json` trong `tsconfig.json` vì file đó đã bị xóa trước khi làm nhiệm vụ.
- Tạo `README.md`, `.gitignore`, `docs/PROJECT_STRUCTURE.md`, tài liệu tiến độ này; cập nhật `docs/TECH_STACK.md` lên 1.1.0, `frontend/README.md` và cảnh báo phiên bản backend trong `AGENTS.md` theo phiên bản thực tế.
- Hoàn thiện gói chạy Windows CodeGraph 1.6.2, khởi tạo index ở gốc, dùng `explore` để đọc `App`/`BackendApplication` và `sync` sau khi chỉnh source.

**Kiểm chứng đã chạy:**

| Kiểm tra | Kết quả |
|---|---|
| SHA256 backend trước/sau di chuyển | 12/12 file giữ nguyên |
| `npm run build` tại `frontend/` | PASS; production bundle được tạo |
| `npm ls --depth=0` | PASS; không báo dependency invalid/missing |
| `npm test -- --watch=false` | FAIL: `No tests found`; dự án chưa có file test |
| `gradlew.bat test bootJar --console=plain` tại `backend/` | PASS; 1 test, 0 lỗi/failed/skipped; tạo bootJar |
| Gradle `dependencyInsight` cho `junit-jupiter` | PASS; xác minh JUnit 6.0.3 và Mockito 5.23.0 |
| `git rev-parse --show-toplevel` từ gốc và frontend | Cùng trỏ đến thư mục gốc `EduCore/` |
| `git check-ignore` | Cache/dependency/output của hai ứng dụng, `.codegraph/`, `.local-backups/` được bỏ qua |
| Kiểm tra cấu trúc và liên kết tài liệu cục bộ | PASS; 19 đường dẫn cần thiết tồn tại, không còn Git/frontend hoặc backend lồng, liên kết trong tài liệu mới hợp lệ |
| `codegraph status . --json` sau `sync` | `complete`; 8 file, 30 node, 29 edge; 0 thay đổi chờ xử lý, 0 pending refs |
| `codegraph explore` cho `App` và `BackendApplication` | PASS; đúng đường dẫn source mới và quan hệ gọi |
| MCP `initialize` và `tools/list` trực tiếp | PASS; server 1.6.2 cung cấp `codegraph_explore` |

Môi trường kiểm tra: Node.js **24.22.0**, npm **11.19.1**, JDK **21.0.9** qua `JAVA_HOME`. Lệnh `java` trên PATH hiện trỏ JDK 25; Gradle Wrapper đã dùng `JAVA_HOME` và toolchain 21 đúng cấu hình.

## Việc còn mở và bước tiếp theo

1. Bổ sung **`docs/EduCore_Design_System_v1.0.md`** từ tài liệu gốc; chưa được tự tạo lại tokens hay thiết kế đã khóa.
2. Frontend `src/app/app.spec.ts` và `tsconfig.spec.json` đã bị xóa trước nhiệm vụ; vẫn giữ trạng thái xóa. Thiết lập lại test khi bắt đầu triển khai chức năng có hành vi cần kiểm chứng.
3. Chốt mô hình dữ liệu và các chính sách nghiệp vụ còn mở trong yêu cầu trước khi tạo schema/migration.
4. Tích hợp backend theo từng phạm vi được giao: REST API, PostgreSQL/Flyway, bảo mật session/CSRF và quyền tài nguyên. Kiểm tra tương thích Boot 4.1.2 khi thêm từng tích hợp.
5. Cấu hình MCP CodeGraph toàn cục đã tồn tại và đúng lệnh `codegraph serve --mcp`; server đã được kiểm tra trực tiếp. Phiên chat thực hiện nhiệm vụ chưa được cấp tool MCP này, nên đã dùng CLI `explore` để đọc code. Kết nối MCP của từng phiên/client cần được kiểm tra riêng.
6. Chưa kiểm thử E2E, giao diện theo Design System, database/transaction, phân quyền hoặc deployment vì các phần này chưa được triển khai.

Lệnh CodeGraph thường dùng tại gốc: `codegraph status .`, `codegraph explore "<symbol hoặc file>" --path . --max-files 2`, `codegraph sync .`. Với MCP, truyền `projectPath` là đường dẫn tuyệt đối đến workspace `EduCore/` trên máy hiện tại.
