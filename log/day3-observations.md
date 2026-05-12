# Day 3 Observations Log

Điền file này trên **máy học của bạn** trong lúc chạy lab `docs/day3.md`.

## Session metadata
- Date:
- Machine:
- OS:
- Hermes version:
- Benchmark project: TodoApp
- Stack: Next.js App Router + TypeScript + SQLite
- Active model/provider:
- Start time:
- End time:

## Preconditions
- Day 2 PASS chưa?:
- Board slug đang dùng:
- `hermes kanban create --help`: PASS / FAIL
- `hermes kanban claim --help`: PASS / FAIL
- `hermes kanban reclaim --help`: PASS / FAIL
- `hermes kanban archive --help`: PASS / FAIL
- `TODOAPP_REPO` là absolute path chưa?:
- `git rev-parse --show-toplevel`: PASS / FAIL
- `git worktree list`: PASS / FAIL

## TodoApp repo path
- `TODOAPP_REPO` tôi dùng là:
- Đây có phải git repo thật không?:
- Có `docs/specs/` chưa?:
- Có `docs/reviews/` chưa?:

## Task IDs
- SCRATCH3_ID:
- DIR3_ID:
- WORKTREE3_ID:

## Workspace observations
### Scratch
- `claim` in ra path gì?:
- Path đó có nằm trong kanban workspace root không?:
- Tôi hiểu `scratch` là gì bằng lời của mình:

### Dir
- `claim` in ra path gì?:
- Path đó có đúng là `TODOAPP_REPO` không?:
- Tôi hiểu `dir:<repo>` là gì bằng lời của mình:

### Worktree
- `claim` in ra intended path gì?:
- Path đó có gợi ý `.worktrees/<task-id>` không?:
- Tôi hiểu `worktree` là gì bằng lời của mình:

## Role mapping
- `pm` nên bắt đầu với workspace nào? Vì sao?
- `reviewer` nên bắt đầu với workspace nào? Vì sao?
- `reviewer` nên dùng workspace nào khi ghi durable artifact? Vì sao?
- `coder` nên dùng workspace nào khi implement feature thật? Vì sao?

## Comparison bắt buộc
### Scratch vs dir:<repo>
- `scratch` mạnh ở điểm nào?
- `scratch` yếu ở điểm nào?
- `dir:<repo>` mạnh ở điểm nào?
- `dir:<repo>` yếu ở điểm nào?

### dir:<repo> vs worktree
- Khi nào `dir:<repo>` là đúng tool?
- Khi nào `worktree` là đúng tool?
- Vì sao `worktree` không phải lựa chọn mặc định cho reviewer?

## Error practice
### Relative dir path
- Tôi đã thử `dir:../relative-path-should-fail` chưa?:
- Kết quả là gì?:
- Tôi rút ra rule gì từ lỗi này?:

### Wrong tool choice
- Nếu dùng `worktree` cho reviewer-only task thì vấn đề là gì?:
- Nếu dùng `scratch` cho coding task thật thì rủi ro là gì?:

## Decision rules của tôi
- Nếu task chưa cần source of truth thật, tôi chọn:
- Nếu task phải ghi artifact thật trong repo, tôi chọn:
- Nếu task là coding thật trên git repo và cần cô lập, tôi chọn:

## Failure log
| Time | Step | Symptom | Root cause guess | What I did |
|---|---|---|---|---|
|  |  |  |  |  |

## Self-evaluation
- Tôi có phân biệt rõ 3 workspace chưa?
- Tôi có hiểu vì sao `dir:<repo>` phải là absolute path chưa?
- Tôi có hiểu `worktree` là intended coding lane chứ không phải magic clone cho mọi task chưa?
- Tôi có map đúng `pm`, `reviewer`, `coder` với workspace chưa?
- Tôi có giữ benchmark TodoApp và fixed stack trong đầu không?

## Verdict
- PASS / FAIL:
- Lý do:
- Câu tôi tự tóm tắt Day 3 trong 1-2 câu:
