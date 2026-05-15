# Level 4 — Agent Teams và Kanban

## Mục tiêu level

Đây là level trung tâm của repo này. Sau level này, bạn phải biết thiết kế và vận hành workflow **Jarvis (orchestrator) -> PM -> Coder -> Reviewer** bằng Hermes một cách:

- bền vững,
- có audit trail,
- có dependency,
- có workspace discipline,
- có review artifact,
- và quan trọng nhất: **con người giữ vai trò chiến lược, Jarvis giữ vai trò orchestration/runtime**.

Nói cách khác, Level 4 không đào tạo bạn thành người ngồi gõ từng command thay Jarvis. Nó đào tạo bạn thành người:
- hiểu Kanban board là gì,
- hiểu runtime team đang vận hành ra sao,
- biết giao objective + guardrails đúng,
- biết quan sát, chẩn đoán, và can thiệp ở cấp lãnh đạo khi cần.

## Source docs

- Delegation
- Kanban Multi-Agent
- Kanban Tutorial
- Persistent Goals
- Profiles
- Git Worktrees
- Context Files

## Lesson 4.1 — Delegation, background terminal, cron, kanban: 4 primitive khác nhau

**Probe**  
Nếu chỉ cần nhờ một subagent làm việc ngắn trong cùng turn, có cần Kanban không?

**Reveal**  
Không. Chọn primitive như sau:

- **delegate_task**: subagent ngắn hạn, synchronous, parent đợi summary
- **background terminal**: process dài nhưng không nhất thiết là board task
- **cron**: durable scheduler theo thời gian
- **kanban**: durable multi-step, multi-profile, có dependency, review loop, audit trail

Đây là bài học nền số 1 để “học Kanban chuẩn hơn”.

**Lab**  
`docs/labs/lab-04-kanban-pm-coder-reviewer.md`

**Feynman check**  
Vì sao dùng Kanban cho mọi việc nhỏ là overkill?

## Lesson 4.2 — Kanban board là gì nếu nhìn đúng bản chất?

**Probe**  
Vì sao docs nhấn mạnh board bootstrapping, task status, dispatcher, runs, reclaim/block/unblock?

**Reveal**  
Kanban trong Hermes không phải bảng sticky notes trang trí. Nó là một **runtime coordination system** gồm:

- **board**: SQLite board bền vững, nơi mọi task/run/comment/link cùng sống
- **task**: work contract có `assignee`, `workspace`, `parents`, `status`, body, comments, artifacts
- **dispatcher**: thành phần nhìn các task `ready`, claim, và spawn đúng worker profile
- **runs/history**: dấu vết runtime cho từng lần worker pickup task

Vì vậy khi nói “hiểu Kanban board”, bạn nên hiểu 4 lớp câu hỏi:

1. **Board đang chứa loại công việc gì?**  
   ví dụ: board cho Todo App benchmark hay board cho project khác.
2. **Task nào phụ thuộc task nào?**  
   đây là dependency graph, không phải checklist phẳng.
3. **Ai làm task nào, ở workspace nào?**  
   assignee + workspace quyết định worker nào làm và làm ở đâu.
4. **Nếu workflow lỗi, tôi nhìn đâu để điều phối lại?**  
   `show`, `runs`, `context`, `block`, `reassign`, `reclaim`.

Học Kanban đúng nghĩa là học **workflow runtime**, không chỉ ticket list.

## Lesson 4.2a — Kanban board có những tính năng gì mà người lãnh đạo phải hiểu?

**Probe**  
Nếu bạn là người điều phối team, bạn cần hiểu gì về board ngoài chuyện “có task để giao”? 

**Reveal**  
Tối thiểu bạn phải hiểu các capability sau:

- **board creation / switching**: mỗi board là một không gian điều phối bền vững riêng
- **task creation**: mô tả objective, body, assignee, workspace
- **dependency linking**: task nào chờ task nào
- **status flow**: pending / ready / running / blocked / done
- **comments + handoff**: lưu chỉ dẫn bổ sung và audit trail
- **runs/history**: xem worker đã thử gì, fail ở đâu, summary gì
- **recovery controls**: block, unblock, reclaim, reassign
- **dispatch/runtime bridge**: gateway/dispatcher thực sự biến task ready thành worker execution

