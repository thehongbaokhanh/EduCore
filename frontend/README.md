# EduCore — Frontend

Angular **22.2.4**, CLI/build **22.2.5**, TypeScript **6.0.3**; phiên bản resolved đầy đủ nằm trong `package-lock.json`.

## Chạy và build

Chạy trong thư mục `frontend/` với Node.js 24 LTS và npm theo `package.json`:

```powershell
npm ci
npm start
```

Dev server mặc định tại [localhost:4200](http://localhost:4200/).

```powershell
npm run build
npm test -- --watch=false
```

Build xuất ra `dist/frontend/`. Hiện lệnh test báo `No tests found`: file test starter đã bị xóa trước khi tổ chức workspace; chưa có test nghiệp vụ. Playwright chưa được cài.

## Tổ chức source

- `src/app/core/`: dịch vụ cấp ứng dụng, guards, interceptors.
- `src/app/layouts/`: khung trang.
- `src/app/shared/`: component, directive, pipe dùng chung.
- `src/app/features/`: chức năng theo nghiệp vụ; tạo module khi bắt đầu chức năng.
- `src/styles/`: tokens/theme khi có Design System gốc.
- `public/`: tài nguyên tĩnh.

Chi tiết xem [cấu trúc workspace](../docs/PROJECT_STRUCTURE.md), [tech stack](../docs/TECH_STACK.md) và [tiến độ](../docs/PROJECT_PROGRESS.md). Tuân thủ [AGENTS.md của frontend](AGENTS.md) và [quy tắc tại gốc](../AGENTS.md).
