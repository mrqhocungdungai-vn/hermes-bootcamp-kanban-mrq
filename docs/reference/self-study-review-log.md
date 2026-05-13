# Self-Study Review Log

File này ghi lại các vòng **tự học + đối chiếu docs gốc + cải thiện repo** sau khi curriculum v1 đã được viết xong.

## Review pass 01
- Review mode: đọc lại docs gốc trong `llms-full.txt` + `llms.txt`, sau đó tự học theo từng level từ thấp lên cao.
- Canonical source bundle:
  - `/tmp/hermes-docs/llms-full.txt`
  - `/tmp/hermes-docs/llms.txt`

### Level 0 — hoàn tất review
**Docs gốc đối chiếu chính:**
- Getting Started / Installation
- Quickstart
- Learning Path
- Sessions
- Profiles

**Điểm kiểm tra từ docs gốc:**
1. Quickstart ưu tiên `hermes setup` / `hermes model`, rồi **real chat**, rồi mới thêm feature khác.
2. Quickstart yêu cầu verify session resume với `hermes --continue`.
3. Sessions docs xác nhận session là transcript có thể resume/search/export.
4. Profiles docs xác nhận profile là một Hermes home riêng, không phải workspace và không phải sandbox.
5. Profiles docs còn nhấn mạnh profile có command alias riêng và `terminal.cwd` mới quyết định working directory.

**Repo improvements đã áp dụng:**
- Cập nhật `docs/levels/level-0-khoi-dong-va-mental-model.md` để nhấn mạnh rule “real chat trước, feature sau”.
- Viết lại `docs/labs/lab-00-day-0-setup.md` để:
  - dùng `hermes model` như đường recovery/provider chuẩn hơn,
  - thêm `hermes profile show default`,
  - thêm bước verify real chat.
- Viết lại `docs/labs/lab-01-first-chat-sessions.md` để:
  - thêm `hermes --tui`,
  - tách slash commands thành nhóm Quickstart-verified vs sourced-from-docs,
  - thêm `hermes sessions list --source cli --limit 10`,
  - thêm `hermes profile show default`.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command đã verify thêm trên host.

**Host checks performed trong pass này:**
```bash
hermes --help
hermes chat --help
hermes sessions list --help
hermes status --help
hermes profile --help
hermes sessions list --limit 5
hermes profile show default
```

**Kết luận Level 0:**
- Level 0 hiện khớp docs gốc hơn ở 3 điểm quan trọng: entrypoint setup/model, base-chat-first, và profile-vs-session mental model.
- Có thể chuyển sang review Level 1.

### Level 1 — hoàn tất review
**Docs gốc đối chiếu chính:**
- CLI
- TUI
- Configuration
- Security
- Checkpoints and /rollback
- Tools & Toolsets
- Slash Commands Reference
- CLI Commands Reference

**Điểm kiểm tra từ docs gốc:**
1. `config.yaml` vs `.env` có boundary rõ, và `hermes config set` tự route đúng file.
2. precedence là: CLI args > `config.yaml` > `.env` > built-in defaults.
3. approvals là security boundary; `--yolo` không vượt hardline blocklist.
4. checkpoints là **opt-in**, mặc định tắt.
5. toolsets và skills là hai lớp khác nhau: capability vs knowledge.

**Repo improvements đã áp dụng:**
- Cập nhật `docs/levels/level-1-core-operator.md` để nhấn mạnh:
  - config precedence,
  - `config set` auto-routing,
  - checkpoints mặc định opt-in,
  - `--yolo` không phải trưởng thành operator,
  - container backend làm đổi security boundary.
- Viết lại `docs/labs/lab-02-models-tools-skills-memory.md` để bám docs hơn:
  - thêm `hermes config path`, `env-path`, `check`,
  - thêm `hermes doctor`, `status --deep`, `hermes model`,
  - thêm `hermes checkpoints --help` và `status`,
  - giữ skills/memory như bridge sang Level 2 thay vì trộn lẫn ngay từ đầu.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command verified thêm trên host.

**Host checks performed trong pass này:**
```bash
hermes tools list --platform cli
hermes skills list --source all
hermes checkpoints status
hermes config path
hermes config env-path
hermes config check
hermes doctor
hermes status --deep
```

**Kết luận Level 1:**
- Level 1 hiện sát docs gốc hơn ở 4 điểm: config surfaces, runtime health, safety boundaries, và checkpoints.
- Có thể chuyển sang review Level 2.

