# Lab 02 — Models, Toolsets, Skills, Memory

**Mục tiêu:** phân biệt các lớp capability cốt lõi và tránh nhầm lẫn giữa runtime, knowledge, và safety boundary.  
**Thời lượng:** 30-45 phút.

> Lab này phục vụ **Level 1** là chính. Các phần về skills/memory/profile ở cuối là cầu nối sang **Level 2**.

## Lý thuyết cần nắm

Lab này giúp bạn nhìn Hermes như một hệ có nhiều lớp boundary: config surfaces, model/provider, toolsets, skills, approvals, checkpoints, và profile preview. Nếu lẫn các lớp này, bạn sẽ dùng Hermes như một hộp đen thay vì như một operator có kiểm soát.

### Sơ đồ mental model

```text
Operator stack

[config / env precedence]
        |
        v
[provider -> model]
        |
        v
[toolsets -> capability boundary]
        |
        +--> [skills -> knowledge on demand]
        |
        +--> [approvals / checkpoints -> safety + recovery]
        v
[profile preview -> state boundary]

Đừng gộp tất cả thành một chữ "settings"
```

### Phần A — Config surfaces: biết file nào chứa cái gì

```bash
hermes config --help
hermes config path
hermes config env-path
hermes config check
```

**Quan sát cần rút ra:**
- `config.yaml` chứa non-secret settings
- `.env` chứa secrets / API keys / tokens
- `hermes config check` giúp bạn thấy config còn thiếu hay đã lỗi thời

**Bài tập nói thành lời:**
1. Vì sao API key không nên viết vào markdown repo?
2. `hermes config set OPENROUTER_API_KEY ...` sẽ đi vào file nào?
3. Khi CLI arg và config file mâu thuẫn, cái nào thắng?

### Phần B — Model / provider / health

```bash
hermes status --deep
hermes doctor
hermes model
```

**Quan sát:**
- `status --deep` và `doctor` là hai góc nhìn khác nhau về cùng runtime
- `hermes model` là entrypoint chuẩn để sửa provider/model khi chat bị lỗi
- operator tốt không đoán mò provider state; operator đọc health output

**Câu hỏi Feynman:**
- model là gì?
- provider là gì?
- tại sao `doctor` pass quan trọng hơn cảm giác “hình như chạy được”?

### Phần C — Toolsets là capability boundary

```bash
hermes tools --help
hermes tools list --platform cli
```

**Quan sát:** toolset là capability boundary, không phải synonym của model.  
Đổi model không tự động bật `browser`, `terminal`, hay `file` nếu toolset/platform config không cho phép.

### Phần D — Skills là knowledge inventory, không phải tool inventory

```bash
hermes skills --help
hermes skills list --source all
```

**Quan sát:**
- toolsets trả lời câu hỏi: “agent **làm được** gì?”
- skills trả lời câu hỏi: “agent **biết workflow nào**?”

**Bài tập:** tìm 3 skill mà bạn nghĩ sẽ hữu ích cho lộ trình riêng của bạn.

### Phần E — Approval và safety boundary

Đọc lại trong docs gốc phần Security rồi trả lời:

1. `approvals.mode` có những mode nào?
2. `/yolo` hay `--yolo` thực sự bỏ qua cái gì?
3. Vì sao docs vẫn có hardline blocklist kể cả khi YOLO bật?
4. Vì sao container backend làm thay đổi security boundary?

**Gợi ý từ docs gốc:**
- `manual | smart | off`
- `--yolo` không vượt qua hardline blocklist
- local backend và docker backend không cùng blast radius

### Phần F — Checkpoints và /rollback

```bash
hermes checkpoints --help
hermes checkpoints status
hermes chat --help
```

**Quan sát cần rút ra:**
- checkpoints là **opt-in**, mặc định không bật
- CLI có `--checkpoints` cho từng session
- `hermes checkpoints status` cho bạn thấy store hiện có gì
- `/rollback` là in-session restore flow; `hermes checkpoints ...` là shell-side inspection/maintenance

### Phần G — Profile là identity boundary (preview cho Level 2)

```bash
hermes profile --help
hermes profile list
hermes profile show default
hermes profile create --help
```

