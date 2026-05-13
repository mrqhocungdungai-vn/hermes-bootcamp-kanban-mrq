# Level 4 — Agent Teams và Kanban

## Mục tiêu level

Đây là level trung tâm của repo này. Sau level này, bạn phải biết thiết kế và vận hành workflow **Jarvis (orchestrator) -> PM -> Coder -> Reviewer** bằng Hermes một cách:

- bền vững,
- có audit trail,
- có dependency,
- có workspace discipline,
- có review artifact.

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

## Lesson 4.2 — Board là durable queue; task là work contract; dispatcher là runtime bridge

**Probe**  
Vì sao docs nhấn mạnh board bootstrapping, task status, dispatcher, runs, reclaim/block/unblock?

**Reveal**  
Kanban trong Hermes không phải bảng sticky notes trang trí. Nó là:

- board SQLite bền vững,
- task row có assignee/workspace/parents/status,
- dispatcher chọn task ready và spawn worker profile phù hợp.

Học Kanban đúng nghĩa là học **workflow runtime**, không chỉ ticket list.

## Lesson 4.2b — Jarvis là orchestrator: học Kanban dễ nhất khi có một bộ não điều phối rõ ràng

**Probe**  
Vì sao nên dạy bằng mô hình một orchestrator tên **Jarvis** trước, thay vì nói trừu tượng về “manager” hay ném thẳng nhiều profile ngang hàng?

**Reveal**  
Vì với learner mới, một orchestrator trung tâm giúp nhìn rõ 4 động tác cốt lõi:

- nhận objective,
- decomposition thành graph task,
- route đúng assignee/workspace,
- theo dõi artifact và dispatch/recovery.

Nói ngắn gọn: **Jarvis không phải người làm hết việc; Jarvis là runtime-aware coordinator**.

## Lesson 4.2c — Có hai surface khác nhau: người vận hành dùng CLI/dashboard; worker dùng `kanban_*` tools

**Probe**  
Vì sao docs Kanban nhấn mạnh rằng code block `bash` là thứ **người** chạy, còn worker thật lại không shell ra `hermes kanban ...`?

**Reveal**  
Vì Hermes Kanban có hai cửa vào cùng một board:

- **human/operator surface**: `hermes kanban ...`, `/kanban ...`, dashboard
- **worker/model surface**: `kanban_show`, `kanban_complete`, `kanban_block`, `kanban_comment`, `kanban_link`...

Điều này quan trọng vì mental model đúng phải là:

- bạn bootstrap board, tạo task, quan sát queue bằng CLI/dashboard,
- còn worker đọc task/handoff/comment/run history qua `kanban_*` toolset do dispatcher inject.

Nói cách khác: **CLI là control plane của bạn; `kanban_*` là runtime interface của worker**.

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

## Anti-patterns Level 4

1. Dùng Kanban cho việc một-turn đơn giản
2. Không ghi workspace rõ
3. Không dùng assignee chuyên trách
4. Không lưu review artifact
5. Quên mất worker dùng `kanban_*` tools, còn người vận hành mới dùng CLI/dashboard
6. Tạo profile rỗng nhưng không clone/setup model-provider nên dispatcher spawn xong worker vẫn không làm được việc
7. Tạo cả graph quá sớm khi chưa biết intermediate findings
8. Nhầm parent/child khi link task
9. Cho coder sửa trực tiếp ở shared repo khi lẽ ra nên dùng worktree

## Exit criteria

Bạn chỉ qua Level 4 khi có thể:

- giải thích 4 primitive: delegation / background / cron / kanban,
- giải thích được hai surface: human control plane vs worker `kanban_*` runtime,
- vẽ graph **Jarvis -> PM -> Coder -> Reviewer** với dependency rõ,
- chọn đúng workspace cho spec / code / review,
- mô tả một review artifact đủ tốt để coder follow-up,
- nêu được ít nhất một recovery path khi worker bị kẹt (`runs` / `context` / `reclaim` / `reassign`),
- chạy được lab board cơ bản.

## Next

- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
- `docs/levels/level-5-advanced-builder-capstone.md`
