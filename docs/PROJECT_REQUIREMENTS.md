# PROJECT REQUIREMENTS — WEBSITE QUẢN LÝ ĐIỂM SINH VIÊN

> **Nguồn:** Tổng hợp, chuẩn hóa từ đề bài thực hành do người dùng cung cấp.  
> **Vai trò:** Tài liệu yêu cầu gốc (source of truth) cho phân tích nghiệp vụ, thiết kế, lập trình và kiểm thử.  
> **Lưu ý:** Những quyết định kỹ thuật hoặc nghiệp vụ chưa được đề bài xác định cần được thảo luận và ghi nhận riêng; không tự coi là đã chốt.

## 1. Mục tiêu và phạm vi

Xây dựng ứng dụng web quản lý hoạt động học tập, bao gồm:

- Quản lý thông tin **sinh viên**, **môn học**, **lớp học phần** và **các đầu điểm thành phần**.
- Cho phép sinh viên **đăng ký/hủy đăng ký học phần** theo các quy tắc nghiệp vụ.
- Nhập, cập nhật, tự động tính **điểm tổng kết**, **xếp loại** và tra cứu kết quả học tập.
- Cung cấp **dashboard**, **báo cáo**, **thống kê** và **nhập/xuất dữ liệu**.
- Có **đăng nhập, phân quyền**, truy vết thay đổi, phản hồi trạng thái và các tối ưu cần thiết.

### 1.1. Nền tảng

- **Frontend:** Angular (bắt buộc theo đề bài).
- **Backend:** Tự chọn.
- **Cơ sở dữ liệu:** Tự chọn.
- Công nghệ backend, database, thư viện và phiên bản chính thức sẽ được quyết định trong `TECH_STACK.md`.

### 1.2. Vai trò người dùng

| Vai trò | Nghiệp vụ chính |
| --- | --- |
| **Quản trị viên (Admin)** | Quản lý dữ liệu; tạo/sửa/mở/đóng lớp học phần; cấu hình sĩ số, lịch học, thời gian đăng ký; xem danh sách người đăng ký; quản trị hệ thống. |
| **Giảng viên** | Xem sinh viên ở các lớp mình phụ trách; thực hiện nghiệp vụ nhập/quản lý điểm trong phạm vi được phân quyền. |
| **Sinh viên** | Đăng nhập, quản lý đăng ký học phần, xem các lớp đang theo học và kết quả học tập của bản thân. |

> Phạm vi quyền chi tiết (ví dụ ai được xóa/sửa điểm đã khóa) cần được chốt khi thiết kế phân quyền.

## 2. Quy tắc tính điểm

- Mỗi môn học có **một hoặc nhiều đầu điểm** và **trọng số** tương ứng.
- Ví dụ: **Chuyên cần 10% + Giữa kỳ 30% + Cuối kỳ 60%**.
- Điểm ở từng đầu điểm nằm trong **[0, 10]**.
- Tổng trọng số các đầu điểm của một môn phải bằng **100%**.
- Điểm tổng kết được tính theo trọng số và **làm tròn đến 2 chữ số thập phân**.
- Nếu còn đầu điểm chưa được nhập, kết quả hiển thị **“Chưa đủ điểm”**.
- Ngưỡng xếp loại có thể cấu hình; mặc định:

| Điểm tổng kết | Xếp loại |
| --- | --- |
| Từ 8.50 đến 10.00 | Giỏi |
| Từ 7.00 đến dưới 8.50 | Khá |
| Từ 5.00 đến dưới 7.00 | Trung bình |
| Dưới 5.00 | Không đạt |

- Không được nhập hoặc lưu điểm nằm ngoài khoảng hợp lệ.
- Phải có chức năng **xem chi tiết cách tính điểm** của từng sinh viên.

## 3. Quản lý sinh viên

- Xem danh sách sinh viên.
- Thêm, sửa, xóa thông tin sinh viên.
- Tìm kiếm theo **mã sinh viên** hoặc **họ tên**; hỗ trợ **tìm kiếm có gợi ý**.
- Xem trang chi tiết sinh viên và kết quả học tập theo từng môn.
- Thông tin tối thiểu:
  - Mã sinh viên (**duy nhất**).
  - Họ tên.
  - Email.
  - Ngày sinh.
  - Lớp.
  - Trạng thái.
  - **Avatar** sinh viên.
- Có khả năng tạo tài khoản sinh viên.
- Sinh viên đăng nhập để xem **điểm của chính mình** và **lớp đang theo học**.

## 4. Đăng ký học phần

