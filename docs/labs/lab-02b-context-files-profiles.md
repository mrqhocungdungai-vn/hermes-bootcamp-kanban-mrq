# Lab 02B — Context Files, Skills Hygiene, Profiles, Worktrees

**Mục tiêu:** biến Level 2 thành kỹ năng vận hành thật: biết để tri thức ở đâu, biết context nào được auto-load, biết khi nào tách profile và khi nào phải tách worktree.  
**Thời lượng:** 35-50 phút.

> Lab này là phần tiếp theo của `lab-02-models-tools-skills-memory.md`. Nếu Lab 02 giúp bạn phân biệt capability vs knowledge, thì Lab 02B giúp bạn dựng **durable context architecture** đúng chuẩn Hermes docs.

## Phần A — Đọc đúng boundary của project context

Đọc lại `AGENTS.md` trong repo này trước.

```bash
pwd
hermes -h
```

Sau đó tự trả lời:
1. Repo này đang dùng `AGENTS.md` để dạy cái gì cho agent?
2. Vì sao docs gốc nói chỉ **một** project context type được load ở startup?
3. SOUL.md có vai trò gì khác AGENTS.md?

**Điểm cần nhớ từ docs gốc:**
- project context priority: `.hermes.md` → `AGENTS.md` → `CLAUDE.md` → `.cursorrules`
- `SOUL.md` là identity riêng, load từ `HERMES_HOME`, không phải từ repo cwd
- subdirectory `AGENTS.md` được discover dần trong lúc agent chạm vào path liên quan

## Phần B — Context references: mạnh, nhưng chủ yếu là CLI feature

Đọc docs gốc phần Context References rồi tự kiểm tra lại bằng lời.

**Bài tập Feynman:**
- `@file:...`, `@folder:...`, `@diff`, `@git:N`, `@url:...` dùng để làm gì?
- Vì sao docs nói `@` syntax **primarily a CLI feature**?
- Trong Discord/Telegram, nếu gõ `@file:README.md` thì chuyện gì xảy ra?

**Đáp án mong đợi:**
- CLI mới expand `@...` trước khi gửi message
- Gateway platforms không expand `@`; ở đó agent phải dùng file/web tools thay thế

## Phần C — Skills là procedural memory, không phải note folder tùy tiện

```bash
hermes skills list --source all | sed -n '1,80p'
```

Tự trả lời:
1. Skills sống ở đâu theo docs gốc?
2. Vì sao docs gọi skills là **on-demand knowledge documents**?
3. Progressive disclosure của skills gồm 3 tầng nào?
4. External skill directories khác local skill dir ở điểm nào?

**Điểm cần rút ra:**
- `~/.hermes/skills/` là source of truth chính
- skill là workflow/procedure có thể load khi cần, không phải chỗ dump project facts
- external skill dirs là read-only scan thêm; local version thắng nếu trùng tên

## Phần D — Curator: hygiene cho skill library

```bash
hermes curator status
```

Nếu muốn xem CLI shape thêm:

```bash
hermes curator --help
```

**Bài tập:** giải thích đúng 4 ý sau:
1. Curator động vào loại skill nào?
2. Nó có auto-delete bundled skills không?
3. Pin skill dùng để làm gì?
4. Vì sao Curator chỉ nên học sau khi đã hiểu skill boundary?

**Điểm cần nhớ từ docs gốc:**
- curator chỉ review **agent-created skills**
- không đụng bundled/hub-installed skills
- **never auto-deletes**; nặng nhất là archive có thể restore

## Phần E — Built-in memory và frozen snapshot pattern

```bash
hermes memory --help
hermes memory status
hermes sessions list --limit 5
```

**Bài tập Feynman:**
1. Built-in memory gồm 2 file nào?
2. Vì sao docs nói memory trong system prompt là **frozen snapshot**?
3. Nếu thêm memory giữa session, tại sao prompt hiện tại không tự đổi ngay?
4. Khi nào nên dùng `session_search` thay vì cố nhét mọi thứ vào memory?

**Điểm cần nhớ:**
- built-in memory luôn active, external provider là additive
- đổi memory giữa session được lưu xuống disk ngay nhưng snapshot trong prompt chỉ refresh ở session mới
- session history/search dành cho chuyện cũ không đủ stable để ở memory thường trực

## Phần F — External memory providers: biết tồn tại, chưa cần lạm dụng

```bash
hermes memory status
```

Quan sát danh sách provider trên máy, rồi trả lời:
- provider nào đang active?
- built-in memory có tắt đi khi chưa chọn external provider không?
- vì sao docs Level 2 nên dạy “hygiene trước, provider sau”?

## Phần G — Profiles là state boundary

```bash
hermes profile list
hermes profile show default
hermes profile create --help
hermes profile alias --help
hermes profile use --help
```

**Bài tập giải thích bằng ví dụ:**
- `default` profile hiện có những state nào tách biệt?
- `--clone` khác `--clone-all` ra sao?
- alias `coder` thực chất là gì theo docs?
- `hermes profile use coder` giải quyết bài toán gì?

**Điểm cần nhớ từ docs gốc:**
- profile = Hermes home riêng: config, `.env`, `SOUL.md`, memories, sessions, skills, cron, gateway state
- profile alias là wrapper command cho `hermes -p <name>`
- sticky default giống `kubectl config use-context`

## Phần H — Profile không phải workspace, cũng không phải sandbox

Đây là phần dễ nhầm nhất của Level 2.

Tự giải thích 3 câu:
1. Profile khác working directory như thế nào?
2. Profile có tự sandbox filesystem access không?
3. Điều gì mới quyết định chỗ terminal/file tools bắt đầu làm việc?

**Đáp án mong đợi:**
- profile chỉ cô lập state
- working directory do `terminal.cwd` quyết định
- local backend vẫn có filesystem access theo quyền user hiện tại; profile không tự biến thành sandbox

## Phần I — Worktrees là filesystem isolation chuẩn cho multi-agent coding

```bash
hermes -h | sed -n '1,80p'
```

Sau đó đọc lại docs gốc phần Git Worktrees và trả lời:
1. Khi nào nên dùng `hermes -w`?
2. Vì sao chỉ tách profile mà không tách worktree vẫn dễ đạp chân nhau?
3. “One worktree per Hermes experiment” nghĩa là gì trong thực chiến?

**Điểm cần nhớ:**
- coding agents song song nên có checkout riêng
- worktree boundary bảo vệ filesystem/branch history tốt hơn profile boundary
- checkpoints và rollback cũng tách theo worktree path

## Phần J — Durable handoff architecture cho Jarvis system

Viết ngắn 5-8 dòng vào notebook/log riêng của bạn để trả lời:
- Project conventions nên sống ở đâu?
- Persona/style nên sống ở đâu?
- Reusable workflow nên sống ở đâu?
- User preference bền nên sống ở đâu?
- Review verdict cho một task cụ thể nên sống ở đâu?

**Đáp án chuẩn Hermes-first:**
- project conventions → `AGENTS.md`
- persona/style → `SOUL.md`
- reusable workflow → skill
- user/environment fact bền → memory
- task-specific review verdict → file artifact trong repo / session history nếu cần tra lại

## Success criteria

Bạn chỉ pass Lab 02B khi tự nói đúng được các cặp boundary sau:
- `AGENTS.md` vs `SOUL.md`
- skills vs memory
- profile vs workspace
- profile vs sandbox
- worktree vs profile
- built-in memory vs external memory provider
- CLI context references vs gateway chat behavior
