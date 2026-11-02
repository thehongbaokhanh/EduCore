# EduCore — TECH STACK & ARCHITECTURE DECISIONS

> **Phiên bản tài liệu:** 1.1.0  
> **Ngày tổng hợp:** 02/11/2026  
> **Trạng thái:** Đã ghi nhận 16/16 quyết định ban đầu; cập nhật phiên bản và cấu trúc ngày 02/11/2026 theo ủy quyền của người dùng. **Chưa đồng nghĩa mọi cấu hình triển khai đã chốt**; tiến độ thực tế xem `PROJECT_PROGRESS.md`.  
> **Dự án:** Website Quản lý điểm sinh viên — EduCore.  
> **Nguồn yêu cầu:** `PROJECT_REQUIREMENTS.md`.  
> **Nguồn thiết kế:** `EduCore_Design_System_v1.0.md` (Design System chính thức, không thay đổi bằng tài liệu này).

## 0. Quy ước và nguyên tắc

- **ĐÃ CHỐT:** Người dùng đã lựa chọn trực tiếp trong cuộc thảo luận `01 — Tech Stack & Architecture Decisions`.
- **BẮT BUỘC:** Yêu cầu gốc hoặc quy chuẩn thiết kế đã có hiệu lực, không phải lựa chọn mới.
- **ĐỀ XUẤT KỸ THUẬT:** Cách triển khai khuyến nghị; **không được hiểu là đã chốt** nếu chưa được người dùng phê duyệt.
- **CÒN MỞ:** Phiên bản patch, tham số vận hành hoặc quyết định chi tiết chưa xác nhận.
- **CẢNH BÁO:** Rủi ro cần giải quyết trước khi đưa sản phẩm vào môi trường thực tế.

**Thứ tự ưu tiên khi có mâu thuẫn:** yêu cầu gốc / thay đổi người dùng đã xác nhận → quyết định công nghệ đã chốt trong tài liệu này → Design System đã khóa cho khía cạnh UI/UX → đề xuất triển khai. Khi có mâu thuẫn, báo rõ và yêu cầu xác nhận; không tự đổi công nghệ hoặc nghiệp vụ.

---

## 1. Bảng Tech Stack đã chốt

| STT | Nhóm | Công nghệ / quyết định | Phiên bản hoặc phạm vi | Trạng thái |
|---:|---|---|---|---|
| 01 | Frontend | **Angular** | **22.2.4**, CLI/build **22.2.5** | Major 22 đã chốt; đã nâng starter bằng Angular CLI |
| 02 | Backend | **Spring Boot (Java)** | **Spring Boot 4.1.2**, **Java 21 LTS** | Giữ phiên bản backend hiện có sau đối chiếu nguồn chính thức; thay lựa chọn 3.5.x ban đầu |
| 03 | Database | **PostgreSQL** | **18** | **ĐÃ CHỐT** |
| 04 | Authentication | **Server-side Session + Cookie** | Chưa chốt thời hạn phiên/cấu hình cookie | **ĐÃ CHỐT cơ chế** |
| 05 | ORM / Data Access | **Spring Data JPA + Hibernate** | Phiên bản thư viện: chưa chốt riêng | **ĐÃ CHỐT** |
| 06 | Database Migration | **Flyway**, sử dụng SQL migrations | Phiên bản Flyway: chưa chốt riêng | **ĐÃ CHỐT** |
| 07 | UI component library | **Angular Material + Angular CDK** | Cùng major **22** là hướng căn chỉnh với Angular 22; patch chưa chốt | **ĐÃ CHỐT thư viện** |
| 08 | Grade Data Grid | **AG Grid Community** | Phiên bản tương thích cần kiểm thử; AG Grid 36 là ứng viên đã xác minh | **ĐÃ CHỐT Community**, không mặc định Enterprise |
| 09 | Charts | **Chart.js + ng2-charts** | ng2-charts 11 tương thích Angular 22; patch chưa chốt | **ĐÃ CHỐT thư viện** |
| 10 | Import / Export | **Apache POI + Apache Commons CSV**, xử lý chủ yếu tại Spring Boot | Phiên bản: chưa chốt | **ĐÃ CHỐT** |
| 11 | Cache | **Spring Cache + Redis** | Phiên bản Redis / TTL: chưa chốt | **ĐÃ CHỐT**, **thay thế Caffeine** |
| 12 | Avatar | **Cloudinary** | Gói dịch vụ / chính sách ảnh: chưa chốt | **ĐÃ CHỐT**, không dùng local file làm phương án chính |
| 13 | Testing | **Vitest + JUnit Jupiter/Mockito + Testcontainers + Playwright** | Backend hiện dùng JUnit Jupiter **6.0.3** theo Spring Boot 4.1.2; các phiên bản khác xem mục 3 hoặc còn mở | Giữ các họ công cụ đã chọn; JUnit theo dependency management của Boot 4.1.2 |
| 14 | Deployment | **Docker Compose + VPS + Nginx** | Nhà cung cấp VPS, OS, domain, TLS: chưa chốt | **ĐÃ CHỐT mô hình** |
| 15 | API & Architecture | **REST API + Modular Monolith** | Quy chuẩn endpoint/versioning: chưa chốt | **ĐÃ CHỐT** |
| 16 | Authorization | **RBAC + Resource-level Authorization** | Ma trận quyền cụ thể: chưa chốt | **ĐÃ CHỐT phương pháp** |