Nếu chỉ hiểu “board = chỗ giao việc”, bạn mới hiểu lớp bề mặt.  
Nếu hiểu được các capability trên, bạn mới đủ năng lực **lãnh đạo trên hệ thống**, không bị rơi xuống vai trò thư ký gõ lệnh.

## Lesson 4.2b — Con người ở tầng chiến lược; Jarvis ở tầng orchestration

**Probe**  
Nếu đã có Jarvis orchestrator, vai trò của con người còn là gì?

**Reveal**  
Con người không biến mất. Con người chỉ **leo lên tầng cao hơn**.

Phân tầng hợp lý là:

- **Con người / leader / operator chiến lược**:
  - chọn mục tiêu đúng
  - chốt scope
  - đặt guardrails
  - chốt tiêu chuẩn chất lượng
  - quyết định khi nào cần escalate / đổi hướng / dừng
- **Jarvis orchestrator**:
  - đọc objective
  - decomposition thành graph task
  - route đúng assignee/workspace
  - siết handoff giữa PM / Coder / Reviewer
  - theo dõi blockers và yêu cầu recovery phù hợp
- **Worker profiles** (`pm`, `coder`, `reviewer`):
  - thực thi task cụ thể trong graph

Nói ngắn gọn: **bạn không nên cạnh tranh với Jarvis ở tầng gõ command**.  
Bạn phải đứng **trên Jarvis** ở tầng:
- định hướng,
- kiểm chuẩn,
- đánh giá output,
- quyết định ưu tiên.

## Lesson 4.2c — Jarvis là orchestrator: học Kanban dễ nhất khi có một bộ não điều phối rõ ràng

**Probe**  
Vì sao nên dạy bằng mô hình một orchestrator tên **Jarvis** trước, thay vì nói trừu tượng về “manager” hay ném thẳng nhiều profile ngang hàng?

**Reveal**  
Vì với learner mới, một orchestrator trung tâm giúp nhìn rõ 4 động tác cốt lõi:

- nhận objective,
- decomposition thành graph task,
- route đúng assignee/workspace,
- theo dõi artifact và dispatch/recovery.

Nói ngắn gọn: **Jarvis không phải người làm hết việc; Jarvis là runtime-aware coordinator**.

## Lesson 4.2d — Có hai surface khác nhau: người vận hành dùng CLI/dashboard; worker dùng `kanban_*` tools

**Probe**  
Vì sao docs Kanban nhấn mạnh rằng code block `bash` là thứ **người** chạy, còn worker thật lại không shell ra `hermes kanban ...`?

**Reveal**  
Vì Hermes Kanban có hai cửa vào cùng một board:

- **human/operator surface**: `hermes kanban ...`, `/kanban ...`, dashboard
- **worker/model surface**: `kanban_show`, `kanban_complete`, `kanban_block`, `kanban_comment`, `kanban_link`...

Điều này quan trọng vì mental model đúng phải là:

- bạn bootstrap board, quan sát queue, recovery bằng CLI/dashboard,
- còn worker đọc task/handoff/comment/run history qua `kanban_*` toolset do dispatcher inject.

Nói cách khác: **CLI là control plane của bạn; `kanban_*` là runtime interface của worker**.

## Lesson 4.2e — Prompt trong Lab 04 thực sự làm những gì?

**Probe**  
Khi lab nói “paste prompt seed cho Jarvis”, prompt đó có vai trò gì ngoài chuyện nói chuyện bằng ngôn ngữ tự nhiên?

**Reveal**  
Prompt trong Lab 04 không phải để “nói cho vui”. Nó là cách để bạn giao cho Jarvis một **orchestration brief**.  
Nếu prompt được viết đúng, Jarvis phải làm ít nhất 6 việc:

1. **Hiểu objective tổng thể**  
   ví dụ: dựng workflow Todo App v1 theo graph Jarvis -> PM -> Coder -> Reviewer.
2. **Chốt execution shape**  
   task nào cần tồn tại, task nào là parent/child, task nào cần artifact.
3. **Gán đúng assignee và workspace**  
   PM/reviewer/docs task thường vào `dir:/...`; coding task thường vào `worktree`.
4. **Thiết kế handoff durable**  
   PM phải để lại spec artifact; reviewer phải để lại review artifact; coder không được chỉ dựa vào chat.
5. **Tự phát hiện độ mơ hồ**  
   nếu artifact path, acceptance criteria, hay dependency chưa rõ thì Jarvis phải siết lại trước dispatch.
