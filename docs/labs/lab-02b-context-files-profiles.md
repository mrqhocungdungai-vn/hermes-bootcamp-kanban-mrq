# Lab 02B — Context Files, Skills Hygiene, Profiles, Worktrees

**Mục tiêu:** biến Level 2 thành kỹ năng vận hành thật: biết để tri thức ở đâu, biết context nào được auto-load, biết khi nào tách profile và khi nào phải tách worktree.  
**Thời lượng:** 35-50 phút.

> Lab này là phần tiếp theo của `lab-02-models-tools-skills-memory.md`. Nếu Lab 02 giúp bạn phân biệt capability vs knowledge, thì Lab 02B giúp bạn dựng **durable context architecture** đúng chuẩn Hermes docs.

## Lý thuyết cần nắm

Lab này khóa boundary của context, skills, memory, profiles, worktrees, và durable handoff. Đây là lớp nền để Level 4 không sụp khi bạn bắt đầu vận hành Jarvis như orchestrator với nhiều repo, nhiều profile, và nhiều artifact bền vững.

### Sơ đồ mental model

```text
Project knowledge routing

Repo rules -----------> [AGENTS.md / context files]
Reusable procedure ---> [Skill]
Durable fact ---------> [Memory]
Past transcript ------> [Session search]
Identity/state -------> [Profile]
Filesystem isolation -> [Worktree]
Handoff artifact -----> [File trong repo]

Nếu route sai, Level 4 sẽ sụp vì Jarvis đọc sai lớp thông tin
```

### Phần A — Đọc đúng boundary của project context

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

### Phần B — Context references: mạnh, nhưng chủ yếu là CLI feature

Đọc docs gốc phần Context References rồi tự kiểm tra lại bằng lời.

**Bài tập Feynman:**
- `@file:...`, `@folder:...`, `@diff`, `@git:N`, `@url:...` dùng để làm gì?
- Vì sao docs nói `@` syntax **primarily a CLI feature**?
- Trong Discord/Telegram, nếu gõ `@file:README.md` thì chuyện gì xảy ra?

**Đáp án mong đợi:**
- CLI mới expand `@...` trước khi gửi message
- Gateway platforms không expand `@`; ở đó agent phải dùng file/web tools thay thế

### Phần C — Skills là procedural memory, không phải note folder tùy tiện

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

### Phần D — Curator: hygiene cho skill library

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

### Phần E — Built-in memory và frozen snapshot pattern

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

### Phần F — External memory providers: biết tồn tại, chưa cần lạm dụng

```bash
hermes memory status
```

Quan sát danh sách provider trên máy, rồi trả lời:
- provider nào đang active?
- built-in memory có tắt đi khi chưa chọn external provider không?
- vì sao docs Level 2 nên dạy “hygiene trước, provider sau”?

### Phần G — Profiles là state boundary

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

### Phần H — Profile không phải workspace, cũng không phải sandbox

Đây là phần dễ nhầm nhất của Level 2.

Tự giải thích 3 câu:
1. Profile khác working directory như thế nào?
2. Profile có tự sandbox filesystem access không?
3. Điều gì mới quyết định chỗ terminal/file tools bắt đầu làm việc?

**Đáp án mong đợi:**
- profile chỉ cô lập state
- working directory do `terminal.cwd` quyết định
- local backend vẫn có filesystem access theo quyền user hiện tại; profile không tự biến thành sandbox

### Phần I — Worktrees là filesystem isolation chuẩn cho multi-agent coding

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

### Phần J — Durable handoff architecture cho Jarvis system

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

## Hiểu sai thường gặp

- Skill, memory, session search, context file đều chỉ là các kiểu “ghi chú” giống nhau.
- Profile tự động xác định workspace/project root.
- Curator là công cụ dọn skill theo kiểu xóa bừa.
- Worktree chỉ là mẹo Git, không liên quan đến multi-agent isolation.

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

```text
Jarvis, hãy làm context architect coach cho tôi trong Lab 02B.

Objective:
- giúp tôi thiết kế đúng boundary giữa AGENTS.md, SOUL.md, skills, memory, profiles, worktrees, và handoff artifacts,
- bắt tôi tự phân loại đúng từng loại thông tin,
- kết thúc bằng một context architecture ngắn gọn mà tôi có thể dùng lâu dài.

Context:
- Tôi đang học Lab 02B — Context Files, Skills Hygiene, Profiles, Worktrees.
- Tôi muốn repo-backed learning: tri thức bền phải sống trong file đúng chỗ.

Guardrails:
- đừng để tôi chỉ đọc lý thuyết mà không tự phân loại ví dụ cụ thể,
- nếu tôi lẫn profile với workspace hoặc sandbox thì phải dừng sửa ngay,
- nếu dùng path/project example thì phải nhắc tôi thay bằng path thật trên máy tôi,
- ưu tiên durable handoff thay vì transcript sống.

Deliverables:
- một sơ đồ hoặc bảng context architecture ngắn cho: AGENTS.md, SOUL.md, skills, memory, profiles, worktrees, review artifacts,
- một bảng phân loại vài ví dụ cụ thể: loại thông tin nào sống ở đâu và vì sao,
- một bản thiết kế cuối lab nêu rõ project conventions sống ở đâu, persona sống ở đâu, workflow reusable sống ở đâu, review verdict sống ở đâu.

Quality standard:
- mọi quyết định phải ưu tiên repo-backed durable artifacts thay vì transcript sống,
- nếu dùng ví dụ path/project thì phải nhắc tôi thay bằng path thật trên máy tôi,
- phải dừng ngay khi tôi lẫn profile với workspace hoặc sandbox,
- bản thiết kế cuối phải đủ cụ thể để tôi tái dùng cho project khác.
```

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

Bạn chỉ pass Lab 02B khi tự nói đúng được các cặp boundary sau:
- `AGENTS.md` vs `SOUL.md`
- skills vs memory
- profile vs workspace
- profile vs sandbox
- worktree vs profile
- built-in memory vs external memory provider
- CLI context references vs gateway chat behavior

### Dấu hiệu cần reject hoặc làm lại

- output biến `AGENTS.md` hoặc `SOUL.md` thành nơi nhét mọi thứ không có boundary
- không map được ví dụ cụ thể “thông tin nào sống ở đâu”
- tiếp tục lẫn profile với workspace, sandbox, hoặc worktree
- bản thiết kế cuối không chỉ ra artifact bền cần sống ở repo/path nào

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis việc phân loại tri thức sau mỗi task: cái gì thành skill, cái gì vào memory, cái gì sống trong context file, và artifact handoff nào phải lưu trong repo. Sau khi pass lab, hãy yêu cầu Jarvis viết một reusable handoff architecture note để bạn tái dùng cho nhiều project.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```