### Level 2 — hoàn tất review
**Docs gốc đối chiếu chính:**
- Skills System
- Curator
- Persistent Memory
- Memory Providers
- Context Files
- Context References
- Personality & SOUL.md
- Profiles
- Git Worktrees

**Điểm kiểm tra từ docs gốc:**
1. Skills là **on-demand knowledge documents** với progressive disclosure; `~/.hermes/skills/` là source of truth chính.
2. Built-in memory là frozen snapshot theo session; ghi memory giữa session không tự làm prompt hiện tại refresh.
3. Startup chỉ load **một** project context type theo priority; `SOUL.md` là identity riêng từ `HERMES_HOME`.
4. `@...` context references chủ yếu là **CLI feature**; gateway chat không tự expand.
5. Profile là state boundary, không phải workspace và không phải sandbox; `terminal.cwd` mới quyết định working directory.
6. Curator chỉ review agent-created skills, không đụng bundled/hub-installed skills, và never auto-deletes.
7. Worktrees là filesystem isolation chuẩn cho multi-agent coding; docs nhấn mạnh “one worktree per Hermes experiment”.

**Repo improvements đã áp dụng:**
- Cập nhật `docs/levels/level-2-context-skills-memory-profiles.md` để nhấn mạnh:
  - skills source-of-truth + progressive disclosure,
  - frozen snapshot pattern của memory,
  - context-file priority + SOUL identity boundary,
  - `@...` chỉ là CLI-first feature,
  - profile ≠ workspace ≠ sandbox,
  - curator không auto-delete.
- Viết mới `docs/labs/lab-02b-context-files-profiles.md` để Level 2 có lab riêng, thay vì phải mượn rải rác Lab 02 và Lab 04.
- Cập nhật `README.md`, `docs/index.md`, `BOOTCAMP-STATUS.md` để route learner qua Lab 02B đúng lúc.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command verified thêm cho memory/profile/curator.

**Host checks performed trong pass này:**
```bash
hermes profile list
hermes profile create --help
hermes profile alias --help
hermes profile use --help
hermes memory --help
hermes memory status
hermes curator status
hermes -h
```

**Kết luận Level 2:**
- Level 2 hiện sát docs gốc hơn ở các boundary quan trọng nhất: context, memory, profiles, worktrees, curator hygiene.
- Có thể chuyển sang review Level 3.

### Level 3 — hoàn tất review
**Docs gốc đối chiếu chính:**
- Cron Jobs
- Hooks
- Batch Processing
- Messaging Gateway overview
- Voice Mode / Browser / Vision / TTS
- MCP
- ACP Editor Integration
- API Server
- Provider Routing
- Fallback Providers
- Credential Pools
- Configuration (auxiliary models)

**Điểm kiểm tra từ docs gốc:**
1. `hook` và `webhook` là hai lớp khác nhau: hook bám lifecycle nội bộ Hermes; webhook nhận event từ hệ thống ngoài.
2. Messaging gateway là **single background process** có per-chat session store, chạy luôn cron scheduler, và là một security boundary với allowlist/pairing/slash-command tiers.
3. Batch Processing chủ yếu dành cho **trajectory generation / eval / training data**, không phải primitive mặc định cho daily ops.
4. MCP / ACP / API Server là ba role kiến trúc khác nhau: external tools, editor-native agent, và OpenAI-compatible backend.
5. Reliability stack có nhiều tầng: credential pools (same-provider rotation) -> fallback providers (cross-provider failover) -> auxiliary task routing; `provider_routing` chỉ áp dụng cho OpenRouter.
6. `auxiliary.*.provider: auto` hiện mặc định bám main model, nên browser/vision/web_extract/approval/compression có tác động chi phí rõ nếu main model đắt.

**Repo improvements đã áp dụng:**
- Viết lại `docs/levels/level-3-automation-integrations.md` để nhấn mạnh:
  - trigger primitive taxonomy rõ hơn,
  - hook vs webhook boundary,
  - gateway như service + security boundary,
  - batch processing là training/eval primitive,
  - MCP vs ACP vs API Server,
  - reliability stack: credential pools / fallback / provider routing / auxiliary routing.
- Viết lại `docs/labs/lab-03-automation-gateway.md` để thêm hooks, pairing, gateway status/setup, và draft webhook subscription.
- Viết mới `docs/labs/lab-03b-routing-reliability.md` để tách riêng phần integration/reliability bridge thay vì nhồi vào một lab duy nhất.
- Cập nhật `README.md`, `docs/index.md`, `BOOTCAMP-STATUS.md` để route learner qua Lab 03B đúng lúc.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command verified thêm cho hooks/gateway/MCP/ACP/fallback.

