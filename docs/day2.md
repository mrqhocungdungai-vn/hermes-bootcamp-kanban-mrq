# Day 2 — Durable Handoff Artifact và Review Loop Lab

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

Sau Day 1, bạn đã thấy:
- board
- assignee
- dependency
- dispatch

Nhưng Day 1 mới chỉ cho bạn thấy **flow**.

Day 2 ép bạn học thứ quan trọng hơn:

> worker sau thật ra đọc cái gì từ worker trước?

Nếu câu trả lời của bạn vẫn là:
- "đọc trong chat"
- "nhìn tạm summary là được"
- "reviewer nói miệng cũng ổn"

thì bạn chưa có workflow bền vững.

Trong agent team thật, đặc biệt với benchmark **TodoApp**, reviewer phải để lại **durable artifact** để coder follow-up được dù:
- session cũ đã đóng
- context đã bị compact
- người khác tiếp quản task
- bạn quay lại sau vài giờ hoặc vài ngày

Bài này ép bạn chuyển từ:

```text
review sống trong đầu / trong chat
```

sang:

```text
review sống trong file + comment + task history
```

---

## 2. Outcome

Cuối bài, bạn phải làm được 5 việc:

1. dùng lại board và 3 role `pm`, `coder`, `reviewer`
2. tạo một pipeline TodoApp mới chạy trên **repo/path thật** thay vì scratch mơ hồ
3. ép reviewer để lại findings ở **file cố định** trong repo TodoApp
4. tự đọc được `context` mà worker sau nhìn thấy
5. hiểu vì sao **file review + comment pointer** tốt hơn chat-only feedback

---

## 3. Mental model trong 60 giây

### Handoff artifact là gì?
Là thứ worker sau có thể đọc lại được từ worker trước.

Trong Hermes Kanban, artifact có thể sống ở:
- task body
- summary
- comment
- run history
- file trong repo/workspace

### Với review loop, ưu tiên artifact nào?
Với bài code/review quanh TodoApp, ưu tiên:

1. **file review trong repo** — source of truth
2. **comment trên task** — pointer ngắn tới file đó
3. **summary / run history** — audit trail bổ sung

### Công thức nhớ nhanh

> Chat giúp nói chuyện. File giúp handoff. Comment giúp định vị file. Kanban giúp mọi thứ sống đủ lâu.

---

## 4. Prerequisites

Bạn chỉ chạy Day 2 khi **Day 1 đã PASS**.

Ngoài ra trên máy học của bạn cần verify:

```bash
hermes --version
hermes profile list
hermes kanban boards list
hermes kanban assignees
hermes kanban context --help
hermes kanban comment --help
```

### Expected result
- Hermes chạy được
- còn thấy board Day 1
- còn thấy role `pm`, `coder`, `reviewer`
- có subcommand `context` và `comment`

### Nếu fail
- sửa Hermes/board/profile trước
- không tiếp tục Day 2 nếu bạn còn chưa đọc được board hoặc profile của Day 1

---

## 5. Step 1 — Khai báo path repo TodoApp thật

Day 1 dùng `scratch` để học primitive.

Day 2 chuyển sang **path thật** để bạn thấy durable artifact sống ở đâu.

Trên máy học của bạn, khai báo path repo TodoApp:

```bash
export TODOAPP_REPO=/absolute/path/to/your/todoapp-repo
printf 'TODOAPP_REPO=%s\n' "$TODOAPP_REPO"
mkdir -p "$TODOAPP_REPO/docs/specs"
mkdir -p "$TODOAPP_REPO/docs/reviews"
```

### Nếu bạn chưa có repo TodoApp thật
Bạn vẫn có thể tạo một thư mục practice tạm để học workflow artifact:

```bash
export TODOAPP_REPO=$HOME/practice-todoapp
mkdir -p "$TODOAPP_REPO/docs/specs"
mkdir -p "$TODOAPP_REPO/docs/reviews"
printf 'TODOAPP_REPO=%s\n' "$TODOAPP_REPO"
```

### Điều cần hiểu
Mục tiêu của Day 2 không phải build full app hoàn chỉnh.
Mục tiêu là làm cho **review artifact có chỗ sống bền vững**.

---

## 6. Step 2 — Quay lại board Day 1 và verify role

Nếu bạn nhớ slug board Day 1 thì switch lại đúng board đó.

Ví dụ:

```bash
hermes kanban boards list
hermes kanban boards switch kanban-agent-team-lab
hermes profile list
hermes kanban assignees
```

### Expected result
- board Day 1 đang active
- role `pm`, `coder`, `reviewer` vẫn tồn tại

### Điều cần hiểu
Day 2 không tạo primitive mới.
Day 2 dùng lại primitive cũ để học **review loop bền vững**.

---

## 7. Step 3 — Tạo pipeline mới nhưng ép artifact phải sống trong repo

Trong lab này, ta dùng slice:

```text
TodoApp create/list slice
```