**Định hướng công cụ bảo mật:** Sử dụng **Spring Security** để hiện thực Session Authentication và kiểm tra quyền ở backend. Đây là hướng triển khai gắn với công nghệ đã chọn; chưa chốt phiên bản độc lập hoặc chính sách bảo mật cụ thể.

**Điều chỉnh cuối cùng có hiệu lực:**

1. Cache đổi từ `Spring Cache + Caffeine` → **`Spring Cache + Redis`** theo yêu cầu trực tiếp của người dùng.
2. Avatar chọn **Cloudinary** thay vì lưu file cục bộ tại server.
3. Chốt các lựa chọn 12–16 đồng thời: **A / A / A / A / C**.
4. Ngày **02/11/2026**, người dùng cho phép đánh giá phiên bản starter, điều chỉnh source/tài liệu cho thống nhất: giữ **Spring Boot 4.1.2 + Java 21**, nâng **Angular 21.2.x → 22.2.4** theo major đã chọn. Angular CLI/build **22.2.5**, TypeScript **6.0.3** được chọn bởi migration chính thức. JUnit Jupiter **6.0.3** là dependency được Boot 4.1.2 quản lý sẵn, thay thông tin JUnit 5 trong tài liệu ban đầu.

---

## 2. Kiến trúc tổng thể đã chọn

```text
                  Người dùng trên trình duyệt
                            |
                            v
                 +---------------------+
                 | Nginx (Reverse Proxy)|
                 | HTTPS / điều phối URL|
                 +----------+----------+
                            |
              +-------------+-------------+
              |                           |
              v                           v
      +---------------+          +-------------------+
      | Angular 22    |   HTTP   | Spring Boot 4.1.2 |
      | Student       | <------> | REST API          |
      | Lecturer      | Session  | Modular Monolith  |
      | Admin         | Cookie   | Java 21           |
      +---------------+          +----+---------+----+
                                      |         |
                     +----------------+         +------------------+
                     |                |                            |
                     v                v                            v
              +-------------+  +--------------+             +-------------+
              | PostgreSQL  |  | Redis cache  |             | Cloudinary  |
              | 18          |  | Spring Cache |             | Avatar     |
              +-------------+  +--------------+             +-------------+

        Docker Compose + VPS cho các dịch vụ tự triển khai;
        Cloudinary là dịch vụ ngoài, không chạy trong Compose.
```

**Modular Monolith** nghĩa là **một ứng dụng backend** triển khai thống nhất, phân chia module theo nghiệp vụ. Cấu trúc workspace đã được duyệt ngày 02/11/2026; quy ước package/module và thời điểm tạo nằm trong `PROJECT_STRUCTURE.md`. Không tự tách microservices.

**Ranh giới trách nhiệm:**

