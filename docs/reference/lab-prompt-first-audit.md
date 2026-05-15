# Audit lab docs theo rule prompt-first

## Mục tiêu audit

File này ghi lại 2 thứ:
1. rule boundary đã khóa cho lab docs,
2. trạng thái sau pass refactor prompt-first cho `lab-01` đến `lab-05`.

Phạm vi:
- Bao gồm: `lab-01` đến `lab-05`
- Loại trừ: `lab-00-day-0-setup.md` vì đây là bài setup/bootstrap ngoại lệ

## Rule đã khóa

Từ `lab-01` trở đi:
- level docs gánh lý thuyết sâu,
- lab docs chỉ giữ mental model ngắn + boundary ngắn,
- prompt chính phải xuất hiện sớm,
- lab phải tập trung vào objective + context + guardrails + deliverables + quality standard,
- phần command dài, primitive comparison, verified/sourced walkthrough, và recovery detail sâu phải nằm ở `docs/levels/*`.

`lab-00` là ngoại lệ hợp lý vì còn nhiều bước bootstrap/setup bằng tay.

## Trạng thái hiện tại

Verdict hiện tại: `PASS`.

Sau pass refactor này, các lab từ `lab-01` đến `lab-05` đã được rút gọn về prompt-first boundary:
- mỗi file vẫn giữ đúng 5 mục learner-facing,
- mục `Lý thuyết cần nắm` chỉ còn mental model + boundary ngắn,
- mỗi lab chỉ còn 1 subheading lý thuyết là `### Sơ đồ mental model`,
- prompt chính xuất hiện sớm hơn rõ rệt,
- không còn pattern lesson-hóa kiểu nhiều `### Bước ...` hoặc `### Phần ...` trước khi vào prompt.

## Bằng chứng định lượng sau refactor

| File | Tổng số dòng | Dòng bắt đầu `Prompt lab cho Jarvis` | Tỷ lệ file phải đọc trước khi vào prompt |
|---|---:|---:|---:|
| `lab-01-first-chat-sessions.md` | 102 | 40 | 38% |
| `lab-02-models-tools-skills-memory.md` | 107 | 43 | 39% |
| `lab-02b-context-files-profiles.md` | 101 | 36 | 35% |
| `lab-03-automation-gateway.md` | 99 | 34 | 33% |
| `lab-03b-routing-reliability.md` | 100 | 39 | 38% |
| `lab-04-kanban-pm-coder-reviewer.md` | 110 | 41 | 36% |
| `lab-05-builder-capstone.md` | 97 | 34 | 34% |

## Đọc kết quả này đúng cách

Heuristic hiện tại:
- nếu prompt xuất hiện sau hơn 50% file, cần điều tra,
- sau hơn 60% file, lab có khả năng cao đang bị lesson-hóa,
- sau hơn 70% file, gần như chắc chắn file đang hoạt động như lesson có thêm lab ở cuối.

Sau refactor này, toàn bộ `lab-01+` đều đưa prompt lên trước mốc 40% file.

## Những thay đổi chính đã áp dụng

### 1. Rút phần lý thuyết trong lab về đúng vai
Mỗi lab giờ chỉ giữ:
- một mental model ngắn,
- vài bullet boundary dùng/không dùng,
- vài hiểu sai thường gặp.

### 2. Đưa prompt chính lên sớm
Mỗi lab giờ vào prompt sau phần lý thuyết ngắn thay vì bắt learner đọc 60-80% file mới hành động.

### 3. Xóa pattern walkthrough dài
Đã bỏ hoặc chuyển khỏi lab các pattern sau:
- chuỗi `### Bước ...` dài,
- chuỗi `### Phần ...` dài,
- command-surface tour dài,
- verified/sourced inventory dài,
- recovery prose quá sâu.

### 4. Giữ lab như execution brief cho Jarvis
Mỗi lab giờ tập trung vào:
- objective,
- context,
- guardrails,
- deliverables,
- quality standard,
- output expectation,
- reusable prompt ngắn sau lab.

## Danh sách file đã được fix trong pass này

- `docs/labs/lab-01-first-chat-sessions.md`
- `docs/labs/lab-02-models-tools-skills-memory.md`
- `docs/labs/lab-02b-context-files-profiles.md`
- `docs/labs/lab-03-automation-gateway.md`
- `docs/labs/lab-03b-routing-reliability.md`
- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
- `docs/labs/lab-05-builder-capstone.md`

## Rule maintainers phải giữ về sau

Khi sửa hoặc thêm lab mới:
- mặc định coi `lab-00` là ngoại lệ duy nhất về bootstrap-heavy structure,
- từ `lab-01` trở đi, đừng viết lab như mini-lesson,
- nếu prompt chưa xuất hiện sớm, lab chưa đạt,
- nếu phần lý thuyết có nhiều `### Bước ...` hoặc `### Phần ...`, hãy xem lại boundary,
- nếu muốn giải thích sâu, chuyển sang level doc tương ứng thay vì bơm thêm prose vào lab.

## One-line verdict

Prompt-first lab boundary hiện đã được khôi phục: ngoài `lab-00`, các lab còn lại đã được refactor để level lo lý thuyết sâu còn lab tập trung vào prompt thực hành cho Jarvis.
