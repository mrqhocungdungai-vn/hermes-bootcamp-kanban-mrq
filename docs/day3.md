# Day 3 — Chọn đúng workspace: `scratch` vs `dir:<repo>` vs `worktree`

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

Sau Day 2, bạn đã thấy:
- review artifact nên sống ở file
- `dir:$TODOAPP_REPO` giúp artifact bền hơn `scratch`

Nhưng nếu dừng ở đây, bạn vẫn rất dễ dùng workspace sai.

Sai lầm phổ biến là:
- task nào cũng để `scratch`
- hoặc task nào cũng dí vào `dir:$TODOAPP_REPO`
- hoặc nghe chữ `worktree` thấy ngầu nên dùng bừa cho mọi việc

Kết quả là:
- artifact bị thất lạc
- coder đụng cùng một repo quá sớm
- review loop lẫn lộn giữa sandbox và source of truth
- bạn không còn hiểu **vì sao** một task nên chạy ở nơi đó

Day 3 ép bạn trả lời câu hỏi nền tảng này:

> với từng loại công việc trong TodoApp benchmark, nên chọn workspace nào và vì sao?

---

## 2. Outcome

Cuối bài, bạn phải làm được 5 việc:

1. phân biệt rõ **mục đích** của `scratch`, `dir:<repo>`, và `worktree`
2. tự tạo task mẫu cho cả 3 loại workspace
3. dùng CLI để quan sát workspace resolution hoặc intended path
4. giải thích được vì sao **pm / reviewer / coder** thường không nên mặc định dùng cùng một workspace
5. chọn được workspace hợp lý cho các task TodoApp thường gặp

---

## 3. Mental model trong 60 giây

### `scratch`
Dùng khi bạn cần một nơi tạm để nghĩ, viết nháp, phân tích, hoặc thao tác không cần bám repo thật.

Hình dung:

```text
bàn nháp riêng
```

### `dir:<repo>`
Dùng khi task phải đọc/ghi trực tiếp vào **một thư mục thật, dùng chung, có ý nghĩa lâu dài**.

Hình dung:

```text
phòng hồ sơ thật của dự án
```

### `worktree`
Dùng khi task là **coding task trên git repo**, và bạn muốn coder có **nhánh làm việc riêng, cô lập**, nhưng vẫn bám cùng project.

Hình dung:

```text
một bản sao làm việc riêng của cùng repo, dành cho lập trình
```

### Công thức nhớ nhanh

> `scratch` để nghĩ. `dir:<repo>` để chia sẻ artifact thật. `worktree` để code an toàn trên cùng repo.

---

## 4. Ground truth đã verify từ Hermes docs/help

Những điểm dưới đây đã được verify từ CLI help và docs Hermes:

- `hermes kanban create --help` xác nhận `--workspace` hỗ trợ:
  - `scratch`
  - `worktree`
  - `dir:<path>`
- user guide của Hermes mô tả:
  - `scratch` là thư mục tạm dưới kanban workspace root
  - `dir:<path>` phải là **absolute path**
  - `worktree` là git worktree dưới `.worktrees/<id>/`
- mã nguồn Hermes hiện tại còn ghi rõ:
  - `dir:<path>` bị reject nếu là relative path
  - `worktree` trong v1 **không tự được tạo ngay**; worker-side flow mới là nơi `git worktree add` diễn ra

### Điều cực quan trọng
`worktree` không có nghĩa là:
- "magic repo copy cho mọi task"
- "thay thế luôn `dir:<repo>`"
- "bất cứ task nào đụng code cũng nên dùng ngay"

---

## 5. Prerequisites

Bạn chỉ chạy Day 3 khi **Day 2 đã PASS**.

Ngoài ra trên máy học của bạn cần verify:

```bash
hermes --version
hermes profile list
hermes kanban create --help
hermes kanban claim --help
hermes kanban reclaim --help
hermes kanban archive --help
```

Nếu bạn muốn test `worktree` một cách đúng tinh thần, repo TodoApp của bạn cũng nên là git repo:

