# Level 0 — Khởi động và mental model Hermes

## Mục tiêu level

Sau level này, bạn phải trả lời được:

- Hermes là gì, và **không** là gì?
- khác nhau giữa `session`, `profile`, `repo`, `workspace`, `toolset` là gì?
- một learner mới cần chạy các bước nào để xác nhận Hermes “sống”?

## Source docs

- Getting Started / Installation
- Quickstart
- Learning Path
- Sessions
- Profiles

## Lesson 0.1 — Hermes là hệ điều hành cho tác vụ agent, không chỉ là chatbot

**Probe**  
Nếu Hermes chỉ là “một chat app có terminal”, vì sao docs lại nói về `sessions`, `profiles`, `skills`, `memory`, `gateway`, `kanban`, `cron`, `MCP`?

**Reveal**  
Hermes là một agent framework chạy qua nhiều môi trường: CLI, gateway, IDE/editor integrations, cron/webhooks, và multi-agent workflows.  
Chat chỉ là bề mặt. Giá trị thật nằm ở:

- tool calling,
- persistent sessions,
- skills/memory,
- profiles,
- automation,
- orchestration.

**Lab**  
Làm `docs/labs/lab-00-day-0-setup.md`.

**Feynman check**  
Giải thích cho người khác bằng 2 câu: “Hermes khác ChatGPT web ở điểm nào?”

## Lesson 0.2 — Day-0 setup: đừng tin là “đã cài” cho đến khi doctor/auth/status xác nhận

**Probe**  
Vì sao việc “em cài rồi” vẫn chưa đủ để bắt đầu học Kanban/team workflow?

**Reveal**  
Docs gốc nhấn mạnh một rule rất quan trọng trong Quickstart: **nếu Hermes chưa chạy được một cuộc chat bình thường, đừng layer thêm feature**. Vì vậy Day-0 của bootcamp phải xác nhận theo thứ tự:

- CLI chạy được,
- provider đã được cấu hình qua `hermes setup` hoặc `hermes model`,
- ít nhất một cuộc chat thật phản hồi bình thường,
- session lưu và resume được,
- với workflow nâng cao: Docker/gateway/kanban environment sẵn sàng khi cần.

**Lab**  
`docs/labs/lab-00-day-0-setup.md`

**Feynman check**  
Tại sao `hermes --version` pass nhưng bootcamp vẫn có thể chưa sẵn sàng, và vì sao docs gốc bắt bạn verify **real chat trước** rồi mới đụng gateway/kanban?

## Lesson 0.3 — Session là đơn vị hội thoại; profile là identity/state; repo là nơi tool làm việc

**Probe**  
Nếu mở 2 cuộc chat Hermes khác nhau, chúng có nhất thiết dùng cùng model, cùng memory, cùng config không?

**Reveal**  
Không. Docs phân tầng như sau:

- **Session**: transcript/hội thoại cụ thể, có thể resume/search/export.
- **Profile**: một Hermes home riêng, có config, `.env`, skills, memory, sessions, gateway state riêng; profile còn có thể có command alias riêng như `coder chat`.
- **Repo/CWD/workspace**: nơi terminal/file/code tools thật sự tác động; đây không tự động bằng profile directory.

Đây là nền để sau này hiểu Kanban workspaces, worktrees, và **Jarvis-orchestrator-vs-worker profiles**.

**Lab**  
`docs/labs/lab-01-first-chat-sessions.md`

**Feynman check**  
Dùng ví dụ “một profile **Jarvis** điều phối hai project khác nhau” để giải thích session khác profile ra sao.

## Lesson 0.4 — Đường học đúng: không đọc docs như phonebook

**Probe**  
`llms-full.txt` chứa rất nhiều docs. Vì sao không nên đọc từ đầu tới cuối như sách giáo khoa tuyến tính?

**Reveal**  
Vì Hermes có nhiều chiều năng lực. Docs gốc đã chia theo:

- getting started,
- user guide,
- core features,
- automation,
- messaging,
- integrations,
- guides,
- developer guide,
- reference.

Khóa học này re-map các phần đó sang **level progression** để learner không bị overload.

**Lab**  
Mở `docs/reference/hermes-docs-source-map.md` và chỉ ra 3 page nào thuộc Level 4.

**Feynman check**  
Vì sao “đọc reference trước khi có mental model” là cách học kém hiệu quả?

## Common misconceptions phải dập từ Level 0

1. “Hermes = một model.” -> Sai, Hermes là framework; model/provider chỉ là backend inference.
2. “Session = profile.” -> Sai.
3. “Có terminal tool là đủ để làm multi-agent.” -> Sai, vì durability/orchestration cần thêm delegation/kanban/cron.
4. “Cài xong là dùng team workflow được ngay.” -> Sai, còn phụ thuộc provider, skills, profiles, workspace discipline.

## Exit criteria

Chỉ qua Level 0 khi bạn làm được cả 4 việc:

- giải thích đúng `session` vs `profile` vs `repo/workspace`,
- chạy Day-0 health checks,
- biết file entrypoint tiếp theo phải đọc là gì,
- mô tả Hermes như một agent framework thay vì chatbot.

## Next

- Sang `docs/levels/level-1-core-operator.md`
- Làm `docs/labs/lab-00-day-0-setup.md`
- Làm `docs/labs/lab-01-first-chat-sessions.md`