- **Angular:** điều hướng, biểu mẫu, hiển thị bảng và biểu đồ, trạng thái Auto Save, trải nghiệm người dùng; không trực tiếp quyết định phân quyền hay kết quả tính điểm cuối cùng.
- **Spring Boot:** API, validation, xác thực/phân quyền, nghiệp vụ tính điểm và đăng ký học phần, transaction, audit, import/export, làm việc với Redis/Cloudinary.
- **PostgreSQL:** dữ liệu chính thức, quan hệ/constraints, transaction, lịch sử thay đổi.
- **Redis:** cache dữ liệu đọc phù hợp, **không** là nguồn sự thật cho sĩ số còn lại hoặc điểm số.
- **Cloudinary:** lưu và cung cấp avatar; PostgreSQL lưu mã định danh/tham chiếu ảnh theo thiết kế dữ liệu sau này.

---

## 3. Kiểm tra phiên bản và rủi ro (02/11/2026)

### 3.1. Bộ phiên bản đang áp dụng

| Thành phần | Đã chọn | Kết quả xác minh / lưu ý |
|---|---|---|
| Angular | **22.2.4**, CLI/build **22.2.5** | Angular 22 đang được hỗ trợ Active. Starter đã nâng từ 21.2.x bằng migration chính thức; dependency được khóa trong `frontend/package-lock.json`. |
| Java | **21 LTS** | Toolchain trong `backend/build.gradle`; tương thích Spring Boot 4.1.2. |
| Spring Boot | **4.1.2** | Bản stable đã phát hành; giữ starter hiện có thay vì hạ về nhánh 3.5.x hết hỗ trợ OSS. |
| PostgreSQL | **18** | Phiên bản chính thức, còn được hỗ trợ. |

**Cơ sở điều chỉnh:** Angular 21 vẫn trong LTS, nhưng Angular 22 đang Active và có thời gian hỗ trợ dài hơn; với starter chưa có module nghiệp vụ, nâng lên major 22 đã chốt giúp thống nhất nền tảng ngay từ đầu. Spring Boot 4.1.2 là bản stable, hỗ trợ Java 21 và Gradle 9.x. Ngược lại, thông báo chính thức ngày **25/06/2026** xác nhận **3.5.16 là bản OSS cuối cùng của nhánh 3.5.x**.

- Quyết định giữ Boot 4.1.2 áp dụng theo ủy quyền đánh giá/cập nhật phiên bản của người dùng ngày 02/11/2026; lịch sử lựa chọn ban đầu được giữ ở D16.
- Backend hiện chỉ có starter và starter-test. Spring Security, JPA, Flyway, Redis, POI chưa được tích hợp; phải kiểm tra tương thích với Boot 4.1.2 khi triển khai từng phần. Không coi việc build starter thành công là đã kiểm chứng toàn bộ stack.
- Backend dùng **Gradle Wrapper 9.8.2**; frontend dùng **npm 11.19.1** theo manifest hiện có. Môi trường kiểm tra: **Node.js 24.22.0**, **JDK 21.0.9** qua `JAVA_HOME`.
- JUnit Jupiter **6.0.3**, Mockito **5.23.0** do dependency management của Boot 4.1.2 quản lý, đã kiểm tra bằng Gradle `dependencyInsight`. Không ép về JUnit 5 độc lập với BOM.

### 3.2. Tương thích thư viện frontend

- Angular 22 theo bảng tương thích chính thức và package metadata yêu cầu **Node.js `^22.22.3` hoặc `^24.15.0` hoặc `>=26.0.0`**; sử dụng nhánh **Node.js 24 LTS** cho workspace. Frontend đã nâng lên **TypeScript 6.0.3**; RxJS giữ trong nhánh **7.8.x** hiện có. Phiên bản resolved chi tiết nằm trong lockfile.
- Angular Material/CDK: ưu tiên **cùng major 22** với Angular đã chọn; không tự dùng phiên bản prerelease.
- AG Grid: bảng tương thích chính thức ghi **Angular 20–22** hoạt động với **AG Grid 28–36+**. **AG Grid 36** là ứng viên triển khai; phiên bản patch chưa được chốt.
- ng2-charts: bảng tương thích của dự án ghi **Angular 22 ↔ ng2-charts 11.x**. Chart.js phải chọn phiên bản peer dependency phù hợp khi cài.
- **AG Grid Community:** có chỉnh sửa ô, lọc, sắp xếp, phân trang và CSV export; **Excel `.xlsx` export tích hợp, range selection, một số clipboard operations là tính năng Enterprise**, không được giả định có trong bản Community. Tính năng xuất Excel chính thức của EduCore đã chốt **xử lý qua Apache POI ở backend**.

