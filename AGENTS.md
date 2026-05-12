# AGENTS.md

Hướng dẫn cho Hermes hoặc bất kỳ agent nào tiếp tục làm việc trong repo này.

## Mission của repo

Repo này tồn tại để tạo **tài liệu bootcamp chuẩn** cho trainee học Hermes về **Kanban** và **Agent Team**.

Benchmark project cố định của khóa học là **TodoApp** với stack:
- Next.js fullstack (App Router)
- TypeScript
- SQLite

Tài liệu trong repo phải phục vụ mục đích:
- trainee đọc và thực hành trên **máy khác**
- tài liệu phải rõ, ít mơ hồ, có checklist, có acceptance criteria
- artifact học tập phải sống trong repo, không sống trong chat

## Đối tượng chính

- Người học nói tiếng Việt
- Đã cài Hermes
- Muốn học theo kiểu Hermes-first, repo-backed, từng bước nhỏ
- Muốn hiểu Kanban/agent-team như workflow vận hành, không chỉ lý thuyết

## Working rules bắt buộc

1. **Viết vào file, không chỉ trả lời trong chat.**
2. **Ưu tiên tính thực hành.** Mỗi bài phải có command block, expected result, và failure modes.
3. **Không giả định máy hiện tại là máy mục tiêu.**
   - phải viết như tài liệu sẽ được chạy trên máy khác
   - dùng placeholder hợp lý như `<repo-root>` hoặc yêu cầu trainee tự thay path
4. **Không nói chung chung.** Nếu nhắc CLI Hermes, ưu tiên lệnh đã được verify từ docs/help.
5. **Một lesson = một concept chính.** Không nhồi quá nhiều primitive trong cùng một bài.
6. **Phân biệt rõ các primitive:**
   - `delegate_task` = short-lived subagent call
   - `kanban` = durable queue + handoff + audit trail
   - `/goal` = persistent objective for one session/agent
7. **Mọi ví dụ app trong khóa học mặc định là TodoApp.**
8. **Source of truth hiện tại:**
   - `BOOTCAMP-STATUS.md` cho trạng thái
   - `docs/todoapp-benchmark.md` cho benchmark project
   - `docs/day1.md` cho bài học hiện tại

## Format chuẩn cho lesson docs

Mỗi lesson nên có tối thiểu các mục sau:

```md
# Title

## Why this lesson exists
## Outcome
## Prerequisites
## Commands to run
## What to observe
## Failure modes
## Self-check
## Next lesson
```

## Nội dung cần tránh

- không dùng ví dụ mơ hồ kiểu “nhiều agent làm việc cùng nhau” mà không có assignee/dependency cụ thể
- không dạy workflow team bằng một profile `default` nếu lesson đang nói về role handoff
- không kết luận PASS nếu thiếu evidence chạy lệnh hoặc quan sát trạng thái
- không biến lesson Kanban thành lesson coding app quá sớm
- không đổi stack benchmark của khóa học nếu chưa có yêu cầu rõ ràng

## Definition chuẩn cần giữ nhất quán

- **Kanban quản lý flow công việc**
- **Agent Team quản lý role/handoff**
- **Board là boundary cứng** của queue
- **Task assignee** là lane/worker identity
- **Parent-child link** là dependency gate
- **Comment / summary / metadata** là handoff artifact

## Trạng thái hiện tại của curriculum

Hiện repo đang ở giai đoạn foundation:
- Day 1 tập trung vào mental model và mini workflow `pm -> coder -> reviewer`
- ví dụ và lab phải bám benchmark **TodoApp** thay vì ví dụ generic
- chưa đi vào multi-project manager pattern

## Khi cập nhật repo

Nếu viết thêm lesson mới:
1. cập nhật `BOOTCAMP-STATUS.md`
2. thêm link vào `README.md`
3. giữ ngôn ngữ tiếng Việt, giọng huấn luyện rõ ràng, ngắn gọn
4. thêm checklist thực hành thay vì chỉ thêm giải thích lý thuyết
5. nếu lesson có demo app, mặc định dùng TodoApp với Next.js App Router + TypeScript + SQLite
