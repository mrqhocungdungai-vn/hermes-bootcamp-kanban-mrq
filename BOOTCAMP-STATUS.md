# HERMES BOOTCAMP KANBAN MRQ STATUS — Source of Truth for Maintainers

> File này dành cho **maintainer/contributor/agent** để resume công việc viết hoặc chỉnh repo. Learner bình thường nên bắt đầu ở `README.md` hoặc `docs/index.md`.

## 1. Canonical repo
- Repo path: `/home/mrq-rd/projects/hermes-bootcamp-kanban-mrq`
- Repo purpose: khóa học Hermes Agent bằng tiếng Việt, tổ chức theo level từ cơ bản đến nâng cao
- Repo is / is not:
  - Là curriculum repo, repo-backed
  - Không phải app repo sản phẩm chính

## 2. Mission / scope

Viết lại toàn bộ khóa học để bám docs gốc Hermes chuẩn hơn, đặc biệt ở trục **Kanban + agent team + automation**.  
Course phải giúp learner đi từ mental model nền tảng tới năng lực thiết kế một hệ thống **Jarvis-as-orchestrator** điều phối nhiều workflow bằng Hermes.

| Phase | Nội dung |
|---|---|
| Phase A | Foundation: install, quickstart, CLI, config, sessions |
| Phase B | Capability: tools, skills, memory, profiles, worktrees |
| Phase C | Automation: cron, gateway, webhooks, MCP, ACP, API server |
| Phase D | Agent Team: delegation, goals, Kanban board, dispatcher, handoff artifacts |
| Phase E | Builder: plugins, memory providers, local LLM, Python library, internals |

## 3. Fixed spec / non-negotiables
- Stack / constraints: curriculum markdown repo, tiếng Việt, Hermes-first
- Fixed scope: học Hermes + Kanban + agent teams, không drift sang app implementation-first
- Acceptance criteria:
  - Có entrypoint rõ ràng
  - Có level progression rõ ràng
  - Có lab copy-paste được ở mức hợp lý
  - Có source map quay về docs gốc
  - Có handoff/source-of-truth files
- Canonical source files:
  - `README.md`
  - `docs/index.md`
  - `docs/reference/hermes-docs-source-map.md`

## 4. Current progress
### Completed
- Đã ingest docs gốc từ `llms-full.txt` và `llms.txt`
- Đã viết lại toàn bộ skeleton + level docs + labs + source map
- Đã cập nhật handoff files (`README.md`, `AGENTS.md`, `BOOTCAMP-STATUS.md`)

### Current milestone
- Authored through: **Level 5 + 8 labs + source map**
- Current phase/sprint/module: **Curriculum rewrite complete (v1.2) + self-study review pass 01 + editorial pass 02 complete**
- Next file/day/module to use: `docs/index.md`, sau đó learner chọn Level 0/1 hoặc đi thẳng roadmap phù hợp
- Framing refinement mới nhất: **Jarvis là orchestrator trung tâm** để learner học route/decompose/dispatch rõ ràng hơn
- Đã thread lại framing Jarvis xuyên suốt từ Level 0 -> Level 5 và Lab 00 -> Lab 05
- Self-study progress: **Level 0-5 đã được review xong trong pass 01**
- Editorial progress: **router, learner-vs-maintainer boundary, và practice guidance đã được review/sync trong pass 02**

### Not started
- Chưa có answer key / sample learner logs
- Chưa bundle một benchmark app repo thật cho Todo App end-to-end; hiện course dùng **scenario minh họa** hoặc learner-provided practice repo
- Chưa có batch lab riêng cho plugin authoring chi tiết hoặc mini plugin sample end-to-end

### Review artifacts
- `docs/reference/self-study-review-log.md` — nhật ký tự học + các cải tiến repo sau mỗi level review

## 5. File map
### Entry points
- `README.md` — mục tiêu repo, cách đọc, level overview
- `BOOTCAMP-STATUS.md` — source of truth cho maintainer/contributor
- `docs/index.md` — router theo level/mục tiêu

### Main content
- `docs/levels/level-0-khoi-dong-va-mental-model.md`
- `docs/levels/level-1-core-operator.md`
- `docs/levels/level-2-context-skills-memory-profiles.md`
- `docs/levels/level-3-automation-integrations.md`
- `docs/levels/level-4-agent-teams-kanban.md`
- `docs/levels/level-5-advanced-builder-capstone.md`

### Labs / artifacts
- `docs/labs/lab-00-day-0-setup.md`
- `docs/labs/lab-01-first-chat-sessions.md`
- `docs/labs/lab-02-models-tools-skills-memory.md`
- `docs/labs/lab-02b-context-files-profiles.md`
- `docs/labs/lab-03-automation-gateway.md`
- `docs/labs/lab-03b-routing-reliability.md`
- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
- `docs/labs/lab-05-builder-capstone.md`
- `docs/templates/capstone-brief-template.md`
- `docs/reference/hermes-docs-source-map.md`

## 6. Verified operational facts
- Docs ingest time: 2026-05-13T16:42:09+07:00
- `llms-full.txt` SHA256: `0586901bdecc2e0a5b60853b15405b1813cd3e9b523f653cd7012dfd7c3144bb`
- `llms.txt` SHA256: `57ba8901c6fef4c8aa6c5dd8f68575b4c526c6eecf24fc1ce68ef831d70ff807`
- Installed Hermes seen on host: `Hermes Agent v0.13.0 (2026.5.7)`
- Repo vẫn là git repo trống về nội dung curriculum cũ; branch hiện tại: `main`

## 7. Known risks / notes
- Một số flow interactive/gateway phụ thuộc credentials và môi trường riêng của learner; trong docs đã ghi rõ khi nào chỉ mới verify command shape.
- Hermes docs thay đổi nhanh; nếu update repo sau này, refresh source map trước.

## 8. Resume protocol
Đọc theo thứ tự:
1. `BOOTCAMP-STATUS.md`
2. `README.md`
3. `docs/index.md`
4. `docs/reference/hermes-docs-source-map.md`
5. level file liên quan nhất với tác vụ hiện tại

Sau đó trả lời 3 câu:
- Repo này đang dạy learner ở level nào?
- File canonical tiếp theo để learner đọc là gì?
- Có command/lab nào cần re-verify vì docs Hermes đã thay đổi không?

## 9. Update rule
Khi thêm/chỉnh module:
- cập nhật `README.md` nếu router thay đổi,
- cập nhật `docs/index.md` nếu entry path thay đổi,
- cập nhật `BOOTCAMP-STATUS.md` với authored-through marker mới,
- nếu command docs thay đổi, cập nhật source map.

## 10. One-line resume summary
Curriculum rewrite đã hoàn tất theo docs gốc Hermes; bước tiếp theo là learner đi theo `docs/index.md` và bắt đầu thực hành lab theo level phù hợp.
