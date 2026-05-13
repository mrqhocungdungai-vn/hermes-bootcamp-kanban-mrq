# AGENTS.md

Handoff file cho mọi agent/phiên làm việc tiếp theo trong repo `/home/mrq-rd/projects/hermes-bootcamp-kanban-mrq`.

## Repo này là gì

Đây là **curriculum repo** về Hermes Agent bằng tiếng Việt, trọng tâm là **Kanban + agent teams + automation + durable handoffs** theo docs gốc của Hermes.

## Repo này không phải là gì

- Không phải repo triển khai Todo App hoàn chỉnh.
- Không phải notes rời rạc kiểu brainstorm.
- Không phải tài liệu chung chung về AI agents tách rời Hermes.

## Non-negotiables

1. **Bám docs gốc Hermes trước**: `llms-full.txt` và `llms.txt` là canonical source cho course framing.
2. **Hermes-first, app-second**: benchmark Todo App là scenario minh họa để học workflow; nếu cần chạy thật, learner phải tự chuẩn bị practice repo.
3. **Kanban chuẩn hơn**: luôn nhấn mạnh khác biệt giữa `delegate_task`, `kanban`, `cron`, `background terminal`.
4. **Artifact bền vững**: mọi flow review nên khuyến khích lưu file review trong repo, không chỉ chat prose.
5. **Giữ router sống**: nếu thêm/chuyển file, cập nhật đồng thời `README.md`, `docs/index.md`, `BOOTCAMP-STATUS.md`.
6. **Một concept mỗi lesson**: tránh lesson quá rộng khiến learner lẫn giữa các layer.
7. **Ghi nguồn rõ**: lesson/level nên dẫn lại page docs liên quan.
8. **Không tự tiện đổi framing**: repo này phục vụ lộ trình từ cơ bản đến nâng cao cho learner Việt Nam muốn xây **Jarvis-as-orchestrator + multi-project Kanban model**.

## Framing sư phạm đã khóa

- Ngôn ngữ: tiếng Việt
- Style: mentor ngắn gọn, Socratic + Feynman + hands-on
- Hình thức: repo-backed, file-based, survive context compaction
- Team model ưa thích: **Jarvis** là orchestrator trung tâm, có thể điều phối nhiều project bằng project-specific skills/tools

## Canonical learning flow

`README.md` -> `docs/index.md` -> level docs -> lab docs -> learner thực hành trên máy thật.

## Benchmark scenario xuyên suốt

Dùng **Todo App** (Next.js + TypeScript + SQLite) làm context chung để minh họa:
- Jarvis orchestrator intake,
- spec,
- backlog,
- task decomposition,
- Jarvis -> PM -> Coder -> Reviewer,
- review artifact,
- worktree isolation,
- Kanban dispatch.

Hiện tại đây là **benchmark scenario minh họa** trong curriculum repo. Nếu learner muốn chạy end-to-end trên code thật, hãy dùng fork/local copy hoặc một practice repo riêng.

## Khi cập nhật repo này

- Ưu tiên chỉnh theo **level/module/lab** thay vì ném thêm note lẻ.
- Nếu thêm command mới, phân loại rõ `Verified on host` hay `Sourced from Hermes docs`.
- Nếu viết thêm capstone, vẫn phải giữ đường dẫn nhập môn cho learner mới.
