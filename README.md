# Hermes Bootcamp Kanban MRQ

Khóa học Hermes-first, repo-backed, viết lại từ đầu để bám **tài liệu gốc Hermes Agent** và ưu tiên đúng trục học: **Kanban + agent team + automation**, không trôi sang kiểu học app-first.

## About khóa học này

Đây là một **curriculum repo bằng tiếng Việt** để học Hermes Agent theo kiểu **repo-backed, theory-first, và Jarvis-directed**.

Repo này tồn tại để giúp bạn chuyển từ mode:
- "chat với AI cho từng việc lẻ",

sang mode:
- "điều phối một hệ thống agent có session, profile, skills, memory, automation, Kanban, review loop, và durable handoff".

Điểm quan trọng của phần about này là:
- đây là **repo học tập**, không phải product app repo,
- đây là **repo để hiểu Hermes đúng mental model**, không phải prompt cheat sheet ngắn hạn,
- đây là **repo để học cộng tác lâu dài với Jarvis như orchestrator**, không phải nơi learner tự làm thay toàn bộ orchestration,
- đây là **repo-backed workflow**, nên tri thức quan trọng phải sống trong file và artifact, không chỉ trong chat.
- đây là repo để **học một lần -> hiểu sâu -> đóng gói -> tái sử dụng -> chia sẻ**, chứ không học để giữ riêng cho một người.

Nếu bạn chỉ muốn một Todo App hoàn chỉnh để chạy production, repo này không phải đích cuối.  
Nếu bạn muốn học cách dùng Hermes để thiết kế và điều phối workflow kiểu Jarvis -> PM -> Coder -> Reviewer một cách bền vững, đây là đúng repo.

## Mục tiêu repo

Repo này dùng để học Hermes theo thứ tự từ **cơ bản -> vận hành -> automation -> agent teams -> builder level**.  
Benchmark xuyên suốt là **Todo App scenario** để có ngữ cảnh thực chiến, nhưng trọng tâm là:

1. hiểu Hermes như một hệ điều hành cho agent work,
2. hiểu khi nào dùng session / profile / skill / memory / cron / delegation / kanban,
3. dựng được workflow **Jarvis (orchestrator) -> PM -> Coder -> Reviewer** với handoff artifact bền vững trong repo,
4. tiến tới một **Jarvis orchestrator** có thể điều phối nhiều project bằng Kanban thay vì “mỗi việc một bot riêng”.

## Contract học tập của repo này: luôn có 2 phần

Mỗi chặng học trong repo này phải có đủ **2 phần bổ sung cho nhau**:

1. **Phần lý thuyết cốt lõi**  
   - bám docs gốc Hermes để learner hiểu sâu boundary và mental model,
   - trả lời câu hỏi **vì sao** phải làm như vậy,
   - giúp learner không bị lệ thuộc vào mẹo prompt ngắn hạn.

2. **Phần lab thực hành dạng prompt cho Jarvis**  
   - biến lý thuyết thành bài tập điều phối thật,
   - learner ra objective + guardrails + tiêu chuẩn đầu ra,
   - Jarvis học cách cùng người học làm việc, cùng học, cùng phát triển trên dự án thật.

Boundary viết lab trong repo này:
- `lab-00` được phép bootstrap-heavy hơn vì đây là bài khởi động.
- Từ `lab-01` trở đi, lab phải prompt-first thật sự: lý thuyết trong lab giữ rất ngắn, còn giải thích sâu phải sống ở `docs/levels/*`.

Mục tiêu cuối cùng **không** phải để người học cứ lặp lại việc “học lại Hermes từ đầu” mỗi lần mở project mới.  
Mục tiêu là: **học một lần cho ra mental model đúng, rồi chuyển sang mode giao việc/đồng hành với Jarvis như một orchestrator lâu dài**.
Sau mỗi chặng, learner nên ép Jarvis giúp mình **đóng gói điều vừa học thành prompt template, checklist, handoff note, hoặc skill draft** để dùng lại lâu dài và có thể chia sẻ cho người đến sau.

## Nguồn gốc khóa học

