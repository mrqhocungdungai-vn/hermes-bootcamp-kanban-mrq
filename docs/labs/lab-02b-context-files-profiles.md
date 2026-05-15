# Lab 02B — Context Files, Skills Hygiene, Profiles, Worktrees

**Mục tiêu:** thiết kế đúng durable context architecture cho Jarvis: repo rules ở đâu, procedural knowledge ở đâu, durable facts ở đâu, profile nào nên tách, và khi nào phải dùng worktree.  
**Thời lượng:** 35-45 phút.  
**Đọc trước:** `docs/levels/level-2-context-skills-memory-profiles.md`

## Lý thuyết cần nắm

Lab này không thay Level 2. Nó chỉ buộc bạn áp mental model của Level 2 vào một repo thật hoặc practice repo của bạn.

### Sơ đồ mental model

```text
Repo rule -----------> AGENTS.md / context file
Reusable procedure --> Skill
Durable fact -------> Memory
Past transcript ----> Session search
Identity/state -----> Profile
Filesystem isolate -> Worktree
Handoff artifact ---> File trong repo
```

Boundary ngắn cần nhớ:
- Đừng nhét mọi thứ vào AGENTS.md.
- Skill không phải note folder tùy tiện.
- Profile là identity/state boundary, còn worktree là filesystem boundary.
- Handoff quan trọng phải sống trong file, không sống trong transcript.

## Hiểu sai thường gặp

- Cứ có thông tin là nhét vào memory.
- Profile có thể thay thế worktree.
- AGENTS.md là nơi chứa cả rule, handoff, workflow, note, checklist, và mọi thứ khác.
- Chỉ cần chat summary, không cần artifact trong repo.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 02B về context architecture.

Objective:
- Audit cách tôi đang route tri thức và context trong repo/project của mình.
- Đề xuất một context architecture bền vững cho Jarvis.

Context:
- Tôi đã đọc `docs/levels/level-2-context-skills-memory-profiles.md`.
- Tôi muốn dùng đúng boundary giữa AGENTS.md, skill, memory, profile, worktree, và handoff artifact.
- Repo/practice repo của tôi là: <ABSOLUTE_PROJECT_PATH>

Guardrails:
- Không giảng lại nguyên Level 2.
- Nếu thiếu dữ liệu, nêu giả định rõ ràng.
- Chỉ ra chỗ nào nên là AGENTS.md, chỗ nào nên là skill, chỗ nào nên là memory, chỗ nào phải thành file artifact.
- Nếu cần nhiều agent cùng sửa code, phải nói rõ khi nào worktree là bắt buộc.

Deliverables:
- Một context architecture ngắn cho repo của tôi.
- Một bảng route quyết định: AGENTS.md vs skill vs memory vs session search vs profile vs worktree.
- Một danh sách 3-5 artifact/handoff file nên tồn tại lâu dài trong repo.

Quality standard:
- Không được lẫn profile với worktree.
- Không được biến memory thành bãi chứa thông tin tạm thời.
- Mỗi loại thông tin phải có “nhà” rõ ràng.
- Handoff artifacts phải được ưu tiên hơn chat summary sống.

Cuối cùng, trả về:
1. Context architecture đề xuất
2. Bảng route quyết định
3. Danh sách artifact/handoff file nên có
4. Boundary nào tôi cần đọc lại ở Level 2
```

## Kết quả mong đợi

Output tốt sẽ có:
- một sơ đồ route rất rõ: loại thông tin nào sống ở đâu,
- giải thích ngắn khi nào cần tách profile và khi nào cần thêm worktree,
- danh sách artifact/handoff file cụ thể để repo bền hơn.

Dấu hiệu cần làm lại:
- vẫn lẫn AGENTS.md với skill,
- coi memory như nơi nhét mọi thứ,
- không nói gì về worktree khi có multi-agent coding,
- vẫn ưu tiên transcript hơn file artifact.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, khi bắt đầu một repo hoặc project mới, hãy giao cho Jarvis việc thiết kế context architecture trước khi bắt đầu workflow phức tạp.

Prompt ngắn để dùng lại:

```text
Jarvis, với repo này, hãy route giúp tôi:
- cái gì nên vào AGENTS.md,
- cái gì nên thành skill,
- cái gì nên vào memory,
- khi nào cần profile riêng,
- khi nào cần worktree,
- và những handoff artifact nào phải sống trong repo.
```
