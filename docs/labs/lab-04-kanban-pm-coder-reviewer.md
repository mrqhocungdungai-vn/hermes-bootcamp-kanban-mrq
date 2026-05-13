# Lab 04 — Kanban Jarvis -> PM -> Coder -> Reviewer

**Mục tiêu:** dựng một board cơ bản và nhìn thấy rõ workflow **Jarvis -> PM -> Coder -> Reviewer** với dependency, workspace discipline, review artifact, và recovery commands tối thiểu.  
**Thời lượng:** 45-90 phút.  
**Quan trọng:** command syntax bên dưới đã được verify; việc chạy end-to-end phụ thuộc profile/model/provider/gateway thực tế của learner.

## Bước 0 — Preconditions

Bạn nên đã qua:
- Level 1
- Level 2
- hiểu profile/workspace cơ bản

Bạn cũng cần nhớ 3 rule Level 4:
1. `dir:<path>` phải là **absolute path**.
2. `worktree` hợp với coding task hơn shared repo trực tiếp.
3. Dispatcher chạy **trong gateway mặc định**, nên task `ready` sẽ không tự chạy nếu gateway chưa lên.

## Bước 1 — xem command surfaces

```bash
hermes profile list
hermes profile create --help
hermes kanban --help
hermes kanban init
hermes kanban boards --help
hermes kanban boards create --help
hermes kanban create --help
hermes kanban list --help
hermes kanban show --help
hermes kanban link --help
hermes kanban context --help
hermes kanban runs --help
hermes kanban reassign --help
hermes kanban dispatch --help
```

**Điều cần hiểu ngay ở đây:** các lệnh trên là **bạn** dùng để bootstrap/quan sát board. Worker thật do dispatcher spawn sẽ dùng `kanban_*` tools, không shell ra `hermes kanban ...`.

## Bước 2 — bootstrap board riêng cho lab

```bash
export BOOTCAMP_REPO="/home/mrq-rd/projects/hermes-bootcamp-kanban-mrq"
export BOARD_SLUG="todoapp-bootcamp"

hermes kanban init
hermes kanban boards create "$BOARD_SLUG" \
  --name "TodoApp Bootcamp" \
  --description "Jarvis -> PM -> Coder -> Reviewer lab" \
  --switch
hermes kanban boards show
```

**Success:** active board là `todoapp-bootcamp`.

## Bước 3 — chuẩn bị profiles đúng cách

Xem profile hiện có:

```bash
hermes profile list
```

Nếu bạn chưa có đúng 4 profile `jarvis`, `pm`, `coder`, `reviewer`, **ưu tiên clone config từ profile đang dùng** để worker có sẵn `config.yaml`, `.env`, và `SOUL.md` cơ bản:

```bash
hermes profile create jarvis --clone
hermes profile create pm --clone
hermes profile create coder --clone
hermes profile create reviewer --clone
```

**Vì sao không khuyên tạo blank profile ở lab này?**  
Vì blank profile có thể chưa có provider/model/API key, khiến dispatcher spawn worker xong nhưng worker không làm được việc. Nếu bạn vẫn muốn profile rỗng, phải tự chạy thêm `jarvis setup`, `pm setup`, `coder setup`, `reviewer setup`.

## Bước 4 — tạo helper để lấy `task_id` từ JSON

Để block lệnh bên dưới copy-paste được gọn hơn, tạo một shell helper nhỏ:

```bash
json_id() {
  python -c 'import sys, json; print(json.load(sys.stdin)["id"])'
}
```

## Bước 5 — tạo graph Jarvis -> PM -> Coder -> Reviewer

### 5.1. Task Jarvis/orchestrator

```bash
JARVIS_ID=$(
  hermes kanban create "orchestrate: todoapp v1 workflow" \
    --assignee jarvis \
    --workspace "dir:$BOOTCAMP_REPO" \
    --body "Nhận objective Todo App benchmark, decomposition workflow PM -> Coder -> Reviewer, xác nhận artifact paths, workspace rules, và dependency graph trước khi team thực thi." \
    --json | json_id
)

echo "$JARVIS_ID"
```

### 5.2. Task PM/spec phụ thuộc Jarvis

```bash
PM_ID=$(
  hermes kanban create "spec: todoapp v1 acceptance criteria" \
    --assignee pm \
    --workspace "dir:$BOOTCAMP_REPO" \
    --parent "$JARVIS_ID" \
    --body "Đọc objective và orchestration contract do Jarvis tạo. Viết spec v1 cho Todo App benchmark. Kết quả phải là artifact markdown trong repo, nêu scope, acceptance criteria, và handoff cho coder." \
    --json | json_id
)

echo "$PM_ID"
```

### 5.3. Task coder phụ thuộc PM

