# Level 1 — Core Operator

## Mục tiêu level

Sau level này, bạn phải vận hành Hermes như một **operator có kiểm soát**, không phải người “nhập prompt rồi cầu may”.

## Source docs

- CLI
- TUI
- Configuration
- Configuring Models
- Sessions
- Security
- Checkpoints & Rollback
- Tools / Toolsets / CLI Commands / Slash Commands

## Lesson 1.1 — Model, provider, toolset là ba layer khác nhau

**Probe**  
Nếu đổi model mà vẫn giữ toolset cũ, capability của Hermes thay đổi thế nào?

**Reveal**  
- **Model**: model inference cụ thể (vd. `gpt5.4`).
- **Provider**: backend cấp model/credential/routing.
- **Toolset**: Hermes cho phép agent dùng những công cụ nào.

Sai lầm phổ biến là trộn 3 thứ này vào một cục. Docs config/models/tools tách chúng rất rõ.

**Lab**  
`docs/labs/lab-02-models-tools-skills-memory.md`

**Feynman check**  
Tại sao đổi model không tự động bật browser/terminal/file tools?

## Lesson 1.2 — CLI/TUI là interface; session management mới là xương sống vận hành

**Probe**  
Vì sao một operator giỏi phải quan tâm `resume`, `continue`, `sessions list`, `title`, `compress`, `rollback`?

**Reveal**  
Vì agent work thật không chỉ là một turn. Session management cho phép:

- tiếp tục việc dang dở,
- giảm mất mát khi context dài,
- truy hồi transcript,
- tách các run khác nhau theo mục tiêu.

TUI/CLI chỉ là hình thức tương tác; session discipline mới quyết định tính bền vững.

Đây cũng là nền để sau này **Jarvis** có thể làm orchestrator đáng tin: nếu operator còn không quản được session, thì orchestrator sẽ sớm thành nút cổ chai hỗn loạn.

**Lab**  
`docs/labs/lab-01-first-chat-sessions.md`

**Feynman check**  
Khi nào nên tạo session mới thay vì tiếp tục session cũ?

## Lesson 1.3 — Config không phải chỗ “vọc tùy hứng”; nó là operating contract

**Probe**  
Vì sao docs có cả `config.yaml`, `.env`, precedence rules, auxiliary models, privacy, approvals, terminal backend?

**Reveal**  
Vì Hermes là một runtime nhiều thành phần. Config quyết định:

- inference backend,
- execution backend,
- display/voice,
- approval/security,
- memory/settings,
- routing phụ trợ.

Docs gốc còn nhấn mạnh 2 điều operator rất hay quên:

- `hermes config set` tự route secret vào `.env` và non-secret vào `config.yaml`,
- precedence có thứ tự rõ: CLI args > `config.yaml` > `.env` > built-in defaults.

Nếu Level 1 không chắc config mental model, Level 3-5 sẽ rất dễ gãy.

**Lab**  
`docs/labs/lab-02-models-tools-skills-memory.md`

**Feynman check**  
Tại sao secrets phải nằm ở `.env` / auth flow chứ không nhét lung tung vào markdown repo?

## Lesson 1.4 — Toolsets là biên capability; approvals và checkpoints là biên an toàn

**Probe**  
Tại sao docs tách hẳn `Tools`, `Toolsets`, `Security`, `Checkpoints & Rollback`?

**Reveal**  
Vì “có thể làm gì” và “được phép làm đến mức nào” là hai câu hỏi khác nhau.

- Toolsets mở/khóa capability.
- Approvals chặn lệnh nguy hiểm; `--yolo` chỉ bỏ approval prompts chứ không biến operator thành an toàn hơn.
- Checkpoints giảm rủi ro phá filesystem, nhưng docs gốc nói rất rõ: **checkpoints là opt-in, mặc định không bật**.
- Container backends đổi security boundary từ host sang sandbox, nên operator phải biết mình đang chạy ở đâu.

Operator phải hiểu cả capability lẫn blast radius.

**Lab**  
`docs/labs/lab-02-models-tools-skills-memory.md`

**Feynman check**  
Vì sao `--yolo` không phải là dấu hiệu trưởng thành operator?

## Lesson 1.5 — Worktree và profile chỉ nên học nhập môn ở đây; mastery để Level 2-4

**Probe**  
Vì sao docs đã nói về profiles/worktrees tương đối sớm nhưng khóa học chưa cho bạn nhảy thẳng vào team workflow?

**Reveal**  
Vì trước khi chạy multi-agent, bạn còn phải nắm:

- context hygiene,
- skills/memory,
- repo vs workspace,
- review artifacts.

Nếu bỏ qua, Kanban sẽ trở thành “nhiều agent nói lung tung”, không phải system.

Ở framing của repo này, hãy hiểu sớm rằng:
- **Jarvis** sẽ là orchestrator profile,
- worker profiles như `pm`, `coder`, `reviewer` là execution roles,
- nhưng Level 1 mới chỉ yêu cầu bạn hiểu boundary, chưa yêu cầu chạy team workflow.

## Operator checklist của Level 1

Bạn phải dùng được:

- `hermes chat`
- `hermes setup`
- `hermes config ...`
- `hermes model`
- `hermes tools list`
- `hermes skills list`
- `hermes sessions list`
- `hermes checkpoints status`
- `hermes profile show default`
- `hermes doctor`
- `hermes status`

## Exit criteria

- Giải thích được model/provider/toolset khác nhau thế nào
- Biết tìm session cũ và resume đúng
- Biết đâu là config, đâu là secret, đâu là tool boundary
- Biết vì sao approvals/checkpoints quan trọng trước khi mở rộng sang automation
- Biết vì sao operator discipline là prerequisite để sau này Jarvis orchestration không bị vỡ

## Next

- `docs/levels/level-2-context-skills-memory-profiles.md`
- `docs/labs/lab-02-models-tools-skills-memory.md`
