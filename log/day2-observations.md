# Day 2 Observations Log

Điền file này trên **máy học của bạn** trong lúc chạy lab `docs/day2.md`.

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
- Day 1 PASS chưa?:
- Board slug đang dùng:
- `pm` profile thấy trong `hermes profile list`?:
- `coder` profile thấy trong `hermes profile list`?:
- `reviewer` profile thấy trong `hermes profile list`?:
- `hermes kanban context --help`: PASS / FAIL
- `hermes kanban comment --help`: PASS / FAIL

## TodoApp repo path
- `TODOAPP_REPO` tôi dùng là:
- Repo/path này là repo thật hay practice folder tạm?:
- Có `docs/specs/` chưa?:
- Có `docs/reviews/` chưa?:

## Task IDs
- PM2_ID:
- CODER2_ID:
- REVIEW2_ID:
- FIX2_ID (nếu có):

## State observations
### Observation 1
- Ngay sau khi tạo, task nào ở `ready`?
- Task nào ở `todo`?
- Vì sao?

### Observation 2
- Sau khi dispatch, state nào đã đổi?
- Tôi thấy `running`, `blocked`, hay `done` ở task nào?

## Context understanding
### Coder context
- `hermes kanban context "$CODER2_ID"` cho tôi thấy gì từ PM?
- PM đã handoff những gì là rõ nhất?

### Reviewer context
- `hermes kanban context "$REVIEW2_ID"` cho tôi thấy gì từ coder?
- Reviewer có đủ dữ liệu để review chưa?

## Durable artifact check
- File review mục tiêu là:
- File đó có tồn tại không?:
- Reviewer có để lại comment pointer tới file không?:
- Tôi đọc được review từ file hay chỉ từ chat/comment?:

## Review quality
- Verdict reviewer là gì?:
- Required changes reviewer nêu là gì?:
- Evidence reviewer đưa ra là gì?:
- Nếu coder follow-up phải làm tiếp, coder sẽ đọc ở đâu?:

## Follow-up loop
- Tôi có tạo FIX2_ID không?:
- Follow-up task có trỏ đúng tới file review không?:
- Tôi hiểu review loop `PM -> Coder -> Reviewer -> Coder follow-up` như thế nào bằng lời của mình:

## Comparison bắt buộc
### Chat-only vs durable artifact
- Nếu review chỉ sống trong chat, tôi sẽ gặp rủi ro gì?
1.
2.
3.

### Comment vs file
- Comment dùng để làm gì?
- File review dùng để làm gì?

### Scratch vs dir:<repo>
- Với Day 2, `scratch` yếu ở điểm nào?
- Với Day 2, `dir:$TODOAPP_REPO` mạnh ở điểm nào?

## Failure log
| Time | Step | Symptom | Root cause guess | What I did |
|---|---|---|---|---|
|  |  |  |  |  |

## Self-evaluation
- Tôi có hiểu durable handoff artifact là gì chưa?
- Tôi có đọc `context` thật chưa?
- Tôi có xác định được review file path rõ ràng chưa?
- Tôi có phân biệt được comment pointer vs source-of-truth file chưa?
- Tôi có hiểu benchmark TodoApp và stack cố định vẫn không đổi chưa?

## Verdict
- PASS / FAIL:
- Lý do:
- Câu tôi tự tóm tắt Day 2 trong 1-2 câu:
