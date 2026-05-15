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

### Editorial pass 03 — Lab 04 chuyển sang prompt-first Jarvis orchestration
**Mục tiêu pass này:**
- sửa `docs/labs/lab-04-kanban-pm-coder-reviewer.md` để không dạy learner tự tay đóng vai task-graph builder,
- chuyển lab sang hướng dùng prompt để Jarvis tự tạo graph và tự siết handoff,
- giữ CLI ở vai trò bootstrap / quan sát / recovery thay vì manual orchestration.

**Repo improvements đã áp dụng:**
- Viết lại `docs/labs/lab-04-kanban-pm-coder-reviewer.md` theo framing mới:
  - nhấn mạnh `prompt-first` là mục tiêu chính của lab,
  - giữ `hermes kanban ...` cho preflight, board bootstrap, quan sát, recovery,
  - bỏ mental model cũ kiểu learner tự copy-paste 4 block `kanban create` để thay Jarvis.
- Thêm 2 prompt mẫu:
  - prompt seed để Jarvis tự tạo graph Jarvis -> PM -> Coder -> Reviewer,
  - prompt self-review để Jarvis tự siết artifact path, task body, và dependency contract trước dispatch.
- Thêm anti-pattern section để chặn cách học sai: operator giành hết phần orchestration của Jarvis.

**Editorial findings chính:**
1. Lab Level 4 này nên dạy cách **huấn luyện Jarvis thành orchestrator**, không chỉ dạy syntax CLI.
2. CLI vẫn là control plane quan trọng, nhưng không nên trở thành default cho việc hand-author toàn bộ task graph khi mục tiêu là agent-team maturity.
3. Prompt quality + handoff quality là phần cần luyện, không chỉ command fluency.

**Verification:**
- Đã đọc lại toàn bộ `docs/labs/lab-04-kanban-pm-coder-reviewer.md` sau rewrite.
- Giữ nguyên các surface CLI cần thiết cho bootstrap/gateway/recovery.
- Không đổi benchmark framing: Todo App vẫn là scenario minh họa trừ khi learner tự chuẩn bị practice repo.

### Editorial pass 04 — Level 4 chuyển rõ sang strategic leadership over Jarvis
**Mục tiêu pass này:**
- làm phần lý thuyết nói rõ learner phải hiểu Kanban để lãnh đạo ở tầng chiến lược,
- giải thích prompt trong Lab 04 thực sự buộc Jarvis làm gì,
- nhấn mạnh board là runtime coordination system chứ không phải chỗ để người ngồi gõ lệnh hộ Jarvis.

**Repo improvements đã áp dụng:**
- Viết lại `docs/levels/level-4-agent-teams-kanban.md` để thêm rõ:
  - mục tiêu level: con người giữ tầng chiến lược, Jarvis giữ tầng orchestration/runtime,
  - định nghĩa Kanban board theo runtime model,
  - capability map của board: task/dependency/status/runs/recovery/dispatch,
  - lesson riêng về vai trò con người đứng trên Jarvis,
  - lesson riêng giải thích prompt trong Lab 04 làm 6 việc gì cho orchestration.
- Cập nhật `docs/index.md` để Level 4 route nói rõ mục tiêu là **hiểu Kanban đủ sâu để lãnh đạo**, không chỉ để Jarvis chạy thay.

**Editorial findings chính:**
1. Nếu lý thuyết không chỉ ra tầng chiến lược của con người, learner rất dễ rơi vào mode "AI assistant cao cấp + tôi làm thư ký gõ command".
2. Lab prompt chỉ có giá trị khi learner hiểu nó là một orchestration brief: objective, graph shape, assignee/workspace, handoff, ambiguity tightening, blocker reporting.
3. Dạy Kanban tốt phải làm learner hiểu board như một hệ runtime có quan sát và recovery, không chỉ là bảng giao việc.

**Verification:**
- Đã đọc lại `docs/levels/level-4-agent-teams-kanban.md` sau rewrite.
- Nội dung Level 4 hiện khớp với framing prompt-first của `docs/labs/lab-04-kanban-pm-coder-reviewer.md`.
- `docs/index.md` đã sync câu mô tả Level 4 với framing mới.

### Editorial pass 05 — khóa lại contract 2 phần: theory sâu + lab prompt-first với Jarvis
**Mục tiêu pass này:**
- chốt rõ ở mức repo framing rằng curriculum luôn có 2 phần song hành,
- làm cho learner thấy ngay từ entrypoint rằng level docs là để hiểu sâu, còn lab docs là để chỉ đạo Jarvis thực hành,
- nhấn mạnh mục tiêu cuối là học một lần để chuyển sang mode cộng tác lâu dài với Jarvis.

**Repo improvements đã áp dụng:**
- Cập nhật `README.md` để thêm:
  - mục **Contract học tập của repo này: luôn có 2 phần**,
  - triết lý `2 lớp học đi cùng nhau`,
  - hướng dẫn cách đọc mỗi cặp level/lab đúng vai trò.
- Cập nhật `docs/index.md` để:
  - thêm rule nền `level docs = lý thuyết`, `lab docs = prompt-first thực hành với Jarvis`,
  - nhắc canonical path phải đi theo nhịp **lý thuyết -> lab**.
- Cập nhật `BOOTCAMP-STATUS.md` để:
  - thêm fixed learning contract vào spec,
  - sync current milestone theo framing mới theory + lab.
- Cập nhật `AGENTS.md` để khóa non-negotiable mới: mỗi chặng luôn phải có đủ 2 phần.

