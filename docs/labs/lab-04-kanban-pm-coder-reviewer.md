# Lab 04 — Kanban Jarvis -> PM -> Coder -> Reviewer

**Mục tiêu:** dùng Jarvis như orchestrator để dựng workflow Kanban bền vững theo graph `Jarvis -> PM -> Coder -> Reviewer`, trong đó human giữ control plane chiến lược còn artifact review sống trong repo.  
**Thời lượng:** 45-75 phút.  
**Đọc trước:** `docs/levels/level-4-agent-teams-kanban.md`

## Lý thuyết cần nắm

Lab này không thay Level 4. Nó chỉ buộc bạn áp đúng boundary đã học: human đặt objective và guardrails, Jarvis thiết kế graph/handoff, worker làm runtime work, và reviewer để lại artifact bền vững.

### Sơ đồ mental model

```text
[Human]
  -> objective + guardrails + review

[Jarvis]
  -> orchestration + graph + handoff

[PM] -> spec artifact
[Coder] -> implementation + summary
[Reviewer] -> review artifact

Recovery loop:
show / context / runs / reassign
```

Boundary ngắn cần nhớ:
- Human không nên hand-author toàn bộ graph bằng tay nếu mục tiêu là học orchestration.
- Jarvis không nên ôm luôn implementation thay coder.
- Review findings phải sống trong file artifact, không chỉ nằm trong chat.
- Nếu board/profile/gateway chưa sẵn sàng, coi đó là preflight gap cần nêu rõ.

## Hiểu sai thường gặp

- Kanban chỉ là bản lớn hơn của `delegate_task`.
- Muốn chắc ăn thì operator nên tự tạo mọi task bằng CLI.
- Reviewer chỉ cần summary trong chat là đủ.
- Coder có thể bỏ qua artifact của PM hoặc reviewer.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 04 về Kanban orchestration.

Objective:
- Dựng workflow `Jarvis -> PM -> Coder -> Reviewer` theo kiểu prompt-first.
- Tôi muốn bạn giữ vai trò orchestrator, còn tôi giữ vai trò operator chiến lược.

Context:
- Tôi đã đọc `docs/levels/level-4-agent-teams-kanban.md`.
- Board Kanban active hoặc practice repo của tôi là: <ABSOLUTE_PROJECT_PATH>
- Nếu môi trường còn thiếu board, profile, gateway, hoặc workspace hợp lệ, hãy báo rõ preflight gaps trước.

Guardrails:
- Không đẩy tôi về manual workflow dài bằng CLI nếu có thể thiết kế orchestration bằng prompt.
- Phải dùng durable handoff và artifact trong repo.
- PM phải để lại spec artifact rõ scope + acceptance criteria.
- Coder phải đọc spec artifact trước khi làm.
- Reviewer phải để lại review artifact với verdict + findings + next actions.
- Nếu graph hoặc artifact path còn mơ hồ, hãy tự siết lại trước khi nói là sẵn sàng dispatch.

Deliverables:
- Graph task rõ ràng cho `Jarvis -> PM -> Coder -> Reviewer`.
- Artifact paths dự kiến cho spec, implementation handoff, và review findings.
- Preflight gaps hoặc blocker thật sự nếu có.
- Một runtime checklist ngắn cho operator: lúc nào nên quan sát, lúc nào nên can thiệp.

Quality standard:
- Human/control-plane và worker/runtime phải tách rõ.
- Không để reviewer chỉ nói trong chat.
- Handoff phải đủ bền để child task không phụ thuộc transcript sống.
- Nếu có blocker môi trường, phải nêu rõ thay vì giả vờ workflow đã chạy ổn.

Cuối cùng, trả về:
1. Graph workflow đã chốt
2. Artifact paths đã chốt
3. Preflight gaps hoặc blocker
4. Runtime checklist cho operator
5. Điểm nào tôi cần review lại bằng Level 4
```

## Kết quả mong đợi

Output tốt sẽ có:
- một graph rõ ràng với đúng 4 vai,
- artifact paths bền vững trong repo,
- Jarvis giữ orchestration thay vì ôm luôn coding,
- runtime checklist giúp operator biết khi nào chỉ quan sát và khi nào phải recovery.

Dấu hiệu cần reject:
- Jarvis đẩy bạn về tự tạo toàn bộ graph bằng tay,
- không có artifact path rõ cho PM hoặc reviewer,
- reviewer chỉ trả lời trong chat,
- không nêu preflight gaps dù môi trường còn thiếu rõ ràng.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, khi bạn muốn chạy một workflow nhiều vai, hãy giao cho Jarvis việc thiết kế graph, siết handoff, và nhắc bạn đúng lúc cần can thiệp ở control plane.

Prompt ngắn để dùng lại:

```text
Jarvis, hãy thiết kế cho tôi một workflow Kanban nhiều vai.
Giữ giúp tôi 4 thứ:
- graph rõ ràng,
- handoff durable,
- artifact review sống trong repo,
- và checklist lúc nào operator phải can thiệp.
```