Và ép reviewer phải để lại findings ở file:

```text
$TODOAPP_REPO/docs/reviews/day2-create-list-review.md
```

### 7.1. Tạo PM task

```bash
PM2_ID=$(hermes kanban create \
  "Write TodoApp create/list spec with artifact path" \
  --assignee pm \
  --workspace "dir:$TODOAPP_REPO" \
  --body "Write a tiny implementation-ready spec for a TodoApp create/list slice using Next.js App Router + TypeScript + SQLite. The spec must be saved or clearly reference docs/specs/day2-create-list-spec.md. Include route/page, minimal data shape, and acceptance criteria." \
  --json | jq -r .id)

printf 'PM2_ID=%s\n' "$PM2_ID"
```

### 7.2. Tạo Coder task

```bash
CODER2_ID=$(hermes kanban create \
  "Implement TodoApp create/list slice from Day 2 spec" \
  --assignee coder \
  --parent "$PM2_ID" \
  --workspace "dir:$TODOAPP_REPO" \
  --body "Read the PM handoff for the TodoApp create/list slice. Work inside the real repo path. Implement the smallest acceptable version for the fixed stack. In your summary, list the files you changed and any known gaps." \
  --json | jq -r .id)

printf 'CODER2_ID=%s\n' "$CODER2_ID"
```

### 7.3. Tạo Reviewer task

```bash
REVIEW2_ID=$(hermes kanban create \
  "Review TodoApp create/list slice and leave durable artifact" \
  --assignee reviewer \
  --parent "$CODER2_ID" \
  --workspace "dir:$TODOAPP_REPO" \
  --body "Review the TodoApp create/list slice. Your findings must live in docs/reviews/day2-create-list-review.md inside the repo. The file must include verdict, required changes, and evidence. Also leave a task comment that points to that file path. Do not leave the review only in chat." \
  --json | jq -r .id)

printf 'REVIEW2_ID=%s\n' "$REVIEW2_ID"
```

### 7.4. Kiểm tra task vừa tạo

```bash
printf 'PM2_ID=%s\nCODER2_ID=%s\nREVIEW2_ID=%s\n' "$PM2_ID" "$CODER2_ID" "$REVIEW2_ID"
hermes kanban list
```

### Nếu máy bạn chưa có `jq`
Tạo từng task riêng rồi copy id bằng tay từ output.

### Điều cần hiểu
So với Day 1, điểm khác biệt cốt lõi là:
- workspace giờ là `dir:$TODOAPP_REPO`
- reviewer bị ép để lại review ở **một file cụ thể**
- comment chỉ đóng vai trò pointer, không phải source of truth chính

---

## 8. Step 4 — Dispatch và quan sát artifact chain

```bash
hermes kanban dispatch
hermes kanban list
```

Sau đó, lặp lại việc quan sát theo thời gian:

```bash
hermes kanban show "$PM2_ID"
hermes kanban runs "$PM2_ID"
hermes kanban show "$CODER2_ID"
hermes kanban runs "$CODER2_ID"
hermes kanban show "$REVIEW2_ID"
hermes kanban runs "$REVIEW2_ID"
```

### Nếu muốn follow live

```bash
hermes kanban tail "$PM2_ID"
```

hoặc sau khi PM xong thì tail task tiếp theo.

### Expected result
Bạn sẽ thấy cùng một pattern như Day 1:
- PM task đi trước
- Coder task chờ parent
- Reviewer task chờ coder

Nhưng lần này bạn còn phải thấy thêm:
- workspace là repo/path thật
- comment/history/reference gắn với artifact path cụ thể

---

## 9. Step 5 — Đọc đúng thứ worker sau thực sự nhận được

Đây là bước quan trọng nhất của Day 2.

Dùng `context` để xem worker sau nhìn thấy gì:

```bash
hermes kanban context "$CODER2_ID"
hermes kanban context "$REVIEW2_ID"
```

### Hãy tự trả lời
1. `coder` đang nhận gì từ `pm`?
2. `reviewer` đang nhận gì từ `coder`?
3. artifact path `docs/reviews/day2-create-list-review.md` đã xuất hiện rõ trong context/task history chưa?

### Điều cần hiểu
Rất nhiều người học sai ở chỗ này.
Họ tưởng handoff là một cảm giác mơ hồ.

Không.

Handoff tốt là thứ bạn có thể **chỉ vào được** bằng:
- file path
- comment
- summary
- context output

---

## 10. Step 6 — Kiểm tra durable review artifact trong repo

Sau khi reviewer task chạy xong hoặc có tiến triển, kiểm tra file review:

```bash
test -f "$TODOAPP_REPO/docs/reviews/day2-create-list-review.md" && echo "REVIEW FILE EXISTS"
```

Nếu file đã tồn tại, đọc nhanh:

```bash
python3 - <<'PY'
from pathlib import Path
import os
p = Path(os.environ['TODOAPP_REPO']) / 'docs' / 'reviews' / 'day2-create-list-review.md'
print(p)
print('-' * 60)
if p.exists():
    print(p.read_text())
else:
    print('review file missing')
PY
```

Và kiểm tra comment pointer trên task reviewer:

```bash
hermes kanban show "$REVIEW2_ID"
```

### PASS pattern mong đợi
Bạn nên thấy đủ 3 lớp:
1. file review tồn tại trong repo
2. reviewer task có comment hoặc summary chỉ tới file đó
3. task history cho phép người sau truy ngược lại review

### FAIL pattern thường gặp
- review chỉ sống trong comment, không có file
- review chỉ sống trong chat/session, không có task evidence
- file có tồn tại nhưng task không hề chỉ tới file đó

---

## 11. Step 7 — Tạo follow-up loop từ review artifact

Nếu reviewer yêu cầu sửa, tạo **task follow-up mới** cho coder.

```bash
FIX2_ID=$(hermes kanban create \
  "Apply Day 2 reviewer findings to TodoApp create/list slice" \
  --assignee coder \
  --parent "$REVIEW2_ID" \
  --workspace "dir:$TODOAPP_REPO" \
  --body "Read docs/reviews/day2-create-list-review.md and apply the required fixes. In your summary, map each finding to the file you changed or explain why you did not change it." \
  --json | jq -r .id)

printf 'FIX2_ID=%s\n' "$FIX2_ID"
hermes kanban show "$FIX2_ID"
```

### Điều cần hiểu
Đây mới là review loop thật:

```text
PM -> Coder -> Reviewer -> Coder follow-up
```

Coder follow-up không nên đoán lại review từ chat.
Nó phải đọc **artifact file** mà reviewer để lại.

---

## 12. Failure modes thường gặp

### Failure mode 1 — Vẫn dùng `scratch`
Triệu chứng:
- worker chạy được
- nhưng bạn khó chỉ ra file artifact sống ở đâu

Cách xử lý:
- với Day 2, ưu tiên `--workspace "dir:$TODOAPP_REPO"`

### Failure mode 2 — Reviewer chỉ để comment, không để file
Triệu chứng:
- `hermes kanban show` có vẻ có review
- nhưng repo không có `docs/reviews/day2-create-list-review.md`

Cách xử lý:
- tạo lại reviewer task với yêu cầu artifact path rõ ràng hơn
- nhắc rõ: comment chỉ là pointer, file mới là source of truth chính

### Failure mode 3 — Có file nhưng coder không biết phải đọc ở đâu
Triệu chứng:
- file nằm trong repo
- nhưng task body/comment không hề chỉ path đó

Cách xử lý:
- reviewer phải để lại comment chỉ rõ path
- follow-up task phải nhắc đúng file path trong body

### Failure mode 4 — Không đọc `context`
Triệu chứng:
- bạn có cảm giác workflow đang chạy
- nhưng không biết worker sau thực sự được truyền gì

Cách xử lý:
- bắt buộc chạy `hermes kanban context "$CODER2_ID"`
- bắt buộc chạy `hermes kanban context "$REVIEW2_ID"`

---

## 13. Self-check

Chỉ tự chấm PASS nếu bạn trả lời được **không nhìn tài liệu**:

1. durable handoff artifact là gì?
2. vì sao review chỉ sống trong chat là yếu?
3. vì sao `dir:$TODOAPP_REPO` hợp với Day 2 hơn `scratch`?
4. `hermes kanban context` giúp bạn thấy điều gì?
5. file review của reviewer nên sống ở đâu trong bài lab này?
6. vì sao follow-up coder task phải đọc review file thay vì đoán lại từ chat?
7. benchmark project cố định vẫn là gì?
8. stack cố định vẫn là gì?

---

## 14. Minimum acceptance criteria

Day 2 PASS khi đủ tất cả:
- dùng lại được board và 3 role `pm`, `coder`, `reviewer`
- tạo được một pipeline TodoApp chạy với `dir:$TODOAPP_REPO`
- đọc được ít nhất một lần output của `hermes kanban context`
- reviewer có để lại hoặc được yêu cầu để lại file `docs/reviews/day2-create-list-review.md`
- bạn hiểu comment chỉ là pointer, file review mới là durable artifact chính cho follow-up
- ghi xong `log/day2-observations.md`

---

## 15. Ghi log bắt buộc

Mở và điền file:

- `log/day2-observations.md`

Không bỏ qua bước này. Nếu không ghi log, bạn sẽ không phân biệt được:
- thứ gì thực sự đã được handoff
- thứ gì chỉ là cảm giác nhớ mang máng trong chat

---

## 16. Next lesson

Sau khi pass Day 2, bài tiếp theo nên là:
- `scratch` vs `dir:<repo>` vs `worktree`
- khi nào dùng workspace nào cho TodoApp benchmark
- vì sao reviewer/coder loop trong repo thật khác với sandbox tạm
