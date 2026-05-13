# Level 5 — Advanced Builder và Capstone

## Mục tiêu level

Sau level này, bạn không chỉ **dùng Hermes** tốt mà còn biết **thiết kế, mở rộng, tích hợp, và debug Hermes như một builder**.

Nói ngắn gọn:
- Level 1-4 dạy bạn làm **operator/orchestrator**.
- Level 5 dạy bạn quyết định **nên mở rộng Hermes ở lớp nào** và **không mở rộng sai lớp**.

## Source docs

Level này bám trực tiếp các docs gốc sau:
- Plugins
- Built-in Plugins
- Memory Providers
- Run Local LLMs on Mac
- Using Hermes as a Python Library
- Developer Guide: Architecture
- Developer Guide: Agent Loop Internals
- Developer Guide: Prompt Assembly
- Developer Guide: Context Compression and Caching
- Developer Guide: Adding Tools
- Developer Guide: Creating Skills
- Developer Guide: Extending the CLI

---

## Lesson 5.1 — Từ operator sang builder: đổi câu hỏi

**Probe**  
Khi bạn từ operator chuyển sang builder, câu hỏi chính thay đổi như thế nào?

**Reveal**  
Operator thường hỏi:
- dùng command nào,
- bật feature nào,
- workflow nào nên chạy trước.

Builder phải hỏi thêm:
- capability mới này nên nằm ở **skill**, **plugin**, **built-in tool**, hay **hệ thống ngoài**?
- đây là chuyện của **runtime config**, **prompt/context**, hay **integration boundary**?
- nếu Hermes cư xử kỳ lạ, lỗi nằm ở **prompt**, **toolset**, **provider**, **session routing**, hay **compression**?

Mental model builder của Level 5:
1. **runtime/config/provider**
2. **context/memory/skills**
3. **orchestration/integration boundary**
4. **extensibility surface**
5. **debugging by subsystem, không đoán mò**

---

## Lesson 5.2 — Chọn đúng bề mặt mở rộng: skill vs plugin vs built-in tool

**Probe**  
Khi muốn dạy Hermes capability mới, có phải cứ viết plugin là đúng không?

**Reveal**  
Không. Docs developer guide nhấn mạnh bạn phải chọn đúng lớp:

### 1. Dùng **skill** khi:
- capability có thể diễn đạt bằng hướng dẫn + commands + tool đang có,
- bạn đang wrapper một CLI/API bằng `terminal`, `web_extract`, `browser`, v.v.,
- bạn muốn reusable procedure, không cần code mới trong core.

Ví dụ:
- git workflow,
- arXiv workflow,
- Docker workflow,
- internal SOP của team.

### 2. Dùng **plugin** khi:
- bạn cần tool/hook/slash command riêng,
- bạn cần đóng gói capability để reuse tốt hơn,
- bạn không muốn sửa Hermes core.

Plugin system trong docs cho phép:
- register tool,
- register hook,
- register slash command,
- register CLI subcommand,
- bundle skill,
- ship data files,
- thậm chí register platform/image backend/context engine/provider plugin.

### 3. Dùng **built-in tool** khi:
- bạn thực sự muốn sửa Hermes core repo,
- capability phải đi vào `tools/` + `toolsets.py`,
- đây là thứ đủ generic để ship cùng Hermes.

### 4. Đừng mở rộng trong Hermes nếu vấn đề thật ra nằm ở hệ thống ngoài
Ví dụ:
- cần một HTTP service/queue riêng,
- cần app frontend/backend riêng,
- cần automation platform riêng gọi Hermes qua webhook/API.

**Builder rule:**  
Bắt đầu bằng **skill** nếu được. Chỉ đi lên **plugin** hoặc **core tool** khi skill không còn đủ.

---

## Lesson 5.3 — Plugin discovery, enablement, và cái bẫy “thấy plugin ≠ plugin đang chạy”

**Probe**  
Nếu `hermes plugins list` thấy plugin, có nghĩa là plugin đã active chưa?

**Reveal**  
Chưa chắc. Đây là một chỗ rất dễ hiểu sai.

Docs gốc nói rõ:
- Hermes scan plugin từ nhiều nguồn: bundled, user, project, pip entry points.
- **Project-local plugins** cần bật `HERMES_ENABLE_PROJECT_PLUGINS=1` mới được dùng.
- **Bundled plugins là opt-in**, không tự bật.
- Thấy plugin trong `hermes plugins list` chỉ có nghĩa là **discover được**, chưa có nghĩa là **load và chạy**.

### Discovery order cần nhớ
1. Bundled: `<repo>/plugins/<name>/`
2. User: `~/.hermes/plugins/<name>/`
3. Project: `./.hermes/plugins/<name>/` (phải bật env)
4. Pip entry points

### Enable/disable surfaces
```bash
hermes plugins list
hermes plugins install <git-or-owner/repo>
hermes plugins enable <name>
hermes plugins disable <name>
```

### Ý nghĩa kiến trúc
- **discover** = Hermes thấy plugin tồn tại
- **enable** = plugin được allow load
- **active runtime** = plugin thực sự ảnh hưởng tool/hooks/slash commands trong session mới

**Anti-pattern:** cài plugin xong rồi quên enable, sau đó tưởng Hermes bị lỗi.

---

## Lesson 5.4 — Bundled plugin, memory provider, context engine: đều là plugin, nhưng không cùng loại

**Probe**  
Có phải plugin nào cũng được quản như nhau không?

**Reveal**  
Không. Docs phân biệt rõ nhiều nhánh plugin khác nhau.

### A. General plugins
Dùng cho:
- tools,
- hooks,
- slash commands,
- CLI extensions,
- bundled skills.

Ví dụ builder-facing:
- `disk-cleanup`
- `spotify`
- `google_meet`
- `kanban/dashboard`

### B. Memory providers
Đây là provider plugin riêng. Docs gốc nhấn mạnh:
- built-in memory luôn tồn tại,
- external memory là **additive**, không thay thế built-in memory,
- **chỉ một external provider active tại một thời điểm**.

Command surface:
```bash
hermes memory setup
hermes memory status
hermes memory off
```

### C. Context engines
Compression engine cũng là plugin surface riêng:
- built-in mặc định là `compressor`,
- plugin engine không auto-active,
- phải set `context.engine` rõ ràng.

**Mental model đúng:**  
Không nên gom tất cả vào chữ “plugin” rồi nghĩ chúng có lifecycle giống hệt nhau.

---

## Lesson 5.5 — External memory là thêm trí nhớ, không phải thuốc tiên

**Probe**  
Có nên bật memory provider sớm để Hermes “thông minh hơn” ngay không?

**Reveal**  
Không nên nghĩ như vậy. External memory làm hệ thống mạnh hơn nhưng cũng khó reasoning hơn.

Docs gốc nhấn mạnh 6 điều:
1. built-in memory (`MEMORY.md`, `USER.md`) vẫn hoạt động như cũ,
2. external provider chỉ **bổ sung**,
3. chỉ **một** provider external active,
4. provider có thể inject context + sync turns + add provider-specific tools,
5. cost/privacy/hygiene tăng lên,
6. đây là lựa chọn kiến trúc, không phải checkbox “bật cho xịn”.

### Khi nào nên bật external memory?
- bạn có workflow cross-session thực sự dài,
- bạn muốn retrieval mạnh hơn built-in memory,
- bạn hiểu dữ liệu nào được lưu ở đâu.

### Khi nào chưa nên bật?
- vẫn đang học mental model Level 1-2,
- chưa ổn session hygiene,
- chưa phân biệt được memory vs context files vs session_search.

---

## Lesson 5.6 — Python library là đường embed Hermes vào hệ thống khác

**Probe**  
Nếu muốn dùng Hermes trong app/script riêng, có phải lúc nào cũng nên shell out sang CLI không?

**Reveal**  
Không. Docs gốc có một builder path rất quan trọng: import `AIAgent` trực tiếp.