```bash
export TODOAPP_REPO=/absolute/path/to/your/todoapp-repo
cd "$TODOAPP_REPO" || exit 1
git rev-parse --show-toplevel
git worktree list
```

### Expected result
- Hermes chạy được
- còn thấy role `pm`, `coder`, `reviewer`
- `hermes kanban create --help` hiện rõ `scratch | worktree | dir:<path>`
- nếu test `worktree`, `git rev-parse --show-toplevel` phải in ra repo root

### Nếu fail
- nếu Hermes help fail: sửa Hermes trước
- nếu `git rev-parse --show-toplevel` fail: bạn chưa có git repo thật, vì vậy phần `worktree` chỉ nên học ở mức concept hoặc chuẩn bị repo trước

---

## 6. Decision table — chọn workspace nào cho TodoApp benchmark?

| Tình huống | Workspace nên dùng | Vì sao |
|---|---|---|
| PM viết spec nháp cho slice nhỏ | `scratch` | chưa cần đụng repo thật |
| Reviewer tổng hợp nhận xét mà chưa cần ghi source-of-truth | `scratch` | tách giai đoạn suy nghĩ khỏi repo |
| Reviewer phải để lại findings bền vững | `dir:$TODOAPP_REPO` | cần ghi file review thật |
| PM/coder/reviewer cùng đọc docs/spec/review đã chốt | `dir:$TODOAPP_REPO` | cần cùng nhìn một source of truth |
| Coder implement feature thật trên repo git | `worktree` | cần cô lập nhánh làm việc nhưng vẫn cùng project |
| Task chỉ là quan sát / debug nhanh ngoài repo | `scratch` | nhanh, rẻ, ít rủi ro |

### Rule đơn giản để nhớ
- Nếu **không cần repo thật**, bắt đầu từ `scratch`
- Nếu **cần đọc/ghi source-of-truth thật**, dùng `dir:<repo>`
- Nếu **cần coding cô lập trên git repo**, dùng `worktree`

---

## 7. Step 1 — Chuẩn bị path repo TodoApp và giữ board gọn

Khai báo repo thật:

```bash
export TODOAPP_REPO=/absolute/path/to/your/todoapp-repo
printf 'TODOAPP_REPO=%s\n' "$TODOAPP_REPO"
mkdir -p "$TODOAPP_REPO/docs/specs"
mkdir -p "$TODOAPP_REPO/docs/reviews"
```

Dùng lại board hiện có từ Day 1/Day 2, nhưng **đừng dispatch** trong bài này.
Mục tiêu của Day 3 là **inspect workspace choice**, không phải chạy full pipeline.

Bạn có thể xem board hiện tại bằng:

```bash
hermes kanban boards list
```

---

## 8. Step 2 — Tạo 3 task mẫu, mỗi task một workspace

### 8.1. Scratch task — PM nháp spec

```bash
SCRATCH3_ID=$(hermes kanban create \
  "Draft TodoApp filter spec in scratch" \
  --assignee pm \
  --workspace scratch \
  --body "Think through a possible TodoApp filter feature (all/active/completed). This is still a draft discussion task; no source-of-truth file required yet." \
  --json | jq -r .id)

printf 'SCRATCH3_ID=%s\n' "$SCRATCH3_ID"
```

### 8.2. Dir task — Reviewer ghi review artifact thật

```bash
DIR3_ID=$(hermes kanban create \
  "Write TodoApp review summary into repo docs" \
  --assignee reviewer \
  --workspace "dir:$TODOAPP_REPO" \
  --body "Write or update a durable review summary under docs/reviews/. The goal is to practice using the real repo as the source of truth for review artifacts." \
  --json | jq -r .id)

printf 'DIR3_ID=%s\n' "$DIR3_ID"
```

### 8.3. Worktree task — Coder implement feature trên repo git

```bash
WORKTREE3_ID=$(hermes kanban create \
  "Implement TodoApp filter feature in isolated worktree" \
  --assignee coder \
  --workspace worktree \
  --body "This is a coding task intended for a git-backed TodoApp repo. The goal is to implement a TodoApp filter feature in an isolated coding workspace rather than directly in the shared repo root." \
  --json | jq -r .id)

printf 'WORKTREE3_ID=%s\n' "$WORKTREE3_ID"
```

