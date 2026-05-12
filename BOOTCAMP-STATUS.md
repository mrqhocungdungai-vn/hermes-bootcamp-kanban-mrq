# BOOTCAMP-STATUS

## Current phase
- Track: Hermes Kanban + Agent Team Foundations
- Language: Vietnamese
- Delivery mode: repo-backed docs for practice on trainee's other machine
- Benchmark project: TodoApp
- Tech stack: Next.js fullstack (App Router) + TypeScript + SQLite
- Status: READY FOR DAY 2

## Current lesson
- File: `docs/day2.md`
- Topic: durable handoff artifact + review loop cho TodoApp slice trên repo/path thật
- Objective: trainee hiểu reviewer findings phải sống ở file + comment pointer + task history, không sống mơ hồ trong chat

## Preconditions trước khi trainee chạy Day 2 trên máy học

Trainee phải tự verify trên máy học:
- Day 1 đã PASS
- Hermes đã cài và chạy được
- model/provider đang hoạt động
- có thể chạy `hermes --version`
- có thể chạy `hermes profile list`
- có thể chạy `hermes kanban --help`
- có thể chạy `hermes kanban context --help`
- có thể truy cập board đã tạo từ Day 1

## Day 2 done khi nào?

Day 2 chỉ PASS nếu trainee có evidence cho đủ 6 thứ:
1. dùng lại được board và 3 profile role cơ bản: `pm`, `coder`, `reviewer`
2. tạo được pipeline TodoApp chạy với workspace `dir:<repo>`
3. đọc được ít nhất một lần output của `hermes kanban context`
4. reviewer có để lại hoặc được yêu cầu để lại file review bền vững trong repo
5. hiểu comment pointer khác source-of-truth file ở đâu
6. ghi xong `log/day2-observations.md`

## Deliverables trainee phải lưu lại

- đọc `docs/todoapp-benchmark.md` trước khi chạy lab
- với Day 1: ghi note vào `log/day1-observations.md`
- với Day 2: ghi note vào `log/day2-observations.md`
- nếu có lỗi, ghi nguyên nhân và command đã chạy
- nếu profile/board naming thay đổi, cập nhật lại file log cho đúng

## Next lesson sau Day 2

Dự kiến Day 3:
- `scratch` vs `dir:<repo>` vs `worktree`
- khi nào dùng workspace nào cho TodoApp benchmark
- vì sao repo thật tạo ra handoff/review loop khác sandbox tạm

## Coach note

Repo này phải luôn phản ánh sự thật hiện tại của bootcamp. Nếu lesson đổi, cập nhật file này trước.
