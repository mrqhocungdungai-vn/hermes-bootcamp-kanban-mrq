# Level 2 — Context, Skills, Memory, Profiles

## Mục tiêu level

Sau level này, bạn phải biết **cách làm cho Hermes ngày càng giỏi hơn và ít cần nhắc lại hơn**, đồng thời biết cách tách môi trường làm việc đúng để chuẩn bị cho agent teams.

## Source docs

- Skills System
- Curator
- Memory
- Memory Providers
- Context Files
- Context References
- Personality & SOUL.md
- Profiles
- Git Worktrees

## Lesson 2.1 — Skills, memory, session search: ba loại “trí nhớ” khác nhau

**Probe**  
Khi nào một thông tin nên vào skill, khi nào vào memory, khi nào chỉ cần nằm trong session transcript?

**Reveal**  
- **Skill**: procedure/workflow reusable; docs gốc gọi skills là **on-demand knowledge documents** với progressive disclosure.
- **Memory**: durable facts về user, môi trường, convention.
- **Session search/history**: recall chuyện đã làm trong quá khứ nhưng không đủ stable để thành memory.

Docs gốc còn nhấn mạnh 2 điểm rất đáng dạy sớm:

- `~/.hermes/skills/` là source of truth chính cho skill library,
- built-in memory trong prompt là **frozen snapshot per session**: đổi memory giữa session được lưu xuống disk ngay, nhưng prompt chỉ refresh ở session mới.

Nếu bạn nhét workflow vào memory, hoặc nhét task progress vào memory, bạn sẽ làm hệ thống bẩn dần.

**Lab**  
`docs/labs/lab-02-models-tools-skills-memory.md`

**Feynman check**  
Vì sao “đã sửa bug X hôm qua” thường không nên vào memory?

## Lesson 2.2 — Context files là cách dạy Hermes theo project, không theo prompt may rủi

**Probe**  
Vì sao docs nhấn mạnh `AGENTS.md`, `.hermes.md`, `CLAUDE.md`, `SOUL.md`, `.cursorrules`?

**Reveal**  
Vì project-specific context nên sống trong file, không nên lặp lại bằng prompt mỗi session.  
Level này bạn cần phân biệt:

- **SOUL.md**: giọng/persona global
- **AGENTS.md**: contract/project rules
- **Context references (@-syntax)**: nạp file/folder/diff/URL theo nhu cầu cụ thể

Hai chi tiết từ docs gốc rất quan trọng nhưng hay bị bỏ sót:

- startup chỉ load **một** project context type theo priority `.hermes.md` → `AGENTS.md` → `CLAUDE.md` → `.cursorrules`, còn `SOUL.md` là identity riêng luôn được load từ `HERMES_HOME`.
- `@file:` / `@diff` / `@url:` là **CLI-first feature**; trên gateway chat, `@...` không được expand tự động.

Đây là nền cực quan trọng để multi-agent handoff không bị lệ thuộc vào chat memory.

**Lab**  
`docs/labs/lab-02b-context-files-profiles.md`

**Feynman check**  
Dùng 3 câu để phân biệt AGENTS.md và SOUL.md.

## Lesson 2.3 — Profiles là identity boundary; worktrees là filesystem boundary

**Probe**  
Vì sao chạy nhiều agent trên cùng repo mà không có worktree/isolation dễ dẫn tới tự đạp chân nhau?

**Reveal**  
- **Profile** cô lập config/memory/skills/state.
- **Worktree** cô lập branch checkout/filesystem changes.

Docs gốc nói rất thẳng: **profile ≠ workspace ≠ sandbox**.

- working directory do `terminal.cwd` quyết định,
- profile alias chỉ là wrapper cho `hermes -p <name>`,
- local backend vẫn có filesystem access theo quyền user hiện tại.

Trong team workflow chuẩn:
- **Jarvis/orchestrator**, pm, reviewer có thể là profile riêng,
- coding tasks thường nên vào `worktree`, nhất là khi chạy song song,
- spec/review docs thường vào `dir:/absolute/path/to/repo`.

**Lab**  
`docs/labs/lab-04-kanban-pm-coder-reviewer.md`

**Feynman check**  
Nếu chỉ tách profile mà không tách workspace, còn rủi ro gì?

## Lesson 2.4 — Curator và memory providers: học tăng trưởng nhưng phải có hygiene

**Probe**  
Vì sao docs có cả Curator, Memory Providers, Honcho, v.v. thay vì chỉ memory đơn giản?

**Reveal**  
Hermes không chỉ “nhớ”, mà còn có thể quản trị cái gì đáng giữ.  
Nhưng bài học Level 2 là: **đừng lao vào external memory trước khi bạn chưa có hygiene nội bộ**.

Theo docs gốc:

- Curator chỉ review **agent-created skills**,
- không đụng bundled/hub-installed skills,
- và **never auto-deletes** — nặng nhất là archive có thể restore.

Thứ tự đúng:
1. hiểu skill/memory boundary,
2. dùng context files tốt,
3. rồi mới mở rộng qua provider/curator sâu hơn.

## Lesson 2.5 — Durable handoff là yêu cầu để Level 4 không sụp

**Probe**  
Tại sao repo này nhấn mạnh review artifact trong file?

**Reveal**  
Vì khi có nhiều agent/profile/session, chat transcript không còn là source of truth tốt nhất.  
Handoff tốt phải có:

- file path rõ,
- verdict rõ,
- changed files / next actions rõ,
- có thể được coder/reviewer khác đọc lại sau này.

Đây chính là chiếc cầu nối giữa Level 2 và Kanban Level 4.

## Exit criteria

- Phân biệt đúng skill vs memory vs session recall
- Biết vai trò của AGENTS.md / SOUL.md / context references
- Giải thích profile vs worktree đúng bằng ví dụ cụ thể
- Nói đúng rằng profile không phải sandbox và `terminal.cwd` mới là workspace start point
- Biết `@...` context references chủ yếu là CLI feature, không phải gateway magic
- Chấp nhận “artifact in repo” là chuẩn vận hành, không phải optional nicety

## Next

- `docs/levels/level-3-automation-integrations.md`
- `docs/levels/level-4-agent-teams-kanban.md`
- `docs/labs/lab-02-models-tools-skills-memory.md`
- `docs/labs/lab-02b-context-files-profiles.md`
