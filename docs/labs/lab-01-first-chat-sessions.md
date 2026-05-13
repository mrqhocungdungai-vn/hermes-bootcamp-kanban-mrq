# Lab 01 — First Chat và Session Discipline

**Mục tiêu:** dùng được first chat, hiểu session lifecycle, biết khi nào nên mở session mới.  
**Thời lượng:** 20-30 phút.

## Bước 1 — xem syntax chat, session, profile

```bash
hermes chat --help
hermes sessions list --help
hermes profile --help
```

## Bước 2 — chọn interface để vào chat

Quickstart của Hermes cho phép bạn bắt đầu bằng CLI cổ điển hoặc TUI mới:

```bash
hermes
hermes --tui
```

**Mục tiêu:** biết rằng cả hai dùng chung sessions, slash commands, và config; khác nhau chủ yếu ở giao diện.

## Bước 3 — chạy single-query mode (verified syntax, learner tự verify output)

```bash
hermes chat -q "Giải thích ngắn gọn session khác profile như thế nào trong Hermes? Dùng ví dụ Jarvis orchestrator và coder profile." -Q
```

**Quan sát:** single-query cho bạn cảm giác Hermes hoạt động mà không cần vào interactive REPL dài.

## Bước 4 — vào interactive session

```bash
hermes
```

Khi vào trong session, thử các slash commands sau:

### Verified from Quickstart docs

```text
/help
/tools
/model
/save
```

### Sourced from slash-command docs, learner verify live

```text
/title foundation-lab
/new
/retry
/compress
```

## Bước 5 — thoát rồi xem session list

```bash
hermes sessions list --source cli --limit 10
```

**Mục tiêu quan sát:** bạn phải thấy session mới xuất hiện trong danh sách.

## Bước 6 — resume session

Chọn một session id từ output phía trên rồi chạy:

```bash
hermes --resume <SESSION_ID>
```

Hoặc resume gần nhất:

```bash
hermes --continue
```

## Bước 7 — nhìn profile riêng, đừng lẫn với session

```bash
hermes profile show default
```

**Mục tiêu quan sát:** session bạn vừa tạo có thể thay đổi liên tục, nhưng profile vẫn là lớp identity/state riêng của Hermes.

## Success criteria

- Chạy được một single-query
- Biết `hermes` và `hermes --tui` đều là hai interface của cùng hệ thống session/config
- Mở được một interactive session
- Dùng ít nhất 1 slash command trong session
- Liệt kê lại được session vừa tạo
- Resume được một session cũ
- Giải thích được vì sao `profile` không đồng nghĩa với `session`

## Reflection

Viết ra 3 tình huống:

1. khi nên mở session mới,
2. khi nên resume session cũ,
3. khi nên đổi hẳn profile chứ không chỉ mở session mới.

Gợi ý framing: **Jarvis** có thể giữ nhiều session theo nhiều objective khác nhau, nhưng vẫn là cùng một profile orchestrator; còn `coder` là một profile khác hẳn vì khác identity/tool boundary/trách nhiệm.