Tài liệu này được tổ chức lại từ 2 nguồn ưu tiên cao nhất mà bạn yêu cầu đọc trước:

- `https://hermes-agent.nousresearch.com/docs/llms-full.txt`
- `https://hermes-agent.nousresearch.com/docs/llms.txt`

**Thời điểm ingest:** 2026-05-13 16:42:10 +07  
**llms-full.txt:** 2332543 bytes, SHA256 `0586901bdecc2e0a5b60853b15405b1813cd3e9b523f653cd7012dfd7c3144bb`  
**llms.txt:** 17422 bytes, SHA256 `57ba8901c6fef4c8aa6c5dd8f68575b4c526c6eecf24fc1ce68ef831d70ff807`

Xem bản đồ nguồn đầy đủ ở: `docs/reference/hermes-docs-source-map.md`

## Ai nên học repo này

- Người đã cài Hermes nhưng chưa có mental model chắc về session, profile, toolsets, skills, memory.
- Người muốn học **agent team chuẩn Hermes docs**, đặc biệt là `delegation`, `kanban`, `profiles`, `worktrees`, `cron`, `gateway`.
- Người muốn chuyển từ “chat với AI” sang “điều hành hệ thống agent có artifact, queue, review, và audit trail”.

## Bắt đầu từ đâu

### Nếu bạn đã cài Hermes rồi

Đọc theo thứ tự này:

1. `docs/index.md`
2. `docs/levels/level-0-khoi-dong-va-mental-model.md` (skim nhanh)
3. `docs/levels/level-1-core-operator.md`
4. `docs/labs/lab-01-first-chat-sessions.md`
5. tiếp tục theo đúng router trong `docs/index.md`

### Nếu bạn mới hoàn toàn

Đọc và làm lab theo thứ tự:

1. `docs/levels/level-0-khoi-dong-va-mental-model.md`
2. `docs/labs/lab-00-day-0-setup.md`
3. `docs/labs/lab-01-first-chat-sessions.md`
4. tiếp tục tuần tự qua từng level

## Cấu trúc level

| Level | Trọng tâm | Kết quả đầu ra |
|---|---|---|
| 0 | Khởi động + mental model | Biết Hermes là gì, setup/doctor/auth, first chat, session/profile/repo khác nhau ra sao |
| 1 | Core operator | Dùng CLI/TUI, config, model/provider, toolsets, sessions, security, checkpoints |
| 2 | Context + skills + memory + profiles | Biết dạy Hermes, nhớ cái gì, nạp context đúng, tách profile/worktree đúng |
| 3 | Automation + integrations | Chọn đúng trigger primitive, vận hành gateway an toàn, hiểu MCP/ACP/API Server, và thiết kế reliability/cost layers |
| 4 | Agent teams + Kanban | Thiết kế và chạy team **Jarvis -> PM -> Coder -> Reviewer** bằng board bền vững |
| 5 | Advanced builder + capstone | Plugin, memory providers, local LLM, Python library, developer internals, capstone |

## Triết lý dạy học của repo này

- **Hermes-first**: học cơ chế Hermes trước, không nhảy ngay vào code app.
- **2 lớp học đi cùng nhau**: level docs lo phần hiểu sâu; lab docs lo phần biến hiểu biết đó thành prompt/workflow điều phối với Jarvis.
- **Đóng gói để dùng lại**: qua lab rồi thì không dừng ở “đã làm xong”, mà phải rút ra asset tái sử dụng và nếu phù hợp thì chia sẻ cho team/cộng đồng.
- **Repo-backed**: tri thức nằm trong file, không nằm trong chat transient.
- **Một concept mỗi lesson**: tránh nhồi nhiều khái niệm gần nhau như model/provider/toolset trong một lúc.
- **Probe -> Reveal -> Lab -> Feynman check**: luôn có nhịp phát hiện hiểu nhầm rồi mới thực hành.
- **Artifact-driven teamwork**: reviewer phải để lại artifact bền vững, coder đọc artifact đó để sửa.
- **Kanban khi cần durability, audit trail, review loop, hoặc multi-profile**; không lạm dụng cho tác vụ nhỏ.
- **Jarvis làm orchestrator**: course dùng một orchestrator trung tâm để learner dễ hiểu cơ chế route/decompose/dispatch trước khi nhân rộng thành team phức tạp hơn.

