# Lab 01 — First Chat và Session Discipline

**Mục tiêu:** dùng được first chat, hiểu session lifecycle, biết khi nào nên mở session mới.  
**Thời lượng:** 20-30 phút.

## Lý thuyết cần nắm

Lab này khóa boundary giữa chat, session, resume/continue, và profile. Mục tiêu không phải thuộc lệnh, mà là hiểu khi nào bạn nên mở hội thoại mới, khi nào nên nối tiếp transcript cũ, và khi nào phải đổi hẳn identity/profile của agent.

### Sơ đồ mental model

```text
[Single query]
    -> kiểm base chat nhanh

[Interactive session]
    -> tạo transcript sống
    -> dùng slash commands
    -> save / title / retry / new

[Resume / continue]
    -> quay lại đúng session cũ

[Profile]
    -> identity/state riêng, KHÔNG phải session
```

### Bước 1 — xem syntax chat, session, profile

```bash
hermes chat --help
hermes sessions list --help
hermes profile --help
```

### Bước 2 — chọn interface để vào chat

Quickstart của Hermes cho phép bạn bắt đầu bằng CLI cổ điển hoặc TUI mới:

```bash
hermes
hermes --tui
```

**Mục tiêu:** biết rằng cả hai dùng chung sessions, slash commands, và config; khác nhau chủ yếu ở giao diện.

### Bước 3 — chạy single-query mode (verified syntax, learner tự verify output)

```bash
hermes chat -q "Giải thích ngắn gọn session khác profile như thế nào trong Hermes? Dùng ví dụ Jarvis orchestrator và coder profile." -Q
```

**Quan sát:** single-query cho bạn cảm giác Hermes hoạt động mà không cần vào interactive REPL dài.

### Bước 4 — vào interactive session

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

### Bước 5 — thoát rồi xem session list

```bash
hermes sessions list --source cli --limit 10
```

**Mục tiêu quan sát:** bạn phải thấy session mới xuất hiện trong danh sách.

### Bước 6 — resume session

Chọn một session id từ output phía trên rồi chạy:

```bash
hermes --resume <SESSION_ID>
```

Hoặc resume gần nhất:

```bash
hermes --continue
```

### Bước 7 — nhìn profile riêng, đừng lẫn với session

```bash
hermes profile show default
```

**Mục tiêu quan sát:** session bạn vừa tạo có thể thay đổi liên tục, nhưng profile vẫn là lớp identity/state riêng của Hermes.

## Hiểu sai thường gặp

- Session chỉ là cửa sổ chat tạm thời nên không cần quản lý.
- Profile và session đều là “nơi Hermes nhớ”, nên có thể dùng thay thế nhau.
- CLI và TUI là hai hệ khác nhau chứ không phải hai interface của cùng session/config system.
- Resume một session cũ luôn tốt hơn mở session mới.

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

Mở Hermes Agent rồi paste prompt này để lab thực sự là bài tập cộng tác với Jarvis, không phải chỉ đọc lệnh rồi tự suy luận một mình.

```text
Jarvis, hãy làm session coach cho tôi trong Lab 01.

Objective:
- giúp tôi phân biệt chat, session, resume, continue, và profile,
- buộc tôi dùng Hermes thật để tạo ít nhất một session rồi resume lại,
- xác nhận tôi đã hiểu khi nào nên mở session mới.

Context:
- Tôi đang học Lab 01 — First Chat và Session Discipline.
- Tôi muốn học theo kiểu prompt-first: tôi giữ vai trò learner, bạn giữ vai trò coach.

Guardrails:
- đừng giải thích quá dài trước khi tôi thực hành,
- bắt tôi chạy thử ít nhất một single-query và một interactive session,
- nếu tôi hiểu sai session vs profile thì phải sửa thật ngắn gọn,
- cuối lab phải kiểm tra lại bằng 3 câu hỏi ngắn.

Deliverables:
- bằng chứng tôi đã chạy ít nhất 1 single-query và 1 interactive session,
- một tóm tắt ngắn: session nào vừa được tạo, session nào đã được resume,
- 3 câu kiểm tra cuối lab để xác nhận tôi thật sự hiểu boundary.

Quality standard:
- không dừng ở định nghĩa; phải buộc tôi thao tác thật trên Hermes,
- phải tách rõ interface (`hermes` / `hermes --tui`) khỏi khái niệm session/profile,
- nếu tôi trả lời sai boundary thì phải sửa ngay bằng ví dụ ngắn,
- verdict cuối phải nói rõ tôi pass hay còn mơ hồ ở điểm nào.
```

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

- Chạy được một single-query
- Biết `hermes` và `hermes --tui` đều là hai interface của cùng hệ thống session/config
- Mở được một interactive session
- Dùng ít nhất 1 slash command trong session
- Liệt kê lại được session vừa tạo
- Resume được một session cũ
- Giải thích được vì sao `profile` không đồng nghĩa với `session`

### Dấu hiệu cần reject hoặc làm lại

- chỉ có giải thích prose nhưng không có bằng chứng đã tạo/resume session thật
- output không chỉ rõ bạn cần quan sát gì sau mỗi bước
- Jarvis làm bạn lẫn giữa interface, session, và profile
- không có 3 câu kiểm tra cuối lab để buộc bạn tự đánh giá lại

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis vai trò session coach: gợi ý khi nào nên mở session mới, khi nào nên resume, và khi nào phải đổi profile vì boundary trách nhiệm đã khác. Sau mỗi lần làm việc đáng nhớ, hãy yêu cầu Jarvis rút ra một session-discipline checklist để dùng lại lâu dài.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```

### Reflection

Viết ra 3 tình huống:

1. khi nên mở session mới,
2. khi nên resume session cũ,
3. khi nên đổi hẳn profile chứ không chỉ mở session mới.

Gợi ý framing: **Jarvis** có thể giữ nhiều session theo nhiều objective khác nhau, nhưng vẫn là cùng một profile orchestrator; còn `coder` là một profile khác hẳn vì khác identity/tool boundary/trách nhiệm.