### 8.4. Kiểm tra nhanh

```bash
printf 'SCRATCH3_ID=%s\nDIR3_ID=%s\nWORKTREE3_ID=%s\n' "$SCRATCH3_ID" "$DIR3_ID" "$WORKTREE3_ID"
hermes kanban list
```

### Nếu máy bạn chưa có `jq`
Tạo từng task riêng rồi copy id bằng tay từ output.

---

## 9. Step 3 — Quan sát từng workspace bằng `claim`

Theo help của Hermes, `claim` sẽ in ra **resolved workspace path**.

Vì đây là bài inspect, bạn claim xong rồi reclaim lại ngay.

### 9.1. Scratch

```bash
hermes kanban claim "$SCRATCH3_ID"
hermes kanban reclaim "$SCRATCH3_ID" --reason "Day 3 scratch inspection only"
```

### Bạn cần thấy gì?
- path nằm trong kanban workspace root
- thường có dạng:

```text
~/.hermes/kanban/workspaces/<task-id>
```

hoặc nếu dùng board non-default:

```text
~/.hermes/kanban/boards/<board-slug>/workspaces/<task-id>
```

### 9.2. Dir

```bash
hermes kanban claim "$DIR3_ID"
hermes kanban reclaim "$DIR3_ID" --reason "Day 3 dir inspection only"
```

### Bạn cần thấy gì?
- path chính là repo/path thật bạn đã đưa vào `dir:$TODOAPP_REPO`
- không phải một thư mục tạm do kanban tự sinh

### 9.3. Worktree

```bash
hermes kanban claim "$WORKTREE3_ID"
hermes kanban reclaim "$WORKTREE3_ID" --reason "Day 3 worktree inspection only"
```

### Bạn cần hiểu gì?
- path intended thường nằm dưới:

```text
<repo-cwd>/.worktrees/<task-id>
```

- nhưng ở Hermes v1, `worktree` **không có nghĩa là directory chắc chắn đã được tạo ngay lúc này**
- worker-side coding flow mới là nơi `git worktree add` thường diễn ra

### Kết luận mini ở bước này
- `scratch` = Hermes tự cấp chỗ tạm
- `dir:<repo>` = bạn chỉ thẳng vào chỗ thật
- `worktree` = intended isolated coding path cho git workflow

---

## 10. Step 4 — Thử case sai để hiểu rule đúng

Bài này nên có ít nhất 2 lỗi có chủ ý.

### 10.1. Lỗi 1 — `dir:` với relative path

```bash
hermes kanban create \
  "Bad relative dir workspace" \
  --assignee pm \
  --workspace "dir:../relative-path-should-fail" \
  --body "Intentional mistake for Day 3"
```

### Điều mong đợi
Task có thể được tạo hoặc giữ metadata, nhưng khi workflow resolve workspace thật, relative path là **sai tinh thần và sẽ bị reject ở bước resolve/dispatch**.

### 10.2. Lỗi 2 — dùng `worktree` cho task không phải coding

Tự trả lời câu này bằng lời của bạn:

```text
Nếu reviewer chỉ cần viết một file review bền vững, vì sao worktree là overkill hoặc sai tool?
```

### Đáp án mong đợi theo tinh thần
Vì reviewer lúc này không cần một git coding lane cô lập.
Điều reviewer cần là **ghi artifact thật vào source of truth**.
`dir:$TODOAPP_REPO` trực tiếp và rõ hơn.

---

## 11. Step 5 — Map workspace vào các role của TodoApp workflow

Bây giờ tự map 3 role cơ bản:

### `pm`
Thường bắt đầu bằng:
- `scratch` khi còn đang draft spec
- `dir:$TODOAPP_REPO` khi cần chốt spec vào docs thật

### `reviewer`
Thường bắt đầu bằng:
- `scratch` khi chỉ đang nghĩ hoặc soạn nháp
- `dir:$TODOAPP_REPO` khi cần để lại review file bền vững