### Những điểm Python library guide nhấn mạnh
1. `chat()` là entrypoint đơn giản nhất.
2. `run_conversation()` cho full result + history.
3. Khi embed Hermes, nên dùng `quiet_mode=True` để tránh output TUI/spinner.
4. Có thể khóa capability bằng `enabled_toolsets` / `disabled_toolsets`.
5. Multi-turn state đi qua `conversation_history`.
6. Batch/concurrent processing cần **một `AIAgent` mới cho mỗi task/thread**.

### Builder use cases
- CI review bot
- internal FastAPI wrapper
- Discord/Telegram bot custom của bạn
- data pipeline cần agent reasoning ở một bước nào đó

### Điều quan trọng về boundary
Python library không phải gateway.
- **gateway** phù hợp chat service đa platform + session store + cron scheduler
- **library** phù hợp embed Hermes vào chương trình của bạn

Chọn sai boundary sẽ làm architecture rối rất nhanh.

---

## Lesson 5.7 — Local LLM trên Mac: privacy/perf knob, không phải nâng cấp miễn phí

**Probe**  
Chạy local LLM trên Mac có phải lúc nào cũng “rẻ hơn và ngon hơn” cloud không?

**Reveal**  
Không. Docs local-LLM guide rất thực tế: local là một trade-off.

### Docs gốc nhấn mạnh
- Guide này dành cho **Apple Silicon**.
- Hai hướng chính: **llama.cpp** và **omlx/MLX**.
- Hermes cần model có **ít nhất 64K context**; local model cũng phải đạt ngưỡng này.
- Với local, bạn phải nghĩ về:
  - context length,
  - KV cache,
  - RAM/unified memory,
  - latency,
  - endpoint health.

### Rule thực chiến cho learner
- Local LLM tốt khi bạn ưu tiên privacy, offline-ish workflow, hoặc muốn giảm cost cloud.
- Cloud vẫn hợp lý hơn nếu bạn cần reliability đơn giản, context lớn, ít DevOps.
- Đừng bật local chỉ vì “ngầu”; hãy bật vì có lý do rõ.

### Điều cần verify trên máy thật
- server có lên endpoint OpenAI-compatible chưa,
- model id là gì,
- context có đủ lớn chưa,
- Hermes có trỏ đúng `base_url` chưa.

---

## Lesson 5.8 — Builder phải hiểu internals đủ để debug đúng lớp

**Probe**  
Vì sao builder cần đọc `Architecture`, `Agent Loop`, `Prompt Assembly`, `Context Compression and Caching`?

**Reveal**  
Vì nếu không, bạn sẽ debug bằng cảm giác.

### A. Architecture: bản đồ hệ thống
Page `Architecture` cho bạn map lớn:
- entry points: CLI, gateway, ACP, batch runner, API server, Python library,
- core loop: `AIAgent` trong `run_agent.py`,
- tool registry/toolsets,
- session storage,
- gateway/platform adapters,
- cron/plugins/tests.

Builder đọc page này để biết: “tôi nên đào file nào trước?”

### B. Agent Loop: runtime sự thật nằm ở đâu
`Agent Loop Internals` mô tả:
- prompt build,
- provider/API mode resolution,
- interruptible API calls,
- sequential vs concurrent tool execution,
- fallback/compression/retry path.

Khi Hermes hành xử kỳ lạ, đây là page giúp bạn biết:
- model trả text hay tool calls,
- tool nào chạy song song,
- message history có hợp lệ không,
- parent/child iteration budgets nằm ở đâu.

### C. Prompt Assembly: cached vs ephemeral
Đây là insight builder rất quan trọng.

Docs gốc nói Hermes tách rõ:
- **cached system prompt state**
- **ephemeral API-call-time additions**

Điều này ảnh hưởng trực tiếp đến:
- token cost,
- prompt caching,
- session continuity,
- memory correctness.

Các layer cached quan trọng gồm:
- identity (`SOUL.md` hoặc default identity),
- tool-aware guidance,
- optional system message,
- frozen MEMORY / USER snapshot,
- skills index,
- context files,
- timestamp / session / platform hint.

**Hàm ý cho learner:**  
Nếu bạn sửa memory/config/skills mà mong session hiện tại phản ứng ngay, rất dễ hiểu sai vì nhiều thứ là snapshot theo session.