## Cách đọc đúng mỗi level/lab pair

Với mỗi level, hãy đi theo nhịp này:

1. đọc **level doc** để nắm lý thuyết, boundary, và mental model,
2. làm **lab tương ứng** để buộc Jarvis thực thi đúng mental model đó,
3. sau lab, tự trả lời lại bằng lời của mình: từ nay phần nào bạn làm ở tầng chiến lược, phần nào giao lại cho Jarvis.
4. nếu đã đánh giá được output, yêu cầu Jarvis đóng gói lại thành skill draft, prompt template, checklist, hoặc handoff note để lần sau không phải học lại từ đầu.

Nếu bạn chỉ đọc lý thuyết mà không làm lab, bạn sẽ hiểu nhưng chưa vận hành được.  
Nếu bạn chỉ copy prompt trong lab mà không hiểu level, bạn sẽ chạy được một lần nhưng không trưởng thành thành người lãnh đạo Jarvis.

## Prerequisites tối thiểu

- Hermes Agent đã cài được (`hermes --version` chạy được)
- Có ít nhất một provider dùng được (`hermes auth list` hoặc provider trong config)
- Biết dùng terminal cơ bản
- Biết Git cơ bản
- Với các lab nâng cao: nên có Docker và/hoặc gateway environment khi muốn học messaging/backend isolation

## Cách thực hành an toàn

Repo này là **curriculum repo**. Bạn có 2 cách thực hành an toàn:

1. **Fork hoặc copy local repo này** nếu bạn muốn làm lab và ghi artifact trực tiếp vào `docs/`.
2. **Dùng practice repo riêng** nếu bạn muốn áp dụng workflow Hermes/Kanban vào codebase khác.

Lưu ý quan trọng:

- Repo này **chưa bundle sẵn một benchmark app repo thật** cho Todo App end-to-end.
- Vì vậy, hiện tại hãy hiểu **Todo App** trong course là **scenario minh họa xuyên suốt**.
- Nếu bạn muốn capstone thực chiến hơn, hãy tự chuẩn bị một practice repo Todo App riêng rồi map workflow của course sang repo đó.

## File map

### Entry points
- `README.md` — entrypoint cấp repo
- `BOOTCAMP-STATUS.md` — source of truth để maintainer/contributor resume
- `docs/index.md` — router theo level / mục tiêu học

### Main curriculum
- `docs/levels/level-0-khoi-dong-va-mental-model.md`
- `docs/levels/level-1-core-operator.md`
- `docs/levels/level-2-context-skills-memory-profiles.md`
- `docs/levels/level-3-automation-integrations.md`
- `docs/levels/level-4-agent-teams-kanban.md`
- `docs/levels/level-5-advanced-builder-capstone.md`

### Labs
- `docs/labs/lab-00-day-0-setup.md`
- `docs/labs/lab-01-first-chat-sessions.md`
- `docs/labs/lab-02-models-tools-skills-memory.md`
- `docs/labs/lab-02b-context-files-profiles.md`
- `docs/labs/lab-03-automation-gateway.md`
- `docs/labs/lab-03b-routing-reliability.md`
- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
- `docs/labs/lab-05-builder-capstone.md`

### Reference
- `docs/reference/hermes-docs-source-map.md`
- `docs/templates/jarvis-SOUL.example.md` — mẫu `SOUL.md` cho profile `jarvis`

## Lưu ý về command trong repo

- Command nào đã được kiểm tra shape trực tiếp trên máy hiện tại sẽ được đánh dấu là **Verified on host** trong file reference.
- Command nào thuộc flow interactive hoặc flow phụ thuộc môi trường cá nhân sẽ được đánh dấu là **Sourced from Hermes docs**.
- Repo này không giả vờ rằng mọi command đều đã chạy thành công trong môi trường của learner; thay vào đó, repo phân biệt rõ **đã verify syntax** và **cần learner verify trên máy của mình**.