### 3.3. Database migration / ORM

- Flyway hỗ trợ PostgreSQL 18; khi tích hợp Spring Boot cần kiểm tra và khai báo **module Flyway hỗ trợ PostgreSQL** phù hợp với dependency management (`flyway-database-postgresql`).
- **ĐỀ XUẤT CHƯA CHỐT:** sử dụng Flyway làm nguồn quản lý schema và cấu hình Hibernate `ddl-auto=validate` thay vì `update`/`create` trong môi trường dùng dữ liệu thực.
- Không tự cập nhật riêng Hibernate, Flyway hoặc Spring Security vượt phiên bản tương thích của Spring Boot mà chưa kiểm tra.

---

## 4. Hợp đồng kỹ thuật bắt buộc từ yêu cầu và Design System

Đây là **ràng buộc phải đáp ứng**, không phải lựa chọn thư viện mới:

### 4.1. Bảo mật / phân quyền

- Ba vai trò: **Admin, Giảng viên, Sinh viên**.
- **RBAC** để xét vai trò, kết hợp **resource-level authorization**: sinh viên chỉ được xem dữ liệu học tập của mình, giảng viên chỉ thao tác đúng lớp được phân công; kiểm tra ở backend.
- Không lưu mật khẩu plaintext; sử dụng bộ băm mật khẩu an toàn. Thuật toán/cost cụ thể **chưa chốt**.
- Session ID ở cookie; triển khai cookie an toàn (`HttpOnly`, `Secure`, `SameSite` tùy luồng), HTTPS, bảo vệ CSRF phù hợp cho thao tác thay đổi dữ liệu, quản lý đăng xuất và hết hạn phiên. **Giá trị cấu hình chính xác còn mở**.

### 4.2. Điểm / nhập điểm / Auto Save

- Điểm thành phần trong **[0, 10]**; tổng trọng số đầu điểm **100%**; tổng kết **làm tròn 2 chữ số**; thiếu đầu điểm bắt buộc phải hiển thị **“Chưa đủ điểm”**.
- Xếp loại mặc định: **Giỏi ≥8.50; Khá ≥7.00 và <8.50; Trung bình ≥5.00 và <7.00; Không đạt <5.00**. Cấu hình nghiệp vụ chi tiết, thời điểm làm tròn còn cần thiết kế.
- AG Grid Community là **phần UI**, không thay thế kiểm tra phía backend.
- Bảng nhập điểm cần hỗ trợ sửa theo ô, tìm kiếm/lọc, trạng thái lưu **Dirty / Saving / Saved / Validation Error / Network Error / Conflict**, khôi phục hoặc retry khi lỗi, và **lưu lịch sử ai sửa, thời điểm, giá trị trước/sau**.
- Design System đề xuất debounce **khoảng 650ms** và hành vi bàn phím kiểu spreadsheet; độ trễ và cơ chế khóa phiên bản dữ liệu cần hiện thực/kiểm thử, không tự coi là đã quyết định backend cụ thể.
- Giao diện mobile theo thiết kế **M2: sửa điểm theo từng sinh viên**, không thu nhỏ nguyên Excel grid.

### 4.3. Đăng ký học phần đồng thời

- Backend kiểm tra học kỳ, thời gian đăng ký, trạng thái lớp, không đăng ký trùng lớp/môn, sĩ số, trùng lịch, giới hạn tín chỉ, học phần tiên quyết.
- Khi hai sinh viên đăng ký chỗ cuối cùng, không được vượt sĩ số. Phải thiết kế **transaction và cơ chế kiểm soát đồng thời** tại database/backend; **chưa chốt cụ thể optimistic/pessimistic locking**.
- Không dựa vào cache Redis để kiểm tra hoặc bảo đảm sĩ số còn lại.

