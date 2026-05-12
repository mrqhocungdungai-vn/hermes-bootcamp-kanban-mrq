# Hermes Bootcamp Kanban MRQ

Repo này là **source of truth** cho lộ trình học Hermes về **Kanban** và **Agent Team**.

Project benchmark cố định của khóa học là **TodoApp** với tech stack:
- Next.js fullstack (App Router)
- TypeScript
- SQLite

## Mục tiêu của repo
- viết tài liệu rõ ràng để trainee thực hành trên **máy khác**
- lưu lại contract, trạng thái bootcamp, checklist, và artifact học tập
- giảm phụ thuộc vào chat history hoặc context tạm thời

## Nguyên tắc sử dụng repo này

1. **Đây là repo tài liệu thực hành, không phải repo demo code chính.**
2. Mỗi bài học phải có:
   - mục tiêu
   - prerequisites
   - lệnh copy-paste được
   - expected result
   - failure modes
   - checklist tự chấm
3. Tài liệu phải giả định người học chạy trên **máy riêng của mình**, không phụ thuộc state của máy hiện tại.
4. Khi có mâu thuẫn, ưu tiên đọc theo thứ tự:
   - `BOOTCAMP-STATUS.md`
   - `docs/todoapp-benchmark.md`
   - `docs/day1.md`
   - `AGENTS.md`

## Lộ trình hiện tại

### Foundation Track
- Day 1: hiểu đúng Kanban vs Agent Team vs Goals
- Day 1 Lab: dựng mini team `pm -> coder -> reviewer` quanh benchmark **TodoApp**
- Day 1 Goal: tạo được board, profile, task pipeline, và quan sát dispatcher/handoff trên một TodoApp slice nhỏ

## File chính

- `BOOTCAMP-STATUS.md` — trạng thái bootcamp, current lesson, next step
- `AGENTS.md` — luật cho Hermes/agents khi tiếp tục viết tài liệu trong repo này
- `docs/todoapp-benchmark.md` — project benchmark cố định của khóa học
- `docs/day1.md` — bài học Day 1 để trainee thực hành trên máy khác
- `log/day1-observations.md` — mẫu log trainee tự điền trong lúc lab

## Cách trainee dùng repo này trên máy học

Trên máy học của trainee:

```bash
git clone <repo-url>
cd hermes-bootcamp-kanban-mrq
```

Sau đó đọc theo thứ tự:

1. `BOOTCAMP-STATUS.md`
2. `docs/todoapp-benchmark.md`
3. `docs/day1.md`
4. chạy lab đúng checklist
5. điền `log/day1-observations.md`

## Learning outcome của pha này

Sau Day 1, trainee phải tự giải thích được:
- Kanban quản lý cái gì
- Agent Team quản lý cái gì
- `/goal` khác Kanban ở đâu
- vì sao `pm -> coder -> reviewer` tốt hơn một agent làm tất cả trong bài lab TodoApp này