6. **Báo cáo lại cho con người ở dạng overview**  
   task IDs, dependency graph, blocker, prerequisite thiếu.

Vì vậy, prompt tốt giúp bạn **lãnh đạo bằng ý định + guardrails**, không lãnh đạo bằng thao tác tay ở mức thấp.

## Lesson 4.3 — Assignee, parents, workspace là ba trụ của task design

**Probe**  
Tại sao nhiều người tạo task xong nhưng team workflow vẫn rối?

**Reveal**  
Vì task spec thường thiếu 3 thứ:

1. **assignee** — profile nào làm,
2. **parents** — phụ thuộc vào ai,
3. **workspace** — làm ở scratch, worktree, hay dir:path.

Rule thực chiến:
- spec/docs/review artifact -> thường `dir:/abs/path/to/repo`
- code implementation -> thường `worktree`
- disposable synthesis -> `scratch`

Nếu 3 trụ này mơ hồ, Jarvis sẽ khó route đúng và worker sẽ khó pickup đúng.

## Lesson 4.4 — Jarvis -> PM -> Coder -> Reviewer là graph, không phải bốn người nói chuyện nối đuôi

**Probe**  
Vì sao workflow team chuẩn không dừng ở “PM giao, coder code, reviewer review”?

**Reveal**  
Vì để durable, bạn cần graph explicit:

- Task 0 (Jarvis/orchestrator) nhận objective, tạo graph và rule handoff
- Task A (PM/spec) tạo artifact spec
- Task B (Coder) phụ thuộc A, sửa code trong workspace đúng
- Task C (Reviewer) phụ thuộc B, tạo artifact review
- Task D (Coder fix round 2) có thể phụ thuộc C nếu reviewer yêu cầu changes

Tức là review loop phải hiện hình trong board, không ẩn trong đầu người.

## Lesson 4.5 — Review artifact bền vững là điều kiện để coder follow-up chuẩn

**Probe**  
Vì sao repo này nhấn mạnh “review findings phải lưu file”? 

**Reveal**  
Vì coder không nên phụ thuộc vào transcript sống của reviewer session.  
Artifact review tốt nên có:

- verdict: approve / request changes
- changed files đã kiểm
- findings theo mức critical / important / minor
- next actions rõ
- file path canonical, ví dụ `docs/reviews/YYYY-MM-DD-task-review.md`

Đây khớp hoàn toàn với user preference “multi-agent handoffs persist artifacts to files”.

## Lesson 4.5b — `runs`, `context`, `block`, `reclaim`, `reassign` là recovery loop của team

**Probe**  
Nếu worker bị kẹt, spawn fail, hoặc reviewer trả về changes request, chỉ nhìn `kanban list` có đủ để điều phối tiếp không?

**Reveal**  
Không đủ. Kanban chuẩn không chỉ có create/list/show; nó còn có **runtime recovery loop**:

- `runs` để xem attempt history,
- `context` để xem worker thực sự nhìn thấy gì,
- `block` / `unblock` khi cần human input,
- `reclaim` khi worker đang chạy bị kẹt,
- `reassign --reclaim` khi cần đổi profile xử lý.

Đây là chỗ Kanban vượt xa todo list thường: không chỉ giao việc, mà còn có cơ chế **quan sát, chẩn đoán, phục hồi**.

## Lesson 4.6 — Goals không thay Kanban; goals là layer ý định, Kanban là layer execution

**Probe**  
Nếu đã có persistent goals, có cần Kanban nữa không?

**Reveal**  
Vẫn cần. Goals giữ hướng làm việc xuyên turn. Kanban giữ queue/task graph/durability/audit trail.  
Goals có thể giúp **Jarvis/orchestrator** giữ định hướng; Kanban giúp worker execution có cấu trúc.

## Lesson 4.7 — Jarvis tốt là orchestrator route đúng, không tự tay làm hết

**Probe**  
Vì sao repo này theo mô hình “**Jarvis** điều phối nhiều project bằng skills/tools” thay vì “mỗi project một manager clone”? 

**Reveal**  
Vì vai trò Jarvis/orchestrator chủ yếu là:

- đọc objective,
- decomposition,
- chọn assignee,
- chọn workspace,
- gắn dependency,
- kiểm review artifacts,
- unblock/escalate.

Nếu Jarvis có discipline tốt, một orchestrator có thể xử lý nhiều project bằng project-specific context/skills.

## Lý thuyết cần nắm