### 4.4. Dữ liệu / xuất nhập / audit

- Tất cả bảng dữ liệu phải có trường audit theo yêu cầu: `created_at`, `created_by`, `last_modified_date`, `last_modified_by` (trừ khi sau này có thay đổi yêu cầu được xác nhận).
- Thiết kế foreign key, unique, check constraint và transaction theo nghiệp vụ; quản lý schema bằng Flyway.
- Import CSV/Excel cần trả lỗi theo dòng, kiểm tra dữ liệu và quyền trước khi ghi; xuất file theo phạm vi quyền truy cập.
- PostgreSQL là nguồn dữ liệu chính; Redis cần chiến lược **eviction/invalidation** khi dữ liệu gốc thay đổi.

### 4.5. UI/UX đã khóa

- Tuân thủ Design System chính thức **Campus Studio — Modern, Warm & Professional**.
- 5 màu thương hiệu đã chốt: primary `#14998F`, accent `#E98A72`, canvas `#FAF7F1`, muted surface `#ECE8E0`, text `#2B3430`; font **Be Vietnam Pro**.
- Primary button dùng màu chức năng `#0F706A`; tránh dùng trực tiếp brand teal sáng với chữ trắng nhỏ thiếu tương phản.
- Student dùng Top Navigation; Lecturer dùng Hybrid Sidebar; Admin dùng Classic Sidebar; trạng thái loading/empty/error và 4 loại toast phải đầy đủ.
- Angular Material/CDK, AG Grid và Chart.js đều phải được tùy biến **theo token và hành vi đã khóa**; không lấy theme mặc định làm chuẩn thay thế.

---

## 5. Kiểm thử đã chốt (nhóm 12)

| Phạm vi | Công cụ đã chọn | Những điều cần chứng minh |
|---|---|---|
| Angular unit/component | **Vitest** | Component, service, form validation, trạng thái Auto Save, lỗi hiển thị |
| Backend unit | **JUnit Jupiter 6.0.3 + Mockito** (Boot 4.1.2 quản lý) | Tính điểm, trọng số, chính sách đăng ký, kiểm tra quyền |
| Integration test | **Testcontainers** (kết hợp test Spring) | PostgreSQL thực, transaction, unique/constraint, đăng ký đồng thời; Redis khi cần |
| E2E web | **Playwright** | Login theo vai trò; thao tác bảng điểm; import/export; lỗi/khôi phục UI |

Đây là **bộ công cụ**, chưa chốt mức coverage, cách chạy CI/CD, môi trường staging hoặc quy trình phát hành.

---

## 6. Deployment đã chốt (nhóm 13)

**ĐÃ CHỐT mô hình:** **Docker Compose + VPS + Nginx**. Cloudinary là dịch vụ ngoài hệ thống; Redis và PostgreSQL thuộc các thành phần triển khai do đội dự án quản lý.

**ĐỀ XUẤT triển khai:** dùng Nginx làm reverse proxy, phục vụ Angular và chuyển tiếp đường dẫn API cùng origin tới Spring Boot để đơn giản hóa cookie/session và CSRF; cấu hình HTTPS, network nội bộ Docker, volume bền vững cho PostgreSQL và backup định kỳ. Các chi tiết này là hướng kỹ thuật, **chưa chốt** domain, nhà cung cấp VPS, chứng chỉ TLS, chiến lược backup cụ thể hay số instance.

---

## 7. Danh sách quyết định còn mở — không tự chốt

