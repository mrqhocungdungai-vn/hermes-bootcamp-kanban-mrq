# Lab 02 — Models, Toolsets, Skills, Memory

**Mục tiêu:** phân biệt các lớp capability cốt lõi và tránh nhầm lẫn giữa runtime, knowledge, và safety boundary.  
**Thời lượng:** 30-45 phút.

> Lab này phục vụ **Level 1** là chính. Các phần về skills/memory/profile ở cuối là cầu nối sang **Level 2**.

## Phần A — Config surfaces: biết file nào chứa cái gì

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

## Phần B — Model / provider / health

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

## Phần C — Toolsets là capability boundary

```bash
hermes tools --help
hermes tools list --platform cli
```

**Quan sát:** toolset là capability boundary, không phải synonym của model.  
Đổi model không tự động bật `browser`, `terminal`, hay `file` nếu toolset/platform config không cho phép.

## Phần D — Skills là knowledge inventory, không phải tool inventory

```bash
hermes skills --help
hermes skills list --source all
```

**Quan sát:**
- toolsets trả lời câu hỏi: “agent **làm được** gì?”
- skills trả lời câu hỏi: “agent **biết workflow nào**?”

**Bài tập:** tìm 3 skill mà bạn nghĩ sẽ hữu ích cho lộ trình riêng của bạn.

## Phần E — Approval và safety boundary

Đọc lại trong docs gốc phần Security rồi trả lời:

1. `approvals.mode` có những mode nào?
2. `/yolo` hay `--yolo` thực sự bỏ qua cái gì?
3. Vì sao docs vẫn có hardline blocklist kể cả khi YOLO bật?
4. Vì sao container backend làm thay đổi security boundary?

**Gợi ý từ docs gốc:**
- `manual | smart | off`
- `--yolo` không vượt qua hardline blocklist
- local backend và docker backend không cùng blast radius

## Phần F — Checkpoints và /rollback

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

## Phần G — Profile là identity boundary (preview cho Level 2)

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

## Phần H — Skills vs memory (preview cho Level 2)

Phân loại từng item sau vào đúng bucket:

1. “User thích trả lời tiếng Việt.”
2. “Workflow review chuẩn của team là reviewer để lại file markdown verdict.”
3. “Hôm qua đã sửa bug login.”
4. “Repo này dùng Next.js + TypeScript + SQLite.”

**Đáp án mong đợi:**
- (1), (4) thường là memory facts bền hơn
- (2) thường là skill / procedural convention
- (3) thường chỉ là session/project history, không nên vào memory

## Success criteria

- Nói đúng 4 cặp phân biệt: model/provider, tool/toolset, skill/memory, session/profile
- Chỉ ra đúng nơi chứa config vs secrets
- Biết checkpoints mặc định không bật và biết đường inspect store
- Giải thích được vì sao approval boundary khác capability boundary
- Nhìn vào output `tools list` và `skills list` mà không còn lẫn giữa capability và knowledge
