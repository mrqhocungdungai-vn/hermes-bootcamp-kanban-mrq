# TodoApp Benchmark của khóa học

## Mục đích

Khóa học này cần một project benchmark cố định để:
- mọi lesson nói về cùng một sản phẩm
- mọi role handoff có cùng ngữ cảnh
- trainee không bị mất thời gian đổi domain liên tục

Benchmark được chọn là **TodoApp**.

---

## Fixed tech stack

- Next.js fullstack (App Router)
- TypeScript
- SQLite

Đây là stack mặc định của khóa học. Khi viết lesson mới hoặc tạo demo mới, hãy bám stack này trừ khi có ghi chú khác.

---

## Vì sao chọn stack này?

### 1. Đủ nhỏ để học
TodoApp đủ đơn giản để trainee không bị chìm trong product complexity.

### 2. Đủ thật để demo agent team
Vẫn có đủ việc cho nhiều role:
- `pm` viết scope và acceptance criteria
- `coder` triển khai slice
- `reviewer` kiểm tra code và handoff

### 3. Fullstack nhưng không quá nặng
Next.js App Router + TypeScript cho phép demo UI, route, server action/handler, và data flow trong cùng một project.

### 4. SQLite giảm friction setup
Giai đoạn đầu không nên bắt trainee mất thời gian dựng DB server riêng chỉ để học Kanban và handoff.

---

## Product scope mức tối thiểu

TodoApp benchmark tối thiểu nên có:
- xem danh sách todo
- tạo todo mới
- toggle complete/incomplete
- xóa todo

Optional ở giai đoạn sau:
- filter all / active / completed
- edit title
- basic validation
- review artifact file

---

## Course usage

### Day 1
- không cần build full app hoàn chỉnh
- chỉ cần dùng TodoApp làm benchmark để hiểu Kanban, role, dependency, handoff

### Day 2+
- bắt đầu dùng TodoApp slice thật cho review loop
- dùng cùng benchmark để so sánh single-agent vs agent-team

---

## Default slice để demo nhanh

Nếu lesson cần một slice mặc định mà không muốn nghĩ thêm, dùng slice này:

**Todo create/list slice**

Acceptance criteria tối thiểu:
- người dùng thấy danh sách todo hiện tại
- người dùng tạo được todo mới
- todo mới xuất hiện ngay trong list
- dữ liệu được lưu bằng SQLite

---

## Rule cho các lesson sau

Khi lesson nói về demo app mà không nêu project khác, mặc định hiểu là:

> TodoApp — Next.js fullstack (App Router) + TypeScript + SQLite