- `delegate_task`, background terminal, cron, kanban là 4 primitive khác nhau; đừng dùng Kanban như búa cho mọi việc.
- Kanban board là **runtime coordination system** với task graph, assignee, workspace, status, runs, recovery.
- Con người giữ tầng chiến lược; Jarvis giữ tầng orchestration/runtime; worker profiles giữ tầng execution.
- Có hai surface khác nhau: human/operator dùng CLI/dashboard, còn worker dùng `kanban_*` tools.
- Prompt lab tốt thực ra là **orchestration brief**: objective, graph shape, assignee/workspace, artifact, ambiguity tightening, blocker reporting.
- Durable review artifact và recovery loop là điều kiện để team workflow sống bền.


### Sơ đồ mental model

```text
[Human / Leader]
      |
      | objective + guardrails + stop/go decisions
      v
[Jarvis Orchestrator]
      |
      +--> [PM task] ------> spec artifact
      |
      +--> [Coder task] ---> code + handoff summary
      |
      +--> [Reviewer task] -> review artifact + next actions
      v
[Kanban board + dispatcher + runs]
      |
      v
Recovery loop: show -> context -> runs -> block/unblock -> reclaim/reassign

Rule gốc:
- người giữ chiến lược
- Jarvis giữ orchestration
- artifact giữ durable handoff
```

## Hiểu sai thường gặp

1. Dùng Kanban cho việc một-turn đơn giản.
2. Không hiểu board là runtime coordination system mà chỉ nghĩ nó là todo list đẹp mắt.
3. Không ghi workspace rõ.
4. Không dùng assignee chuyên trách.
5. Không lưu review artifact.
6. Quên mất worker dùng `kanban_*` tools, còn người vận hành mới dùng CLI/dashboard.
7. Biến mình thành người gõ lệnh thay Jarvis thay vì đứng ở tầng chiến lược.
8. Tạo profile rỗng nhưng không clone/setup model-provider nên dispatcher spawn xong worker vẫn không làm được việc.
9. Tạo cả graph quá sớm khi chưa biết intermediate findings.
10. Nhầm parent/child khi link task.
11. Cho coder sửa trực tiếp ở shared repo khi lẽ ra nên dùng worktree.

## Prompt lab cho Jarvis

```text
Jarvis, hãy đóng vai orchestrator coach cho Level 4.

Mục tiêu:
- giúp tôi lãnh đạo workflow Kanban ở tầng chiến lược,
- để chính bạn tạo graph và siết handoff thay vì tôi làm thư ký gõ lệnh,
- buộc team workflow sống bằng artifact và dependency rõ ràng.

Cách làm:
1. Dùng `lab-04-kanban-pm-coder-reviewer.md` làm khung chính.
2. Trước khi dispatch, hãy tự kiểm tra objective, assignee, workspace, dependency, artifact path.
3. Nếu thấy mơ hồ, hãy sửa task design trước khi chạy.
4. Trong suốt lab, báo cho tôi ở mức leader view: task graph, blockers, review artifact, recovery path nếu có lỗi.
```

## Kết quả mong đợi

- Giải thích 4 primitive: delegation / background / cron / kanban.
- Giải thích Kanban board là gì dưới góc nhìn runtime chứ không chỉ ticket list.
- Nêu được các capability chính của board: task, dependency, status, runs, recovery.
- Giải thích được hai surface: human control plane vs worker `kanban_*` runtime.
- Giải thích prompt trong Lab 04 buộc Jarvis phải làm những gì.
- Vẽ graph **Jarvis -> PM -> Coder -> Reviewer** với dependency rõ.
- Chọn đúng workspace cho spec / code / review.
- Mô tả vai trò con người ở tầng chiến lược so với Jarvis ở tầng orchestration.
- Mô tả một review artifact đủ tốt để coder follow-up.
- Nêu được ít nhất một recovery path khi worker bị kẹt (`runs` / `context` / `reclaim` / `reassign`).
- Chạy được lab board cơ bản.

## Sau lab, từ nay giao gì cho Jarvis

- decomposition objective thành graph PM -> Coder -> Reviewer,
- đề xuất assignee/workspace/dependency đúng trước khi dispatch,
- kiểm tra chất lượng handoff artifact và review artifact,
- tóm tắt board ở mức leader view: blockers, recovery options, next dispatch decisions.

## Next

- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
- `docs/levels/level-5-advanced-builder-capstone.md`