### 4.1. Chức năng cho sinh viên

- Xem các lớp học phần **đang mở đăng ký**.
- Tìm kiếm/lọc theo **mã môn, tên môn, giảng viên, học kỳ hoặc lịch học**.
- Xem chi tiết lớp học phần và **số chỗ còn lại**.
- Đăng ký/hủy đăng ký trong **thời gian cho phép**.
- Xem các học phần đã đăng ký và **trạng thái đăng ký**.
- Hiển thị thông báo rõ ràng khi đăng ký **thành công hoặc không thành công**.

### 4.2. Quy tắc đăng ký bắt buộc

1. Không được đăng ký trùng **cùng một lớp học phần**.
2. Không được đăng ký **nhiều lớp của cùng một môn trong cùng học kỳ**, trừ trường hợp có quy định khác.
3. Không được đăng ký khi lớp **đủ sĩ số**, **đóng đăng ký** hoặc **ngoài thời gian đăng ký**.
4. Phải kiểm tra **xung đột lịch học** với các lớp đã đăng ký.
5. Hệ thống có thể cấu hình **giới hạn tín chỉ tối thiểu/tối đa theo học kỳ**.
6. Khi môn học có **học phần tiên quyết**, sinh viên phải đạt học phần tiên quyết trước khi đăng ký.
7. **Tất cả kiểm tra nghiệp vụ bắt buộc được thực thi tại backend**, không chỉ ở Angular.

### 4.3. Chức năng cho Admin và giảng viên

- **Admin** được tạo, sửa, mở/đóng lớp học phần; cấu hình sĩ số, lịch học, thời gian đăng ký.
- **Admin** xem danh sách sinh viên đăng ký từng lớp học phần.
- **Giảng viên** xem danh sách sinh viên trong các lớp mình phụ trách.
- Số chỗ còn lại phải cập nhật **chính xác khi đăng ký/hủy**, kể cả khi **nhiều yêu cầu đồng thời**; không được vượt sĩ số.

## 5. Quản lý điểm

- Chọn sinh viên và môn học/lớp học phần phù hợp để nhập điểm.
- Nhập và cập nhật điểm cho từng đầu điểm.
- Tự động tính điểm tổng kết và xếp loại.
- Nếu thiếu điểm: trạng thái **“Chưa đủ điểm”**.
- Lọc kết quả theo **môn học**, **lớp**, **đạt/không đạt**.
- Xem chi tiết cách tính điểm cho từng sinh viên.
- Validate điểm trong **[0, 10]**.
- **Giao diện nhập điểm giống thao tác bảng tính Excel** (ưu tiên nhập nhanh trên lưới dữ liệu).
- **Auto Save:** thay đổi điểm được tự động lưu mà không cần nhấn nút Lưu.
- **Lưu lịch sử thay đổi điểm** để truy vết.

> Những hành vi như debounce, thao tác bàn phím, xử lý lỗi lưu hoặc sửa điểm đồng thời cần được thiết kế và chốt cụ thể để đáp ứng Auto Save an toàn.

## 6. Báo cáo và thống kê

Màn hình báo cáo tối thiểu có:

- Tổng **số lượng sinh viên**.
- **Điểm trung bình theo môn học**.
- **Số sinh viên đạt và không đạt**.
- **Danh sách sinh viên theo khoảng điểm**.
- **Phân trang và tìm kiếm** trên danh sách kết quả.
- **Biểu đồ phân bố điểm**.

## 7. Giao diện Angular

### 7.1. Các trang bắt buộc theo đề bài

| Trang | Chức năng |
| --- | --- |
| Dashboard | Các số liệu tổng quan. |
| Sinh viên | Danh sách, tìm kiếm, thêm/sửa thông tin. |
| Môn học | Danh sách và cấu hình đầu điểm/trọng số. |
| Nhập điểm | Chọn sinh viên, môn và nhập/sửa điểm. |
| Báo cáo | Bộ lọc, bảng và biểu đồ thống kê. |
| Chi tiết sinh viên | Thông tin cá nhân và bảng điểm theo môn. |

### 7.2. Màn hình phát sinh từ các chức năng khác

Cần thiết kế thêm luồng/màn hình tương ứng cho:

- Đăng nhập và quyền truy cập theo vai trò.
- Danh sách/chi tiết/đăng ký/hủy đăng ký lớp học phần.
- Quản trị lớp học phần, sĩ số, lịch học, thời gian đăng ký.
- Sinh viên xem học phần/lớp đang theo học và bảng điểm của mình.
- Nhập/xuất dữ liệu và xem lịch sử thay đổi điểm (có thể là trang, tab hoặc dialog).

