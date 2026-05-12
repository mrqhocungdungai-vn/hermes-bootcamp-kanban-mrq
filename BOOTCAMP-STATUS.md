# BOOTCAMP-STATUS

## Current phase
- Track: Hermes Kanban + Agent Team Foundations
- Language: Vietnamese
- Delivery mode: repo-backed docs for practice on trainee's other machine
- Benchmark project: TodoApp
- Tech stack: Next.js fullstack (App Router) + TypeScript + SQLite
- Status: READY FOR DAY 3

## Current lesson
- File: `docs/day3.md`
- Topic: chọn đúng workspace `scratch` vs `dir:<repo>` vs `worktree` cho TodoApp workflow
- Objective: trainee hiểu khi nào dùng workspace nào cho PM/reviewer/coder, và vì sao chọn sai workspace sẽ làm hỏng handoff hoặc coding flow

## Preconditions trước khi trainee chạy Day 3 trên máy học

Trainee phải tự verify trên máy học:
- Day 2 đã PASS
- Hermes đã cài và chạy được
- model/provider đang hoạt động
- có thể chạy `hermes --version`
- có thể chạy `hermes profile list`
- có thể chạy `hermes kanban create --help`
- có thể chạy `hermes kanban claim --help`
- có thể chạy `hermes kanban reclaim --help`
- nếu muốn test `worktree` thực chiến, TodoApp path phải là git repo thật

## Day 3 done khi nào?

Day 3 chỉ PASS nếu trainee có evidence cho đủ 6 thứ:
1. tạo được task mẫu với `scratch`, `dir:<repo>`, và `worktree`
2. dùng `claim`/`reclaim` để inspect ít nhất 2 workspace path hoặc intended path
3. hiểu `dir:<repo>` phải là absolute path
4. map được `pm`, `reviewer`, `coder` sang workspace hợp lý
5. hiểu `worktree` là coding isolation tool chứ không phải lựa chọn mặc định cho mọi task
6. ghi xong `log/day3-observations.md`

## Deliverables trainee phải lưu lại

- đọc `docs/todoapp-benchmark.md` trước khi chạy lab
- với Day 1: ghi note vào `log/day1-observations.md`
- với Day 2: ghi note vào `log/day2-observations.md`
- với Day 3: ghi note vào `log/day3-observations.md`
- nếu có lỗi, ghi nguyên nhân và command đã chạy
- nếu profile/board naming thay đổi, cập nhật lại file log cho đúng

## Next lesson sau Day 3

Dự kiến Day 4:
- thiết kế pipeline TodoApp chuẩn hơn cho `pm -> coder -> reviewer`
- khi nào coder follow-up nên tạo task mới thay vì sửa chồng lên task cũ
- cách chọn workspace xuyên suốt một review loop thật

## Coach note

Repo này phải luôn phản ánh sự thật hiện tại của bootcamp. Nếu lesson đổi, cập nhật file này trước.
