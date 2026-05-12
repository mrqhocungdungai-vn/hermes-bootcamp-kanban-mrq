# Day 1 — Kanban và Agent Team Foundation Lab

Coach: Hermes
Trainee: bạn
Trạng thái: READY
Mục đích: tài liệu này được viết để bạn thực hành trên **máy học của bạn**, không phụ thuộc vào state của máy hiện tại.

Benchmark project cố định cho bài này là **TodoApp** với stack:
- Next.js fullstack (App Router)
- TypeScript
- SQLite

---

## 1. Why lesson này tồn tại

Nếu chưa phân biệt được:
- Kanban là gì
- Agent Team là gì
- `/goal` là gì

thì bạn rất dễ học Hermes sai hướng:
- thấy nhiều agent là tưởng đã có team
- thấy task board là tưởng tự nhiên có workflow tốt
- thấy `/goal` auto-continue là tưởng thay được Kanban

Bài này ép bạn **tự tay dựng một mini team** để thấy:
- Kanban quản lý flow
- Agent Team quản lý role/handoff
- dependency quyết định thứ tự chạy
- assignee quyết định ai làm

Và vì khóa học cần một benchmark thống nhất, mọi ví dụ trong bài này sẽ bám **TodoApp**.

---

## 2. Outcome

Cuối bài, bạn phải làm được 3 việc:

1. tạo một board học riêng
2. tạo 3 profile role cơ bản: `pm`, `coder`, `reviewer`
3. tạo workflow nhỏ dạng:

```text
PM task -> Coder task -> Reviewer task
```

và tự quan sát được vì sao:
- task sau không chạy khi parent chưa done
- assignee là identity của worker lane
- Kanban khác `/goal` ở mức primitive
- cùng một benchmark TodoApp có thể được dùng lại qua các ngày sau

---

## 2.1. Demo project cố định của khóa học

Từ bài này trở đi, benchmark project mặc định là **TodoApp**.

### Fixed stack
- Next.js fullstack (App Router)
- TypeScript
- SQLite

### Tại sao chọn stack này?
- nhỏ gọn, đủ thực tế để demo workflow agent team
- dễ chạy trên máy học cá nhân
- đủ fullstack để các role `pm`, `coder`, `reviewer` có việc thật
- SQLite giúp giảm friction setup ở giai đoạn foundation

### Scope benchmark mức tối thiểu
TodoApp benchmark không cần phức tạp. Chỉ cần đủ để về sau demo được:
- list todos
- create todo
- toggle done
- delete todo
- review / fix / handoff loop

---

## 3. Mental model trong 60 giây

### Kanban
Là durable queue quản lý trạng thái công việc:
- triage
- todo
- ready
- running
- blocked
- done

### Agent Team
Là tập hợp role thực hiện công việc:
- `pm`
- `coder`
- `reviewer`

### Goals
Là cơ chế giữ **một agent** tiếp tục làm việc qua nhiều turn cho tới khi đạt mục tiêu.

### Công thức nhớ nhanh

> Kanban quản lý flow. Agent Team quản lý handoff. `/goal` giữ một agent không bỏ cuộc quá sớm.

---

## 4. Prerequisites

Chạy toàn bộ trên **máy học của bạn**.

### 4.1. Verify Hermes

```bash
hermes --version
hermes profile list
hermes kanban --help
hermes kanban boards --help
```

### Expected result
- Hermes in ra version
- có thể xem danh sách profile
- `hermes kanban` có subcommand `create`, `list`, `show`, `dispatch`, `boards`
- `hermes kanban boards` có `create`, `switch`, `list`

### Nếu fail
- sửa Hermes install trước
- không tiếp tục lesson này nếu `hermes kanban --help` còn fail

---

## 5. Step 1 — Tạo board riêng cho bài học

Chọn một slug rõ ràng. Ví dụ:

```bash
hermes kanban boards create kanban-agent-team-lab
hermes kanban boards switch kanban-agent-team-lab
hermes kanban boards list
```

### Bạn đang làm gì ở bước này?
Bạn đang tạo **boundary** riêng cho queue học tập này.

### Expected result
- board mới xuất hiện trong `boards list`
- board đó là board current/active

### Điều cần hiểu
Board khác session.
Board cũng khác profile.
Board là nơi task sống.
Profile là identity của worker.