### D. Context Compression: có hai lớp, không phải một
Docs gốc nói rõ Hermes có **dual compression system**:
1. **gateway session hygiene** (~85%) — safety net trước khi agent xử lý message
2. **agent ContextCompressor** (~50% mặc định) — cơ chế chính trong loop

Điểm builder phải nhớ:
- compression không chỉ là một nút on/off,
- summary model phải có context đủ lớn, nếu không chất lượng compaction tụt mạnh,
- context engine có thể thay bằng plugin.

**Anti-pattern:** thấy session bị compress là nghĩ ngay “Hermes mất trí”. Thực ra phải hỏi nó bị compress ở lớp nào.

---

## Lesson 5.9 — Extending CLI và builder ergonomics

**Probe**  
Nếu bạn muốn Hermes CLI có panel/hotkey/slash command riêng, có phải fork bừa `run()` là cách tốt?

**Reveal**  
Không. Docs `Extending the CLI` đã mở sẵn extension seams để wrapper CLI không phải override bạo lực.

Các seam chính:
- `_get_extra_tui_widgets()`
- `_register_extra_tui_keybindings()`
- `_build_tui_layout_children()`
- `process_command()`
- `_build_tui_style_dict()`

Điểm rút ra cho course này:
- builder tốt là builder tận dụng seam có sẵn,
- không fork sâu khi chỉ cần extension hook mỏng.

---

## Lesson 5.10 — Chọn capstone theo primitive trung tâm

**Probe**  
Capstone nên chọn theo feature “thích nhất”, hay theo primitive trung tâm của hệ thống bạn muốn làm?

**Reveal**  
Nên chọn theo **primitive trung tâm**.

### Capstone A — Solo operator stack
**Primitive trung tâm:** config + tools + skills + memory + cron  
**Phù hợp khi:** bạn muốn một profile cá nhân rất mạnh, ít phụ thuộc team.

### Capstone B — Team assistant gateway
**Primitive trung tâm:** gateway + session isolation + slash/model discipline  
**Phù hợp khi:** bạn muốn một bot cho Discord/Telegram/team chat.

### Capstone C — Jarvis Kanban orchestrator
**Primitive trung tâm:** kanban board + dispatcher + profiles + worktrees + review artifacts  
**Phù hợp khi:** bạn muốn điều phối PM/coder/reviewer bền vững.

### Capstone D — Builder/extender
**Primitive trung tâm:** plugin / Python library / custom integration / internal tooling  
**Phù hợp khi:** bạn muốn thay đổi chính Hermes hoặc tích hợp Hermes vào hệ thống khác.

---

## Anti-patterns của Level 5

- Thấy thứ gì mới cũng muốn biến thành plugin
- Chưa rõ boundary đã lao vào external memory/local LLM
- Dùng Python library khi thật ra cần gateway service
- Dùng gateway khi thật ra chỉ cần một script/library call
- Cài plugin xong quên enable
- Thấy plugin trong list rồi tưởng plugin đã active
- Không hiểu cached prompt vs ephemeral layers nhưng lại cố debug bằng cảm giác
- Không biết session bị compress ở gateway hay agent loop mà đã kết luận sai

---

## Exit criteria

Bạn qua Level 5 khi bạn có thể tự trả lời rõ ràng:
- Khi nào dùng **skill**, khi nào dùng **plugin**, khi nào phải sửa **core tool**
- Plugin discovery, enablement, và runtime activation khác nhau thế nào
- External memory khác built-in memory ở đâu, và vì sao chỉ một provider active
- Python library vs gateway khác nhau ở lớp kiến trúc nào
- Local LLM trên Mac cần những verify nào trước khi dùng thật
- Nếu Hermes “kỳ”, bạn phải đọc page internals nào trước
- Bạn chọn được **một capstone** với primitive trung tâm rõ ràng

## Next

- `docs/labs/lab-05-builder-capstone.md`
- nếu muốn builder sâu hơn: quay lại source docs trong `docs/reference/hermes-docs-source-map.md`
- nếu muốn team workflow thực chiến hơn: quay lại `docs/levels/level-4-agent-teams-kanban.md` + `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