### `coder`
Thường nên dùng:
- `worktree` cho coding task thật trên git repo
- đôi khi `scratch` cho spike cực nhỏ, nhưng đó **không nên** là mặc định nếu mục tiêu là sửa repo benchmark thật

### Công thức vai trò

```text
PM: scratch -> dir
Reviewer: scratch -> dir
Coder: worktree (khi code thật)
```

---

## 12. Failure modes thường gặp

### Failure mode 1 — Xem `scratch` như repo thật
Triệu chứng:
- coder làm việc trong scratch
- sau đó mọi người tưởng artifact/code đang sống ở repo benchmark

Cách xử lý:
- nếu output phải sống lâu và được dùng lại, đừng mặc định `scratch`

### Failure mode 2 — Dùng `dir:<repo>` cho mọi task
Triệu chứng:
- task nào cũng chạm repo thật
- board mất vùng đệm an toàn để draft/think

Cách xử lý:
- chỉ dùng `dir:<repo>` khi thật sự cần source of truth chung

### Failure mode 3 — Dùng `worktree` cho task không cần code
Triệu chứng:
- reviewer/pm task bị kéo vào git workflow không cần thiết
- người học nhầm `worktree` là lựa chọn “xịn hơn” nên phải dùng

Cách xử lý:
- nhớ rằng `worktree` là **công cụ cho coding isolation**, không phải huy hiệu cao cấp

### Failure mode 4 — Quên absolute path cho `dir:`
Triệu chứng:
- path nhìn có vẻ hợp lý nhưng là relative
- về sau dispatch/resolve workspace fail hoặc mơ hồ

Cách xử lý:
- luôn export path tuyệt đối, ví dụ:

```bash
export TODOAPP_REPO=/absolute/path/to/your/todoapp-repo
```

### Failure mode 5 — Tưởng `worktree` tự tạo đầy đủ ngay lập tức
Triệu chứng:
- claim xong nhưng không thấy một repo hoàn chỉnh xuất hiện như mình tưởng

Cách xử lý:
- nhớ rằng ở Hermes v1, `worktree` là intended coding path; worker-side flow mới là nơi `git worktree add` thường chạy

---

## 13. Self-check

Chỉ tự chấm PASS nếu bạn trả lời được **không nhìn tài liệu**:

1. `scratch` dùng khi nào?
2. `dir:<repo>` dùng khi nào?
3. `worktree` dùng khi nào?
4. vì sao `dir:<repo>` phải là absolute path?
5. vì sao reviewer ghi file review thật thường hợp với `dir:<repo>` hơn `worktree`?
6. vì sao coder implement feature thật thường hợp với `worktree` hơn `scratch`?
7. benchmark project cố định của khóa học vẫn là gì?
8. stack cố định vẫn là gì?

---

## 14. Minimum acceptance criteria

Day 3 PASS khi đủ tất cả:
- tạo được ít nhất 3 task mẫu với `scratch`, `dir:<repo>`, và `worktree`
- dùng `claim`/`reclaim` để inspect ít nhất 2 workspace path hoặc intended path
- hiểu và ghi lại được vì sao `dir:<repo>` cần absolute path
- map được `pm`, `reviewer`, `coder` sang workspace hợp lý trong TodoApp workflow
- ghi xong `log/day3-observations.md`

---

## 15. Ghi log bắt buộc

Mở và điền file:

- `log/day3-observations.md`

Không bỏ qua bước này. Nếu không ghi log, bạn sẽ rất dễ quay lại kiểu suy nghĩ cũ:

> task nào cũng dùng một workspace cho tiện

Đó là cách làm nhanh để hỏng workflow.

---

## 16. Next lesson

Sau khi pass Day 3, bài tiếp theo nên là:
- thiết kế pipeline TodoApp chuẩn hơn cho `pm -> coder -> reviewer`
- khi nào coder follow-up nên tạo **task mới** thay vì sửa chồng lên task cũ
- cách chọn workspace xuyên suốt một review loop thật