---

## 6. Step 2 — Tạo 3 role profile tối thiểu

Tạo 3 profile mới bằng cách clone từ profile đang dùng:

```bash
hermes profile create pm --clone
hermes profile create coder --clone
hermes profile create reviewer --clone
hermes profile list
```

### Bạn đang làm gì ở bước này?
Bạn đang tạo **worker identities** cho agent team.

### Expected result
Trong `hermes profile list` phải thấy ít nhất:
- `pm`
- `coder`
- `reviewer`

### Nếu fail
- nếu profile đã tồn tại, ghi nhận và dùng tiếp
- nếu `--clone` fail, dừng và sửa vấn đề profile/config trước

### Điều cần hiểu
Nếu bạn chỉ có `default`, bạn chưa thật sự thấy được handoff team.
Bài này không dùng `default` làm ví dụ chính vì mục tiêu là học role-based workflow.

---

## 7. Step 3 — Tạo mini pipeline `pm -> coder -> reviewer`

Bây giờ tạo 3 task có dependency.

```bash
PM_ID=$(hermes kanban create \
  "Write TodoApp slice spec" \
  --assignee pm \
  --body "Write a tiny spec for TodoApp using Next.js App Router + TypeScript + SQLite. Scope for this lab: minimal create/list todo slice. Output should clarify route, data shape, and acceptance criteria." \
  --json | jq -r .id)

CODER_ID=$(hermes kanban create \
  "Implement TodoApp slice from spec" \
  --assignee coder \
  --parent "$PM_ID" \
  --body "Read the PM handoff and implement the smallest possible TodoApp create/list slice with the fixed course stack." \
  --workspace scratch \
  --json | jq -r .id)

REVIEWER_ID=$(hermes kanban create \
  "Review TodoApp slice implementation" \
  --assignee reviewer \
  --parent "$CODER_ID" \
  --body "Read the coder result for the TodoApp slice and decide whether the work is acceptable or should be blocked for changes." \
  --json | jq -r .id)

printf "PM_ID=%s\nCODER_ID=%s\nREVIEWER_ID=%s\n" "$PM_ID" "$CODER_ID" "$REVIEWER_ID"
hermes kanban list
```

### Nếu máy bạn chưa có `jq`
Tạo task bằng từng lệnh riêng, rồi copy id bằng tay từ output.

### Bạn đang làm gì ở bước này?
- task 1 thuộc role `pm` và tạo spec cho một TodoApp slice
- task 2 thuộc role `coder` nhưng bị gate bởi `pm`
- task 3 thuộc role `reviewer` nhưng bị gate bởi `coder`

### Expected result
Ngay sau khi tạo:
- task `pm` có khả năng ở `ready`
- task `coder` thường ở `todo`
- task `reviewer` thường ở `todo`

### Điều cần hiểu
`assignee` quyết định **ai làm**.
`parent` quyết định **khi nào được làm**.

---

## 8. Step 4 — Quan sát task state thật

```bash
hermes kanban list
hermes kanban show "$PM_ID"
hermes kanban show "$CODER_ID"
hermes kanban show "$REVIEWER_ID"
```

### Hãy tự trả lời 3 câu hỏi
1. Task nào đang `ready`?
2. Task nào còn `todo` vì phụ thuộc chưa xong?
3. Task nào gắn với role nào?

Nếu bạn chưa trả lời được 3 câu này, chưa qua bài.

---

## 9. Step 5 — Dispatch để thấy flow chạy

```bash
hermes kanban dispatch
hermes kanban list
```

Nếu bạn có gateway đang chạy, dispatcher thường sống ở gateway. Nhưng ở bài foundation này, bạn chỉ cần biết `dispatch` là thứ làm việc claim/promote/spawn trong queue.

### Expected result
Sau dispatch, bạn có thể thấy một trong các tình huống:
- task `pm` sang `running`
- task `pm` sang `blocked`
- task `pm` sang `done`
- nếu profile spawn không chạy được, bạn sẽ thấy board không tiến như kỳ vọng và phải đọc trạng thái/log

### Điều cần hiểu
Kanban không chỉ là board nhìn đẹp.
Nó có engine xử lý:
- claim
- dependency promotion
- spawn worker theo assignee
- lưu event/run history

