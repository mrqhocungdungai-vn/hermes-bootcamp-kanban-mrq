# Hermes Docs Source Map

File này ghi lại **nguồn**, **mapping từ docs gốc -> course levels**, và **command nào đã verify syntax trên host**.

## 1. Canonical sources đã ingest

| Nguồn | URL | Bytes | SHA256 | Ghi chú |
|---|---|---:|---|---|
| Full docs bundle | `https://hermes-agent.nousresearch.com/docs/llms-full.txt` | 2332543 | `0586901bdecc2e0a5b60853b15405b1813cd3e9b523f653cd7012dfd7c3144bb` | Dùng để ingest one-shot toàn bộ docs |
| Docs index | `https://hermes-agent.nousresearch.com/docs/llms.txt` | 17422 | `57ba8901c6fef4c8aa6c5dd8f68575b4c526c6eecf24fc1ce68ef831d70ff807` | Dùng để map page -> lesson/level |

**Ingest time:** 2026-05-13 16:42:10 +07

## 2. Course level mapping

### Level 0 — Khởi động & mental model
Từ các docs:
- Getting Started / Installation
- Quickstart
- Learning Path
- Sessions
- Profiles (khái niệm mức nhập môn)

### Level 1 — Core operator
Từ các docs:
- CLI
- TUI
- Configuration
- Configuring Models
- Sessions
- Security
- Checkpoints & Rollback
- Tools / Toolsets / Slash Commands / CLI Commands

### Level 2 — Context, skills, memory, profiles
Từ các docs:
- Skills System
- Curator
- Memory
- Memory Providers
- Context Files
- Context References
- Personality & SOUL.md
- Profiles
- Git Worktrees

### Level 3 — Automation & integrations
Từ các docs:
- Cron Jobs
- Hooks
- Batch Processing
- Messaging Overview + platform docs
- Voice Mode / Browser / Vision / TTS
- MCP / ACP / API Server
- Provider Routing / Fallback Providers / Credential Pools
- Configuration (đặc biệt `auxiliary.*`, browser config, gateway-adjacent config)

### Level 4 — Agent teams & Kanban
Từ các docs:
- Delegation
- Kanban Multi-Agent
- Kanban Tutorial
- Persistent Goals
- Profiles
- Git Worktrees
- Context Files (để handoff chuẩn)

**Course framing note:** Level 4 của repo này dùng **Jarvis** như một orchestrator identity nhất quán để giúp learner nhìn rõ ranh giới giữa orchestration và execution.

### Level 5 — Advanced builder & capstone
Từ các docs:
- Plugins / Built-in Plugins
- Honcho Memory / Memory Providers
- Python Library guide
- Local LLM on Mac
- Developer Guide: Architecture, Agent Loop, Prompt Assembly, Context Compression, Adding Tools/Providers/Platforms, Creating Skills, Extending the CLI

## 3. Verified on host (đã kiểm tra syntax/help/version)

Các command dưới đây đã được kiểm tra trực tiếp trên host khi viết repo này:

```bash
hermes --version
hermes chat --help
hermes setup --help
hermes config --help
hermes config path
hermes config env-path
hermes config check
hermes doctor --help
hermes doctor
hermes status --help
hermes status --deep
hermes tools --help
hermes tools list --help
hermes tools list --platform cli
hermes skills --help
hermes skills list --help
hermes skills list --source all
hermes sessions list --help
hermes sessions list --limit 5
hermes auth list --help
hermes memory --help
hermes memory status
hermes profile --help
hermes profile list
hermes profile list --help
hermes profile show default
hermes profile create --help
hermes profile alias --help
hermes profile use --help
hermes curator status
hermes checkpoints --help
hermes checkpoints status
hermes mcp --help
hermes mcp list --help
hermes mcp add --help
hermes cron list --help
hermes hooks --help
hermes hooks list --help
hermes hooks doctor --help
hermes webhook --help
hermes webhook list --help
hermes webhook subscribe --help
hermes webhook test --help
hermes gateway --help
hermes gateway setup --help
hermes gateway status --help
hermes dashboard --help
hermes acp --help
hermes fallback --help
hermes fallback list --help
hermes pairing list --help
hermes auth add --help
hermes plugins --help
hermes plugins list
hermes plugins install --help
hermes plugins enable --help
hermes plugins disable --help
hermes memory setup --help
hermes memory off --help
hermes kanban boards --help
hermes kanban boards create --help
hermes kanban create --help
hermes kanban list --help
hermes kanban show --help
hermes kanban tail --help
hermes kanban complete --help
hermes kanban link --help
hermes kanban claim --help
hermes kanban reassign --help
hermes kanban context --help
hermes kanban runs --help
hermes kanban dispatch --help
```

Host also reported:

```text
Hermes Agent v0.13.0 (2026.5.7)
```

## 4. Sourced from Hermes docs (chưa chạy full flow end-to-end trong repo này)

Những flow sau được dùng trong curriculum nhưng **chỉ coi là sourced / learner must verify on their own environment**:

- setup gateway cho Telegram/Discord/Slack/WhatsApp/Signal/Email/SMS/Matrix/Mattermost
- voice mode đầy đủ (CLI + gateway voice channels)
- MCP add/test tới server cụ thể
- ACP/editor integration thực tế với VS Code/Zed/JetBrains
- API Server exposure cho frontend ngoài
- shell hooks declared in `config.yaml` và allowlist flow end-to-end
- plugin authoring + publish end-to-end
- local LLM benchmark trên Mac theo hardware thực tế của learner

## 5. Những docs đặc biệt quan trọng cho trục “Kanban + agent team chuẩn hơn”

Nếu phải chọn 10 page quan trọng nhất cho lộ trình này, ưu tiên:

1. Quickstart
2. Configuration
3. Sessions
4. Tools
5. Skills System
6. Memory
7. Profiles
8. Git Worktrees
9. Delegation
10. Kanban Multi-Agent / Kanban Tutorial

## 6. Rule cập nhật source map

Nếu Hermes docs đổi lớn:
1. re-download `llms-full.txt` và `llms.txt`
2. cập nhật hash + ingest time ở file này
3. review lại Level 3-5 trước, vì các phần automation/integrations/builder đổi nhanh nhất
4. nếu command syntax đổi, sửa lab tương ứng