**Host checks performed trong pass này:**
```bash
hermes gateway setup --help
hermes gateway status --help
hermes acp --help
hermes fallback --help
hermes webhook subscribe --help
hermes mcp add --help
hermes pairing list --help
hermes auth add --help
hermes gateway --help
hermes webhook test --help
hermes hooks --help
hermes hooks list --help
hermes hooks doctor --help
hermes fallback list --help
hermes mcp list
```

**Kết luận Level 3:**
- Level 3 hiện sát docs gốc hơn ở các boundary quan trọng nhất: trigger selection, gateway operations, integration roles, và reliability layers.
- Có thể chuyển sang review Level 4.

### Level 4 — hoàn tất review
**Docs gốc đối chiếu chính:**
- Kanban (Multi-Agent Board)
- Kanban tutorial
- Persistent Goals
- Profiles
- Git Worktrees
- Context Files
- Delegation

**Điểm kiểm tra từ docs gốc:**
1. Kanban có **hai surface**: human/operator dùng CLI, dashboard, slash commands; worker do dispatcher spawn dùng `kanban_*` tools trên cùng board DB.
2. Dispatcher chạy **trong gateway mặc định**, nên task `ready` sẽ không tự chạy nếu gateway chưa lên; `hermes kanban daemon` chỉ còn đường lui deprecated.
3. `dir:<path>` phải là **absolute path**; `worktree` là workspace riêng cho coding tasks; profile không đồng nghĩa với workspace.
4. Retry/recovery là một phần cốt lõi của Kanban: `runs`, `context`, `block/unblock`, `reclaim`, `reassign` giúp nhìn và phục hồi workflow bị kẹt.
5. Với multi-profile lab, tạo profile blank thường chưa đủ để worker chạy; docs profiles cho thấy `--clone` là đường an toàn hơn nếu muốn mang theo config/.env/SOUL từ profile hiện tại.

**Repo improvements đã áp dụng:**
- Cập nhật `docs/levels/level-4-agent-teams-kanban.md` để thêm:
  - lesson về **human control plane vs worker runtime toolset**,
  - lesson về **recovery loop** (`runs`, `context`, `reclaim`, `reassign`),
  - anti-pattern mới: tạo profile rỗng mà không clone/setup, và lẫn CLI với worker toolset.
- Viết lại `docs/labs/lab-04-kanban-pm-coder-reviewer.md` theo hướng:
  - bootstrap board bằng `init` + `boards create --switch`,
  - nhấn mạnh dispatcher-in-gateway nên cần `hermes gateway start/status`,
  - tạo profile bằng `--clone` an toàn hơn cho lab,
  - capture task id bằng shell variable thay vì placeholder tay,
  - thêm bước quan sát/recovery bằng `stats`, `context`, `runs`, `reassign --reclaim`.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command verified thêm cho Kanban Level 4.

**Host checks performed trong pass này:**
```bash
hermes kanban --help
hermes kanban boards --help
hermes kanban boards create --help
hermes kanban create --help
hermes kanban link --help
hermes kanban claim --help
hermes kanban reassign --help
hermes kanban context --help
hermes kanban runs --help
hermes profile list
hermes profile create --help
```

**Kết luận Level 4:**
- Level 4 hiện bám docs gốc tốt hơn ở đúng các điểm làm learner dễ lệch nhất: ai dùng surface nào, khi nào task thực sự chạy, và recover một team workflow bị kẹt ra sao.
- Có thể chuyển sang review Level 5.

### Level 5 — hoàn tất review
**Docs gốc đối chiếu chính:**
- Plugins
- Built-in Plugins
- Memory Providers
- Run Local LLMs on Mac
- Using Hermes as a Python Library
- Architecture
- Agent Loop Internals
- Prompt Assembly
- Context Compression and Caching
- Adding Tools
- Creating Skills
- Extending the CLI

**Điểm kiểm tra từ docs gốc:**
1. Builder phải chọn đúng **extension surface**: skill -> plugin -> built-in tool -> hệ thống ngoài; không phải capability mới nào cũng đáng thành plugin.
2. Plugin discovery, enablement, và runtime activation là ba chuyện khác nhau; **bundled plugins là opt-in**, project plugins còn cần `HERMES_ENABLE_PROJECT_PLUGINS=1`.
3. External memory là **additive** trên built-in memory và chỉ **một provider external active** tại một thời điểm.
4. Python library là boundary kiến trúc khác với gateway; khi embed `AIAgent` nên dùng `quiet_mode=True`, và concurrent/batch phải tạo **một agent mới cho mỗi task/thread**.
5. Local LLM on Mac là trade-off thực: cần endpoint OpenAI-compatible, context đủ lớn cho Hermes, và lý do rõ ràng hơn là chỉ “muốn local”.
6. Prompt assembly tách **cached system prompt** khỏi **ephemeral additions**; context compression có **hai lớp** (gateway hygiene + agent compressor), nên builder phải debug đúng subsystem.