---

## 10. Step 6 — Quan sát handoff sau khi task đầu hoàn tất

Sau khi task `pm` xong hoặc có tiến triển, chạy lại:

```bash
hermes kanban show "$PM_ID"
hermes kanban show "$CODER_ID"
hermes kanban runs "$PM_ID"
```

### Hãy quan sát
- `pm` đã để lại summary/comment gì?
- `coder` có được promote từ `todo` sang `ready` chưa?
- có run history/event history chưa?

### Đây là điểm học quan trọng nhất
Kanban không chỉ chuyển trạng thái.
Nó tạo ra **handoff artifact** để worker sau đọc được kết quả của worker trước.

Trong benchmark TodoApp, artifact đó có thể là:
- spec cho route/page cần làm
- mô tả data shape của todo item
- nhận xét review về slice create/list/toggle/delete

---

## 11. Step 7 — So sánh với `/goal`

Bây giờ bạn phải tự viết ra câu trả lời này vào file log:

```text
Nếu dùng /goal thay vì Kanban cho bài này thì tôi sẽ thiếu điều gì?
```

### Đáp án mong đợi theo tinh thần
Bạn phải nhìn ra ít nhất 3 ý:
- `/goal` không tự tạo multi-role queue bền vững
- `/goal` không thay thế parent-child dependency giữa nhiều role
- `/goal` là persistence cho một objective trong một session, còn Kanban là persistence cho workflow/handoff giữa nhiều worker

---

## 12. Failure modes thường gặp

### Failure mode 1 — Chỉ có profile `default`
Triệu chứng:
- vẫn tạo task được
- nhưng lesson không còn là role handoff thật

Cách xử lý:
- quay lại Step 2
- tạo đủ `pm`, `coder`, `reviewer`

### Failure mode 2 — Tạo task nhưng không có dependency
Triệu chứng:
- mọi task đều có thể vào `ready`
- bạn không thấy flow tuần tự

Cách xử lý:
- tạo lại task với `--parent`

### Failure mode 3 — Nhầm board
Triệu chứng:
- list task không thấy task vừa tạo
- tưởng Hermes lỗi nhưng thật ra đang ở board khác

Cách xử lý:

```bash
hermes kanban boards list
```

rồi switch lại board đúng.

### Failure mode 4 — Dispatch không tiến
Triệu chứng:
- task không sang `running`
- profile/worker không spawn như mong đợi

Cách xử lý:
- đọc `show` và `runs`
- kiểm tra lại profile, model, config Hermes trên máy học
- mục tiêu bài này là **hiểu flow**, không bắt buộc phải có một feature code hoàn chỉnh

---

## 13. Self-check

Chỉ tự chấm PASS nếu bạn trả lời được **không nhìn tài liệu**:

1. Kanban quản lý cái gì?
2. Agent Team quản lý cái gì?
3. `assignee` dùng để làm gì?
4. `parent` dùng để làm gì?
5. tại sao `coder` task chưa chạy ngay khi `pm` chưa xong?
6. `/goal` khác Kanban ở điểm cốt lõi nào?
7. benchmark project cố định của khóa học là gì?
8. stack cố định của benchmark là gì?

Nếu trả lời mơ hồ, coi như chưa pass.

---

## 14. Minimum acceptance criteria

Day 1 PASS khi đủ tất cả:
- tạo được board riêng
- tạo được 3 profile role cơ bản
- tạo được 3 task với dependency đúng
- tự quan sát được ít nhất 2 trạng thái khác nhau trên board
- hiểu rằng benchmark project cố định của khóa học là TodoApp với Next.js App Router + TypeScript + SQLite
- ghi xong `log/day1-observations.md`
- viết được phần so sánh Kanban vs `/goal`

---

## 15. Ghi log bắt buộc

Mở và điền file:

- `log/day1-observations.md`

Không bỏ qua bước này. Nếu không ghi log, bạn sẽ chỉ có cảm giác học chứ không có evidence học.

---

## 16. Next lesson

Sau khi pass Day 1, bài tiếp theo nên là:
- review loop
- TodoApp slice handoff
- durable artifact cho reviewer
- vì sao reviewer findings nên sống ở file/comment thay vì chỉ sống trong chat
