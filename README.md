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
   - current lesson file được chỉ ra trong `BOOTCAMP-STATUS.md`
   - `AGENTS.md`

## Lộ trình hiện tại

### Foundation Track
- Day 1: hiểu đúng Kanban vs Agent Team vs Goals
- Day 1 Lab: dựng mini team `pm -> coder -> reviewer` quanh benchmark **TodoApp**
- Day 1 Goal: tạo được board, profile, task pipeline, và quan sát dispatcher/handoff trên một TodoApp slice nhỏ
- Day 2: durable handoff artifact và review loop
- Day 2 Lab: ép reviewer để lại findings cho TodoApp slice ở file review bền vững trong repo/path thật
- Day 2 Goal: đọc được `context`, phân biệt comment pointer vs source-of-truth file, và tạo follow-up loop từ review artifact
- Day 3: chọn đúng workspace `scratch` vs `dir:<repo>` vs `worktree`
- Day 3 Lab: tạo task mẫu cho cả 3 workspace, inspect resolved path/intended path, và map chúng vào role `pm` / `reviewer` / `coder`
- Day 3 Goal: hiểu khi nào nên dùng từng workspace cho TodoApp workflow thay vì chọn theo thói quen

## File chính

- `BOOTCAMP-STATUS.md` — trạng thái bootcamp, current lesson, next step
- `AGENTS.md` — luật cho Hermes/agents khi tiếp tục viết tài liệu trong repo này
- `docs/todoapp-benchmark.md` — project benchmark cố định của khóa học
- `docs/day1.md` — bài học Day 1 để trainee thực hành trên máy khác
- `docs/day2.md` — bài học Day 2 về durable handoff artifact và review loop
- `docs/day3.md` — bài học Day 3 về chọn workspace đúng cho workflow TodoApp
- `log/day1-observations.md` — mẫu log trainee tự điền trong lúc lab Day 1
- `log/day2-observations.md` — mẫu log trainee tự điền trong lúc lab Day 2
- `log/day3-observations.md` — mẫu log trainee tự điền trong lúc lab Day 3

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
4. chạy lab Day 1 và điền `log/day1-observations.md`
5. `docs/day2.md`
6. chạy lab Day 2 và điền `log/day2-observations.md`
7. `docs/day3.md`
8. chạy lab Day 3 và điền `log/day3-observations.md`

## Learning outcome của pha này

Sau Day 3, trainee phải tự giải thích được:
- Kanban quản lý cái gì
- Agent Team quản lý cái gì
- `/goal` khác Kanban ở đâu
- vì sao `pm -> coder -> reviewer` tốt hơn một agent làm tất cả trong bài lab TodoApp này
- vì sao reviewer findings nên sống ở file review + comment pointer thay vì chỉ sống trong chat
- `scratch`, `dir:<repo>`, và artifact durability liên quan với nhau thế nào ở mức nền tảng
- khi nào `worktree` là đúng lựa chọn cho coding task trên TodoApp repo
- vì sao chọn sai workspace sẽ làm hỏng handoff hoặc coding flow