| Nhóm | Nội dung cần quyết định khi đến bước triển khai | Thời điểm phù hợp |
|---|---|---|
| **Tương thích backend** | Kiểm tra Spring Security, JPA, Flyway và các tích hợp trên Boot 4.1.2; không dùng cấu hình Boot 3.x một cách máy móc | Khi tích hợp từng thư viện |
| **Phiên bản patch** | Angular core 22.2.4, CLI/build 22.2.5 và Boot 4.1.2 đã áp dụng; patch Material/CDK, PostgreSQL, AG Grid, ng2-charts, Chart.js, Flyway, Redis, POI, Commons CSV, Testcontainers/Playwright còn mở | Trước khi cài từng package |
| **Môi trường** | Gradle 9.8.2, npm 11.19.1, Node 24 LTS và TypeScript 6.0.3 đã ghi nhận; môi trường CI/container và lịch cập nhật patch còn mở | Khi thiết lập CI/deployment |
| **Security** | Chính sách mật khẩu, thời hạn session, nhớ đăng nhập, đăng xuất mọi thiết bị, CSRF token, cookie SameSite, chính sách khóa tài khoản | Khi thiết kế auth |
| **Session storage** | Lưu phiên theo cơ chế server mặc định hay sử dụng Redis/Spring Session; **Redis cache đã chốt không đồng nghĩa Redis Session** | Khi thiết kế auth/deploy |
| **Phân quyền chi tiết** | Ma trận quyền CRUD theo vai trò, ai được sửa điểm khóa, mở khóa và xem lịch sử | Khi đặc tả nghiệp vụ |
| **Mô hình dữ liệu** | Phân biệt lớp hành chính/lớp học phần, học kỳ, tiên quyết, chính sách xóa/khóa điểm, quy tắc làm tròn chính xác | Trước ERD/migration |
| **Điểm đồng thời** | Optimistic locking/version hay giải pháp khác; chiến lược retry/error/conflict cho Auto Save | Khi thiết kế grade API |
| **Đăng ký đồng thời** | Kiểu locking, isolation, transaction boundary và test race condition | Khi thiết kế enrollment API |
| **Cache Redis** | TTL, key naming, cache invalidation, cấu hình memory/failure, có hay không cache riêng dữ liệu nhạy cảm | Khi tối ưu các API cụ thể |
| **Cloudinary** | Chính sách public/private asset, định dạng/kích thước avatar, quyền sửa/xóa, quota/tối ưu | Khi xây dựng avatar |
| **API** | Đặt tên route, chuẩn mã lỗi, DTO, phân trang, versioning, tài liệu OpenAPI nếu cần | Khi thiết kế API contract |
| **Import/export** | Template file, định dạng lỗi theo dòng, partial/all-or-nothing import, giới hạn dung lượng | Khi thiết kế nghiệp vụ nhập file |
| **Triển khai** | Nhà cung cấp VPS, OS, domain, HTTPS, secrets, backup/restore, monitoring, CI/CD | Khi chuẩn bị vận hành |
| **Testing** | Coverage mục tiêu, test data, chiến lược chạy CI, kiểm thử accessibility và hiệu năng | Khi lập test plan |

**Không cần mở thêm vòng chọn thư viện** cho những điều không ảnh hưởng thiết kế hiện tại; giải quyết từng mục khi thật sự triển khai tới phần đó. Nếu chọn công nghệ mới hoặc đổi quyết định đã khóa, phải ghi decision log và xin xác nhận.

---

## 8. Decision log

