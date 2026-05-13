# Lab 05 — Builder Track và Capstone Design

**Mục tiêu:** chọn đúng capstone builder/operator nâng cao, và chứng minh bằng command surface + kiến trúc thật từ Hermes docs.  
**Thời lượng:** 60-90 phút.  
**Output cuối:** một file brief 1 trang cho capstone bạn chọn.  
**Khuyến nghị thực hành:** tạo brief trong **fork/local copy** của repo course, hoặc áp brief đó sang practice repo riêng nếu bạn muốn build tiếp trên codebase thật.

## Trước khi bắt đầu

Lab này không yêu cầu bạn phải code plugin ngay.  
Nó yêu cầu bạn làm đúng việc khó hơn: **chọn đúng primitive trung tâm** trước khi build.

---

## Bước 1 — Quan sát command surfaces builder-facing

Chạy các lệnh sau để nhìn rõ builder surfaces hiện có trên host:

```bash
hermes plugins --help
hermes plugins list
hermes plugins install --help
hermes plugins enable --help
hermes plugins disable --help

hermes memory --help
hermes memory status
hermes memory setup --help
hermes memory off --help

hermes mcp --help
hermes dashboard --help
hermes skills --help
hermes tools --help
hermes profile --help
```

### Câu hỏi tự kiểm
1. `plugins list` cho bạn biết điều gì, và **không** cho bạn biết điều gì?
2. `memory status` khác gì với built-in `MEMORY.md`/`USER.md` mental model bạn đã học ở Level 2?
3. Ở máy hiện tại, bạn thấy các bundled plugins nào nhưng chưa enable?

**Checkpoint:** bạn phải nói được câu này bằng lời của mình:  
> “discover được plugin” khác với “plugin đang active trong runtime”.

---

## Bước 2 — Chọn một capstone theo primitive trung tâm

Chọn **1 trong 4** capstone sau.

### Capstone A — Solo Operator Stack
**Phù hợp khi:** bạn muốn một profile cá nhân rất mạnh cho coding + docs + research + automation.  
**Primitive trung tâm:** config + tools + skills + memory + cron.

### Capstone B — Team Gateway Assistant
**Phù hợp khi:** bạn muốn bot cho Discord/Telegram/team chat.  
**Primitive trung tâm:** gateway + sessions + slash/model discipline + approvals.

### Capstone C — Jarvis Kanban Orchestrator
**Phù hợp khi:** bạn muốn Jarvis điều phối PM/coder/reviewer trên benchmark repo.  
**Primitive trung tâm:** kanban board + dispatcher + profiles + worktrees + review artifacts.

### Capstone D — Builder / Extender
**Phù hợp khi:** bạn muốn mở rộng chính Hermes hoặc embed Hermes vào hệ thống khác.  
**Primitive trung tâm:** plugin / Python library / custom integration.

---

## Bước 3 — Chốt “extension surface” đúng

Nếu bạn chọn Capstone D, bắt buộc trả lời thêm câu này:

> Capability mới của bạn nên là **skill**, **plugin**, **built-in tool**, hay **hệ thống ngoài gọi Hermes**?

Dùng bảng này:

| Nếu nhu cầu là... | Surface nên ưu tiên |
|---|---|
| Procedure dùng lại được bằng tools sẵn có | Skill |
| Tool/hook/slash command riêng, không muốn sửa core | Plugin |
| Tính năng đủ generic để ship trong Hermes core | Built-in tool |
| Service/app lớn hơn, Hermes chỉ là một phần | Hệ thống ngoài + Python library / API / webhook |

**Checkpoint:** viết 3-5 dòng giải thích vì sao bạn **không** chọn 3 surface còn lại.

---

## Bước 4 — Nếu nghiêng về plugin/memory/local-LLM, hãy viết rõ assumption kỹ thuật

### Trường hợp 1: bạn nghiêng về plugin
Phải trả lời:
- plugin của bạn đến từ đâu? bundled, user, project, hay pip?
- có cần `HERMES_ENABLE_PROJECT_PLUGINS=1` không?
- cần enable bằng `hermes plugins enable <name>` không?
- runtime mới có cần session/gateway restart hay session mới không?

### Trường hợp 2: bạn nghiêng về external memory
Phải trả lời:
- vì sao built-in memory chưa đủ?
- provider external này lưu gì thêm?
- privacy/cost/risk tăng ở đâu?
- bạn chấp nhận việc chỉ **một external provider active** như thế nào?

### Trường hợp 3: bạn nghiêng về local LLM trên Mac
Phải trả lời:
- bạn dùng llama.cpp hay omlx?
- endpoint OpenAI-compatible của bạn là gì?
- context có đạt ngưỡng tối thiểu cho Hermes chưa?
- vì sao local phù hợp hơn cloud ở capstone này?

---

## Bước 5 — Chọn source docs phải đọc lại theo capstone

### Nếu chọn Capstone A — Solo Operator
Đọc lại:
- Configuration
- Tools
- Skills System
- Memory
- Tips / best practices

### Nếu chọn Capstone B — Team Gateway Assistant
Đọc lại:
- Messaging overview + platform docs liên quan
- Sessions
- Security
- Slash commands / CLI commands liên quan gateway
- Level 3 labs của repo này

### Nếu chọn Capstone C — Jarvis Kanban Orchestrator
Đọc lại:
- Delegation
- Kanban Multi-Agent
- Kanban tutorial
- Profiles
- Git Worktrees
- Level 4 của repo này

### Nếu chọn Capstone D — Builder / Extender
Đọc lại:
- Plugins
- Built-in Plugins
- Memory Providers
- Using Hermes as a Python Library
- Architecture
- Agent Loop
- Prompt Assembly
- Context Compression and Caching
- Adding Tools / Creating Skills / Extending the CLI

---

## Bước 6 — Viết capstone brief 1 trang

Tạo file mới từ template này:

```bash
mkdir -p docs/capstones
cp docs/templates/capstone-brief-template.md docs/capstones/<ten-capstone-cua-ban>.md
```

Ví dụ:

```bash
mkdir -p docs/capstones
cp docs/templates/capstone-brief-template.md docs/capstones/jarvis-kanban-orchestrator.md
```

Sau đó điền đầy đủ 9 mục sau:

1. **Mục tiêu thực tế** là gì?
2. **Người dùng chính** là ai?
3. **Primitive trung tâm** là gì?
4. **Surface mở rộng chính** là gì? (nếu có)
5. **Profile / role** nào tham gia?
6. **Artifact bền vững** nào phải tồn tại?
7. **Failure mode lớn nhất** là gì?
8. **Docs gốc cần đọc lại trước khi build thật** là gì?
9. **Exit criteria** của capstone là gì?

---

## Bước 7 — Feynman defense

Tự bảo vệ capstone của bạn bằng 5 câu:

1. Vì sao đây là capstone đúng với mục tiêu hiện tại?
2. Primitive trung tâm của nó là gì?
3. Vì sao không chọn capstone đơn giản hơn?
4. Feature nào bạn **cố ý chưa làm** ở giai đoạn đầu để tránh scope creep?
5. Nếu capstone fail, nó có khả năng fail ở lớp nào nhất: config, context, orchestration, hay extension?

---

## Gợi ý chấm chất lượng brief

Brief tốt khi:
- không lẫn giữa **feature phụ** và **primitive trung tâm**,
- không lẫn giữa **skill** và **plugin**,
- không nói mơ hồ kiểu “xây một Hermes mạnh hơn”,
- có boundary rõ: session, profile, workspace, artifact,
- biết chính xác docs nào cần đọc lại trước khi build thật.

Brief chưa tốt khi:
- muốn làm cả gateway + kanban + plugin + memory provider + local LLM cùng lúc,
- không giải thích vì sao phải dùng external memory/plugin,
- không biết output artifact cuối là gì,
- không nói được failure mode đầu tiên cần kiểm.

---

## Success criteria

Bạn hoàn thành Lab 05 khi:
- chọn được **1 capstone rõ ràng**,
- gọi đúng tên **primitive trung tâm**,
- chọn đúng **extension surface** nếu có,
- tạo được file brief trong `docs/capstones/`,
- biết phải quay lại docs gốc nào trước khi build thật.

## Sau lab này

Nếu bạn chọn:
- **Capstone A** -> quay lại Level 2-3 để harden stack cá nhân
- **Capstone B** -> quay lại Lab 03 + 03B
- **Capstone C** -> quay lại Level 4 + Lab 04
- **Capstone D** -> đọc sâu source docs builder track rồi mới build