```bash
CODER_ID=$(
  hermes kanban create "implement: todoapp v1 according to spec" \
    --assignee coder \
    --workspace worktree \
    --parent "$PM_ID" \
    --body "Đọc artifact spec do PM tạo. Thực hiện implementation tối thiểu theo spec, ghi rõ changed files và tests đã chạy trong handoff summary." \
    --json | json_id
)

echo "$CODER_ID"
```

### 5.4. Task reviewer phụ thuộc coder

```bash
REVIEW_ID=$(
  hermes kanban create "review: todoapp v1 implementation" \
    --assignee reviewer \
    --workspace "dir:$BOOTCAMP_REPO" \
    --parent "$CODER_ID" \
    --body "Đọc thay đổi của coder. Tạo review artifact markdown trong repo với verdict approve/request-changes, findings theo mức độ, và next actions rõ ràng cho coder." \
    --json | json_id
)

echo "$REVIEW_ID"
```

## Bước 6 — quan sát queue trước khi cho worker chạy

```bash
hermes kanban list
hermes kanban stats
hermes kanban show "$JARVIS_ID"
hermes kanban show "$PM_ID"
hermes kanban show "$CODER_ID"
hermes kanban show "$REVIEW_ID"
hermes kanban context "$CODER_ID"
hermes kanban dispatch --dry-run
```

**Điều cần hiểu:**
- `PM_ID` chưa ready nếu `JARVIS_ID` chưa done
- `CODER_ID` chưa ready nếu `PM_ID` chưa done
- `REVIEW_ID` chưa ready nếu `CODER_ID` chưa done
- `dispatch --dry-run` giúp bạn thấy scheduler logic mà chưa thực sự spawn worker

## Bước 7 — bật dispatcher thật bằng gateway

Nếu bạn muốn task thực sự được pickup, hãy đảm bảo gateway đang chạy:

```bash
hermes gateway status
hermes gateway start
hermes gateway status
```

**Ý chính:** trong Hermes docs hiện tại, dispatcher chạy **bên trong gateway** theo mặc định. Nếu gateway không chạy, task `ready` sẽ chỉ nằm chờ.

## Bước 8 — theo dõi runtime khi worker bắt đầu làm việc

```bash
hermes kanban list
hermes kanban show "$JARVIS_ID"
hermes kanban runs "$JARVIS_ID"
hermes kanban runs "$CODER_ID"
```

Khi workflow chạy thật, bạn nên để ý:
- task nào đang `ready`
- task nào đã sang `running`
- run history của từng task có summary/handoff gì
- parent output có đủ để child đọc tiếp không

## Bước 9 — review artifact tiêu chuẩn

Reviewer không nên chỉ nói trong chat. Reviewer nên để lại file kiểu:

```text
docs/reviews/YYYY-MM-DD-todoapp-v1-review.md
```

File đó ít nhất nên có:
- verdict: `approve` hoặc `request-changes`
- changed files reviewed
- critical / important / minor findings
- next actions for coder

**Rule:** coder follow-up nên đọc file artifact này, không phụ thuộc vào transcript sống của reviewer session.

## Bước 10 — recovery commands tối thiểu phải biết

Nếu task bị kẹt hoặc bạn muốn chẩn đoán worker context, dùng:

```bash
hermes kanban runs "$CODER_ID"
hermes kanban context "$CODER_ID"
hermes kanban show "$CODER_ID"
```

Nếu một task đang chạy bị kẹt và bạn muốn chuyển sang profile khác, cú pháp là:

```bash
# Ví dụ cú pháp; thay coder-v2 bằng profile có thật của bạn
hermes kanban reassign "$CODER_ID" coder-v2 --reclaim --reason "switch to alternate coder profile"
```

Nếu bạn chưa có profile thay thế, ít nhất hãy hiểu `reassign --reclaim` là lối recovery chuẩn hơn so với ngồi chờ mãi một worker lỗi.

## Feynman check

Trả lời không nhìn tài liệu:

1. Vì sao task reviewer nên dùng `dir:$BOOTCAMP_REPO` thay vì `scratch`?
2. Vì sao task coder thường hợp với `worktree` hơn shared repo trực tiếp?
3. Kanban khác delegation ở tính durable như thế nào?
4. Vì sao nói CLI/dashboard là human control plane, còn `kanban_*` là worker runtime surface?
5. Vì sao gateway lại quan trọng với lab này?
6. Nếu coder task bị kẹt, bạn sẽ nhìn `runs` hay chỉ nhìn `list`? Vì sao?

## Success criteria

- Tạo được một board riêng
- Tạo được 4 task có dependency rõ
- Chọn đúng workspace cho spec / code / review
- Hiểu vì sao Jarvis nên là orchestrator thay vì coder kiêm luôn dispatcher
- Hiểu rõ vì sao review artifact phải sống trong repo
- Biết ít nhất một recovery path cơ bản: `runs` / `context` / `reassign --reclaim`
