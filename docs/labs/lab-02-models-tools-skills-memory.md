# Lab 02 — Models, Toolsets, Skills, Memory

**Mục tiêu:** phân biệt đúng capability boundary giữa model/provider, toolsets, skills, memory, và approvals để giao việc cho Jarvis không bị lẫn lớp.  
**Thời lượng:** 30-40 phút.  
**Đọc trước:** `docs/levels/level-1-core-operator.md` và phần mở đầu của `docs/levels/level-2-context-skills-memory-profiles.md`

## Lý thuyết cần nắm

Lab này chỉ dùng lại mental model từ Level 1 và Level 2. Nếu bạn chưa chắc model/provider khác gì toolsets, hoặc skill khác gì memory, hãy quay lại level trước khi làm lab.

### Sơ đồ mental model

```text
[provider -> model]
  -> năng lực suy luận / chi phí / độ ổn định

[toolsets]
  -> agent CÓ THỂ làm gì

[skills]
  -> agent BIẾT CÁCH làm gì

[memory]
  -> fact bền vững cần nhớ lại

[approvals/checkpoints]
  -> safety + recovery boundary
```

Boundary ngắn cần nhớ:
- Model/provider là runtime engine, không phải tri thức.
- Toolsets là capability boundary, không phải playbook.
- Skills là procedural knowledge, không phải note vụn.
- Memory chỉ nên chứa fact bền vững, không chứa tiến độ ngắn hạn.

## Hiểu sai thường gặp

- Đổi model là giải quyết được thiếu tool.
- Skill và memory chỉ là hai tên gọi cho cùng một thứ.
- Toolset càng nhiều càng tốt.
- Approval/checkpoint chỉ là thứ “phiền”, không liên quan quality hoặc recovery.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 02 về model/toolset/skill/memory boundary.

Objective:
- Giúp tôi audit cách tôi đang hiểu Hermes operator stack.
- Biến phần tôi vừa học thành một capability map ngắn, dễ dùng lại.

Context:
- Tôi đã đọc Level 1 và phần đầu Level 2.
- Tôi muốn tránh dùng Hermes như một hộp đen.

Guardrails:
- Không giảng lại toàn bộ level.
- Nếu cần command, chỉ nêu command tối thiểu để tôi tự kiểm setup hiện tại.
- Phải tách bạch 5 lớp: provider/model, toolsets, skills, memory, approvals/checkpoints.
- Nếu có điểm nào cần xem lại level docs, chỉ rõ đúng boundary đó.

Deliverables:
- Một capability map ngắn với 5 phần: model/provider, toolsets, skills, memory, approvals.
- 3 ví dụ phân loại đúng: cái gì nên là skill, cái gì nên là memory, cái gì chỉ là config/runtime.
- Một checklist ngắn để tôi dùng lại khi chuẩn bị giao việc cho Jarvis.

Quality standard:
- Phân loại phải rõ ràng, không chồng lớp.
- Ít nhất phải có 1 ví dụ sai phổ biến và cách sửa.
- Checklist cuối phải đủ ngắn để dùng trước khi bắt đầu một task mới.

Cuối cùng, trả về:
1. Capability map
2. 3 ví dụ phân loại
3. Checklist tái sử dụng
4. Boundary nào tôi cần đọc lại ở Level 1 hoặc Level 2
```

## Kết quả mong đợi

Output tốt sẽ có:
- một capability map ngắn, nhìn vào là thấy rõ runtime vs capability vs knowledge vs memory vs safety,
- ví dụ phân loại đúng và dễ kiểm,
- checklist thực dụng trước khi giao việc cho Jarvis.

Dấu hiệu cần reject:
- Jarvis trả lời như một bài tổng hợp dài dòng,
- không tách rõ skill với memory,
- không nói gì về approvals/checkpoints,
- capability map quá mơ hồ để dùng lại.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, khi thấy một việc mới, hãy giao cho Jarvis việc phân loại xem vấn đề đó thuộc lớp nào trước khi lao vào làm.

Prompt ngắn để dùng lại:

```text
Jarvis, trước khi làm task này, hãy phân loại giúp tôi:
- phần nào là model/provider concern,
- phần nào là toolset concern,
- phần nào cần skill,
- phần nào là memory bền vững,
- phần nào cần approval/checkpoint.

Trả lời thật ngắn, theo bullet.
```