**Editorial findings chính:**
1. Nếu không khóa contract 2 phần ở mức repo router, learner rất dễ coi lab như phụ lục thay vì nửa còn lại của việc học.
2. Giá trị dài hạn của repo này là giúp learner chuyển từ mode “học lại” sang mode “điều phối Jarvis” trên dự án thật.
3. Lab prompt chỉ phát huy tác dụng lâu dài khi learner hiểu rõ level doc đang dạy boundary nào.

**Verification:**
- Đã sync 4 file framing/handoff chính: `README.md`, `docs/index.md`, `BOOTCAMP-STATUS.md`, `AGENTS.md`.
- Không đổi canonical route tổng thể của repo; chỉ làm rõ contract học tập của route đó.

### Editorial pass 06 — chuẩn hóa fixed format cho toàn bộ Level 0-5
**Mục tiêu pass này:**
- chuẩn hóa toàn bộ level docs theo cùng một khung đọc nhanh ở cuối level,
- giúp learner luôn thấy rõ phần nào là lý thuyết, phần nào là hiểu sai, prompt lab, kết quả mong đợi, và phần việc có thể giao lại cho Jarvis,
- làm cho mỗi level trở thành một handoff contract rõ ràng giữa người học và Jarvis.

**Repo improvements đã áp dụng:**
- Cập nhật cả 6 file level:
  - `docs/levels/level-0-khoi-dong-va-mental-model.md`
  - `docs/levels/level-1-core-operator.md`
  - `docs/levels/level-2-context-skills-memory-profiles.md`
  - `docs/levels/level-3-automation-integrations.md`
  - `docs/levels/level-4-agent-teams-kanban.md`
  - `docs/levels/level-5-advanced-builder-capstone.md`
- Mỗi level hiện đều có đủ 5 mục cố định:
  - `Lý thuyết cần nắm`
  - `Hiểu sai thường gặp`
  - `Prompt lab cho Jarvis`
  - `Kết quả mong đợi`
  - `Sau lab thì từ nay giao gì cho Jarvis`
- Cập nhật `AGENTS.md` để khóa non-negotiable mới cho future editors: level docs phải giữ fixed format này.
- Cập nhật `BOOTCAMP-STATUS.md` để ghi nhận Level 0-5 đã được chuẩn hóa format.

**Editorial findings chính:**
1. Người học học nhanh hơn khi cuối mỗi level có một “contract tóm tắt” nhất quán thay vì phải tự suy ra từ lesson prose dài.
2. Mục `Sau lab thì từ nay giao gì cho Jarvis` giúp khóa chuyển đổi từ mode học sang mode cộng tác dài hạn.
3. Prompt lab chỉ mạnh khi level doc nói rõ learner đang luyện boundary nào và sau đó chuyển quyền phần nào cho Jarvis.

**Verification:**
- Đã kiểm tra cả 6 level đều có đủ 5 heading chuẩn hóa.
- Đã đọc lại các đoạn tail mới của Level 0-5 để xác nhận format nhất quán và không gãy router `Next`.

### Editorial pass 07 — mọi lab đều phải có prompt test copy vào Jarvis
**Mục tiêu pass này:**
- sửa learner-facing contract của lab docs để learner thực hành bằng Hermes Agent thật,
- chặn việc lab bị hiểu thành prose/reference hoặc checklist shell command thuần túy,
- chuẩn hóa đầu mỗi lab bằng một prompt test có thể copy ngay vào Jarvis.

**Repo improvements đã áp dụng:**
- Thêm mục `Prompt test để copy vào Jarvis` vào đầu cả 8 lab:
  - `docs/labs/lab-00-day-0-setup.md`
  - `docs/labs/lab-01-first-chat-sessions.md`
  - `docs/labs/lab-02-models-tools-skills-memory.md`
  - `docs/labs/lab-02b-context-files-profiles.md`
  - `docs/labs/lab-03-automation-gateway.md`
  - `docs/labs/lab-03b-routing-reliability.md`
  - `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
  - `docs/labs/lab-05-builder-capstone.md`
- Chuẩn hóa prompt section theo khung gần nhất quán: `Objective`, `Context`, `Guardrails`, `Output standard`.
- Điều chỉnh riêng `lab-04` để phần đầu file cũng có prompt ngắn copy-paste được, ngoài prompt orchestration đầy đủ ở Bước 4 và prompt self-review ở Bước 5.
- Giữ vai trò của CLI trong labs ở mức bootstrap / quan sát / recovery thay vì để learner vô thức thay Jarvis làm phần orchestration.

**Editorial findings chính:**
1. Với triết lý của repo này, learner phải mở Hermes Agent thật khi làm lab; nếu không có prompt test ở đầu file, learner rất dễ trượt về mode self-study prose.
2. Prompt-first không có nghĩa là prompt mơ hồ; learner vẫn phải được buộc nêu objective, context, guardrails, và standard.
3. Lab 04 là chỗ dễ lệch nhất: nếu phần mở đầu không có prompt rõ, learner dễ quay lại hand-author workflow bằng CLI thay vì huấn luyện Jarvis orchestrate.

**Verification:**
- Đã kiểm tra bằng script toàn bộ `docs/labs/lab-*.md`: cả 8 file đều có heading `Prompt test để copy vào Jarvis`.
- Đã xác nhận cả 8 prompt section đều có đủ 4 trường: `Objective`, `Context`, `Guardrails`, `Output standard`.
- Đã đọc lại representative snippets của Lab 00, Lab 03, Lab 04, và Lab 05 để xác nhận wording prompt-first xuất hiện ngay đầu file.