**Bài tập:** giải thích vì sao “profile = identity/state boundary” còn “session = conversation boundary”.

Gợi ý trả lời bằng ví dụ:
- `jarvis` = profile orchestrator,
- `coder` = profile implementer,
- mỗi profile có thể có nhiều session khác nhau theo từng objective/project.

### Phần H — Skills vs memory (preview cho Level 2)

Phân loại từng item sau vào đúng bucket:

1. “User thích trả lời tiếng Việt.”
2. “Workflow review chuẩn của team là reviewer để lại file markdown verdict.”
3. “Hôm qua đã sửa bug login.”
4. “Repo này dùng Next.js + TypeScript + SQLite.”

**Đáp án mong đợi:**
- (1), (4) thường là memory facts bền hơn
- (2) thường là skill / procedural convention
- (3) thường chỉ là session/project history, không nên vào memory

## Hiểu sai thường gặp

- Model, provider, toolset, skill đều là “setting của Hermes” nên có thể gộp một cục.
- Skills là inventory của tool hoặc plugin.
- Bật `--yolo` là dấu hiệu trưởng thành operator.
- Checkpoints mặc định luôn có sẵn nên không cần quan tâm trạng thái.

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

Mở Hermes Agent rồi paste prompt này. Mục tiêu của lab là để Jarvis kiểm tra boundary cùng bạn, không phải để bạn chỉ chạy `--help` rồi tự đoán.

```text
Jarvis, hãy đóng vai boundary coach cho Lab 02.

Objective:
- giúp tôi phân biệt đúng model, provider, toolset, skill, memory, và approval boundary,
- dẫn tôi đi qua các command surfaces cần nhìn,
- bắt tôi tự giải thích lại bằng lời của mình sau mỗi cụm khái niệm.

Context:
- Tôi đang học Lab 02 — Models, Toolsets, Skills, Memory.
- Tôi muốn học để sau này chỉ đạo Jarvis đúng boundary, không học mẹo prompt ngắn hạn.

Guardrails:
- đừng chỉ cho đáp án ngay; hãy bắt tôi trả lời ngắn trước,
- nếu tôi lẫn capability với knowledge thì dừng lại sửa ngay,
- ưu tiên command quan sát an toàn như config/status/doctor/tools/skills/memory,
- không được biến lab thành checklist shell khô khan không có agent loop.

Deliverables:
- một bảng boundary tối thiểu cho các cặp: model/provider, tool/toolset, skill/memory, session/profile,
- một map ngắn: config nào ở đâu, secret nào ở đâu, checkpoint state nằm ở đâu,
- một bảng cuối lab gồm 2 cột: tôi đang hiểu đúng gì, tôi còn lẫn gì.

Quality standard:
- mỗi kết luận phải bám vào command surface hoặc docs-backed behavior đã nhìn thấy,
- phải sửa ngay nếu tôi lẫn capability boundary với knowledge boundary,
- phải chỉ rõ approval boundary khác capability boundary ở đâu,
- không được biến lab thành danh sách lệnh không có câu chốt boundary sau mỗi phần.
```

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

- Nói đúng 4 cặp phân biệt: model/provider, tool/toolset, skill/memory, session/profile
- Chỉ ra đúng nơi chứa config vs secrets
- Biết checkpoints mặc định không bật và biết đường inspect store
- Giải thích được vì sao approval boundary khác capability boundary
- Nhìn vào output `tools list` và `skills list` mà không còn lẫn giữa capability và knowledge

### Dấu hiệu cần reject hoặc làm lại

- chỉ liệt kê command mà không chốt được boundary sau mỗi phần
- giải thích skills như một “danh sách tool khác” hoặc memory như “note folder tùy ý”
- không chỉ ra được config vs secrets vs checkpoint store
- verdict cuối không cho thấy bạn còn lẫn gì để quay lại học tiếp

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis việc kiểm tra capability boundary trước khi làm việc: đang dùng model/provider nào, toolsets nào, skill nào cần nạp, và safety boundary nào cần giữ. Khi qua lab này, hãy yêu cầu Jarvis đóng gói một operator checklist cho các việc cấu hình và health-check lặp lại.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```