**Repo improvements đã áp dụng:**
- Viết lại `docs/levels/level-5-advanced-builder-capstone.md` để thêm:
  - mental model builder rõ hơn,
  - decision tree **skill vs plugin vs built-in tool vs external system**,
  - plugin discovery/enablement/runtime distinction,
  - external memory như additive layer,
  - Python library boundary + `quiet_mode` + one-agent-per-thread,
  - local LLM as trade-off,
  - developer internals: architecture / agent loop / prompt assembly / dual compression.
- Viết lại `docs/labs/lab-05-builder-capstone.md` theo hướng:
  - quan sát command surfaces builder-facing thật trên host,
  - chọn capstone theo **primitive trung tâm**,
  - ép learner chốt đúng extension surface,
  - yêu cầu tạo brief file thật thay vì chỉ nghĩ trong đầu.
- Tạo mới `docs/templates/capstone-brief-template.md` để learner có output artifact bền vững cho capstone selection.
- Cập nhật `docs/reference/hermes-docs-source-map.md` với các command verified thêm cho plugin/memory surfaces.
- Cập nhật `BOOTCAMP-STATUS.md` để đánh dấu self-study pass 01 đã hoàn tất.

**Host checks performed trong pass này:**
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
hermes dashboard --help
hermes mcp --help
hermes profile --help
```

**Kết luận Level 5:**
- Level 5 hiện sát docs gốc hơn ở đúng các điểm builder dễ nhầm nhất: chọn sai lớp mở rộng, hiểu sai plugin lifecycle, lẫn library với gateway, và debug nhầm subsystem.
- Self-study review pass 01 cho toàn bộ Level 0-5 đã hoàn tất.

### Editorial pass 02 — repo coherence review hoàn tất
**Mục tiêu pass này:**
- review toàn repo như một biên tập viên khóa học,
- giảm chỗ learner dễ đi nhầm route,
- tách rõ file dành cho learner với file dành cho maintainer,
- làm rõ benchmark scenario vs practice repo thật.

**Repo improvements đã áp dụng:**
- Cập nhật `README.md` để:
  - learner bắt đầu từ `docs/index.md` thay vì `BOOTCAMP-STATUS.md`,
  - thêm mục **Cách thực hành an toàn**,
  - nói rõ Todo App hiện là **scenario minh họa** trừ khi learner tự chuẩn bị practice repo.
- Cập nhật `docs/index.md` để:
  - chốt **canonical path** rõ ràng,
  - đồng bộ quick paths với prereq tối thiểu,
  - thêm `lab-01` vào route cho người đã cài Hermes nhưng mental model còn mơ hồ.
- Cập nhật `BOOTCAMP-STATUS.md` để:
  - đổi framing sang **Source of Truth for Maintainers**,
  - nói rõ learner nên bắt đầu ở `README.md` hoặc `docs/index.md`,
  - đánh dấu pass editorial hiện tại.
- Cập nhật `docs/levels/level-2-context-skills-memory-profiles.md` để đưa Lesson 2.3 về đúng lab chính là `lab-02b`, còn `lab-04` là preview sau Level 4.
- Cập nhật `docs/labs/lab-04-kanban-pm-coder-reviewer.md` và `docs/labs/lab-05-builder-capstone.md` để thêm guidance thực hành trên fork/local copy hoặc practice repo riêng.
- Cập nhật `AGENTS.md` để sync lại non-negotiables và benchmark framing với router mới.

**Editorial findings chính:**
1. learner route phải thống nhất `README.md -> docs/index.md -> level/lab`,
2. `BOOTCAMP-STATUS.md` là file maintainer-facing, không phải learner-facing,
3. Todo App cần được nói rõ là scenario minh họa nếu chưa bundle benchmark repo thật,
4. Level 2 không nên ép learner nhảy sang Lab 04 quá sớm,
5. labs có artifact ghi vào repo phải nói rõ khuyến nghị dùng fork/local copy.

**Verification:**
- Đã re-check markdown links nội bộ: không có broken relative links.
- Đã sync các file router/handoff chính: `README.md`, `docs/index.md`, `BOOTCAMP-STATUS.md`, `AGENTS.md`.
