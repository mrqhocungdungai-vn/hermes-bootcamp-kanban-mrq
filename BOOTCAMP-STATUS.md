# BOOTCAMP-STATUS

## Current phase
- Track: Hermes Kanban + Agent Team Foundations
- Language: Vietnamese
- Delivery mode: repo-backed docs for practice on trainee's other machine
- Benchmark project: TodoApp
- Tech stack: Next.js fullstack (App Router) + TypeScript + SQLite
- Status: READY FOR DAY 1

## Current lesson
- File: `docs/day1.md`
- Topic: Kanban vs Agent Team vs Goals + mini team lab `pm -> coder -> reviewer` cho TodoApp
- Objective: trainee tự tay dựng được board, role profiles, dependency pipeline tối thiểu, và map nó vào một TodoApp slice

## Preconditions trước khi trainee chạy Day 1 trên máy học

Trainee phải tự verify trên máy học:
- Hermes đã cài và chạy được
- model/provider đang hoạt động
- có thể chạy `hermes --version`
- có thể chạy `hermes profile list`
- có thể chạy `hermes kanban --help`

## Day 1 done khi nào?

Day 1 chỉ PASS nếu trainee có evidence cho đủ 5 thứ:
1. tạo được board học riêng
2. tạo được 3 profile role cơ bản: `pm`, `coder`, `reviewer`
3. tạo được pipeline task có dependency
4. quan sát được status flow ít nhất ở mức:
   - `todo`
   - `ready`
   - `running` hoặc `blocked` hoặc `done`
5. hiểu benchmark project cố định là TodoApp với stack đã chốt

## Deliverables trainee phải lưu lại

- đọc `docs/todoapp-benchmark.md` trước khi chạy lab
- ghi note vào `log/day1-observations.md`
- nếu có lỗi, ghi nguyên nhân và command đã chạy
- nếu profile/board naming thay đổi, cập nhật lại file log cho đúng

## Next lesson sau Day 1

Dự kiến Day 2:
- đọc handoff artifact đúng cách
- thiết kế review loop bền vững cho TodoApp slice
- tại sao reviewer nên để lại durable artifact cho coder follow-up

## Coach note

Repo này phải luôn phản ánh sự thật hiện tại của bootcamp. Nếu lesson đổi, cập nhật file này trước.