| ID | Quyết định / thay đổi | Trạng thái |
|---|---|---|
| D01 | Spring Boot làm backend | ĐÃ CHỐT |
| D02 | PostgreSQL làm database | ĐÃ CHỐT |
| D03 | Session-based Authentication + Cookie | ĐÃ CHỐT |
| D04 | Spring Data JPA + Hibernate | ĐÃ CHỐT |
| D05 | Flyway | ĐÃ CHỐT |
| D06 | Angular Material + Angular CDK | ĐÃ CHỐT |
| D07 | AG Grid Community | ĐÃ CHỐT |
| D08 | Chart.js + ng2-charts | ĐÃ CHỐT |
| D09 | Apache POI + Apache Commons CSV, xử lý backend | ĐÃ CHỐT |
| D10 | **Spring Cache + Redis**, thay Caffeine trước đây | ĐÃ CHỐT / CÓ THAY ĐỔI |
| D11 | Cloudinary lưu avatar | ĐÃ CHỐT |
| D12 | Testing A: Vitest + JUnit/Mockito + Testcontainers + Playwright | ĐÃ CHỐT; phiên bản JUnit cập nhật theo D17 |
| D13 | Deployment A: Docker Compose + VPS + Nginx | ĐÃ CHỐT |
| D14 | Architecture A: REST API + Modular Monolith | ĐÃ CHỐT |
| D15 | Authorization A: RBAC + Resource-level Authorization | ĐÃ CHỐT |
| D16 | Lựa chọn ban đầu: Angular 22 + Java 21 LTS + Spring Boot 3.5.x + PostgreSQL 18 | LỊCH SỬ; Spring Boot được điều chỉnh theo D17 |
| D17 | 02/11/2026: theo ủy quyền đánh giá phiên bản, giữ Boot 4.1.2 + Java 21; nâng starter Angular lên 22.2.4, CLI/build 22.2.5, TypeScript 6.0.3; JUnit Jupiter 6.0.3 theo BOM Boot | ĐÃ ÁP DỤNG; kiểm chứng xem `PROJECT_PROGRESS.md` |
| D18 | 02/11/2026: workspace chung frontend/backend/docs/infra, backend trực tiếp trong `backend/`, Git tại gốc và CodeGraph cho workspace | ĐÃ DUYỆT VÀ ÁP DỤNG; chi tiết xem `PROJECT_STRUCTURE.md` |

---

## 9. Nguồn chính thức được đối chiếu (02/11/2026)

Nguồn bên dưới để **kiểm chứng phiên bản/khả năng tương thích**, không được dùng để tự thay đổi lựa chọn đã chốt.

1. Angular — Version compatibility: https://angular.dev/reference/versions
2. Angular — Release schedule/support policy: https://angular.dev/reference/releases
3. Angular Components (Material/CDK) — Releases: https://github.com/angular/components/releases
4. Spring Boot 3.5.16 — **Last OSS release of 3.5.x**: https://spring.io/blog/2026/06/25/spring-boot-3-5-16-available-now/
5. Spring Boot 4.1 — Java/Gradle system requirements: https://docs.spring.io/spring-boot/4.1/system-requirements.html
6. PostgreSQL — Versioning policy: https://www.postgresql.org/support/versioning/
7. AG Grid — Angular compatibility: https://www.ag-grid.com/angular-data-grid/compatibility/
8. AG Grid — Community/Enterprise feature comparison: https://github.com/ag-grid/ag-grid/blob/latest/README.md
9. AG Grid — Excel export (Enterprise): https://www.ag-grid.com/javascript-data-grid/excel-export/
10. ng2-charts — Angular compatibility: https://www.npmjs.com/package/ng2-charts
11. Flyway — PostgreSQL driver/module: https://github.com/flyway/flyway/blob/main/documentation/Reference/Database%20Driver%20Reference/PostgreSQL%20Database.md
12. Spring Security — CSRF documentation: https://docs.spring.io/spring-security/reference/features/exploits/csrf.html
13. Angular — Testing with Vitest: https://angular.dev/guide/testing
14. Spring Boot — Database initialization & Flyway: https://docs.spring.io/spring-boot/how-to/data-initialization.html
15. Spring Boot 4.1.2 — Stable release: https://spring.io/blog/2026/10/23/spring-boot-4-1-2-available-now/

---

## 10. Phạm vi và cách cập nhật tài liệu

- `PROJECT_REQUIREMENTS.md` giữ nguyên nghiệp vụ gốc.
- `EduCore_Design_System_v1.0.md` giữ nguyên thiết kế đã khóa.
- **`TECH_STACK.md` là nguồn quyết định kỹ thuật chính thức** sau khi người dùng đưa vào Project Sources. Nội dung **chưa** khẳng định các gói đã được cài hay ứng dụng đã chạy thành công.
- Khi điều chỉnh nhánh Spring Boot, cập nhật **mục 1, mục 2, mục 3, mục 7, mục 8** và tăng phiên bản tài liệu. Bản 1.1.0 đã ghi nhận việc giữ Boot 4.1.2 theo ủy quyền ngày 02/11/2026.
- Khi bắt đầu cài đặt, lưu thêm bản patch thực tế và lockfile/dependency manifest; kiểm tra lại version compatibility trước khi xác nhận môi trường đã hoạt động.