### 7.3. Quy chuẩn kỹ thuật cho Angular

- Sử dụng **Angular Router** để điều hướng.
- Dùng **Reactive Forms** cho biểu mẫu.
- Có thông báo khi lưu thành công hoặc xảy ra lỗi.
- Có trạng thái **loading**, **dữ liệu rỗng** và **lỗi tải dữ liệu**.
- Tách **component**, **service**, **model** hợp lý.
- **Không gọi API trực tiếp trong component**.

## 8. Đăng nhập, bảo mật và phân quyền

- Có đăng nhập và phân quyền **Admin / Giảng viên / Sinh viên**.
- Sinh viên chỉ được tra cứu điểm của **chính mình**; giảng viên truy cập dữ liệu lớp mình phụ trách theo quyền.
- **Không lưu mật khẩu dạng plaintext**; dùng thuật toán **băm mật khẩu an toàn**.
- Các kiểm tra quyền và nghiệp vụ quan trọng phải nằm ở **backend**.

## 9. Import/Export, tìm kiếm và thông báo

- Nhập và xuất **danh sách sinh viên** hoặc **bảng điểm** bằng **CSV/Excel**.
- **Tìm kiếm có gợi ý**.
- Có **loading** trong thao tác bất đồng bộ.
- Dùng **toastr** với 4 loại: **Thành công, Lỗi, Cảnh báo, Thông tin**.

## 10. Audit, hiệu năng và tính nhất quán dữ liệu

- **Tất cả bảng dữ liệu** cần có các trường audit theo yêu cầu:
  - `created_at` — Created At.
  - `created_by` — Created By.
  - `last_modified_date` — Last Modified Date.
  - `last_modified_by` — Last Modified By.
- Lưu **lịch sử thay đổi điểm** (ngoài thông tin audit chung).
- Sử dụng **cache** để giảm tải và cải thiện thời gian tải dữ liệu.
- Đảm bảo tính chính xác của **số chỗ còn lại** khi đăng ký/hủy đăng ký đồng thời.
- Nhập điểm **Auto Save**, hỗ trợ thao tác tương tự Excel và xử lý cập nhật dữ liệu an toàn.

## 11. Kiểm tra nghiệm thu tổng quát

Dự án được coi là đáp ứng đề bài khi chứng minh được:

- Các vai trò đăng nhập đúng quyền; sinh viên không xem được điểm người khác.
- Quản lý được sinh viên, môn học, cấu hình đầu điểm và lớp học phần.
- Các quy tắc đăng ký học phần được backend thực thi; không vượt sĩ số khi đồng thời.
- Nhập điểm trong giới hạn; tự tính điểm/trạng thái/xếp loại đúng trọng số.
- Nhập điểm qua giao diện lưới và tự động lưu; có lịch sử sửa điểm.
- Dashboard, báo cáo, bộ lọc, phân trang và biểu đồ hoạt động.
- Nhập/xuất CSV/Excel và tìm kiếm có gợi ý hoạt động.
- Có đủ trạng thái loading/empty/error và 4 loại toastr.
- Các bảng dữ liệu có thông tin audit và có cơ chế cache hợp lý.

## 12. Những nội dung cần chốt trong quá trình thiết kế

Đề bài **chưa quyết định chi tiết** các mục sau, không được tự coi là yêu cầu cố định:

- Framework/backend, database, phiên bản và các thư viện.
- Cách quản lý tài khoản, xác thực (session/token), ma trận phân quyền chi tiết.
- Phân biệt **lớp hành chính** và **lớp học phần**; mô hình học kỳ, lịch học.
- Môn học tiên quyết, quy định đạt học phần, chính sách tín chỉ; quy tắc ngoại lệ đăng ký.
- Chính sách xóa, khóa bảng điểm, quyền sửa điểm; làm tròn trong từng bước tính.
- Trải nghiệm nhập điểm kiểu Excel, debounce Auto Save, xử lý lỗi/xung đột.
- Định dạng CSV/Excel, chính sách import lỗi, nơi lưu avatar.
- Giải pháp cache, thời hạn cache và cách làm mới dữ liệu.
- Thiết kế giao diện và tiêu chuẩn responsive/accessibility cụ thể.

Các quyết định được thống nhất sẽ lưu trong tài liệu riêng (đặc biệt `TECH_STACK.md`, `DESIGN_SYSTEM.md`), tránh sửa mất yêu cầu gốc.
