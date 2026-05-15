# Lab 03B — Reliability, Routing, MCP, ACP, API Server

**Mục tiêu:** tách đúng các lớp reliability và integration của Hermes để không lẫn provider routing, fallback, auxiliary models, MCP, ACP, và API Server.  
**Thời lượng:** 35-45 phút.  
**Đọc trước:** `docs/levels/level-3-automation-integrations.md`

## Lý thuyết cần nắm

Lab này chỉ áp dụng khung reliability/integration của Level 3. Nó không thay Level 3, và không phải nơi để học lại toàn bộ config surface.

### Sơ đồ mental model

```text
[Main provider/model]
  -> chạy task chính

Nếu lỗi cùng provider ------> credential pool / retry strategy
Nếu outage provider -------> fallback provider
Nếu task phụ trợ riêng ----> auxiliary model routing

Bên cạnh đó:
MCP = external tools
ACP = editor-native agent bridge
API Server = OpenAI-compatible serving surface
```

Boundary ngắn cần nhớ:
- Reliability routing và integration surface là hai lớp khác nhau.
- MCP không phải editor bridge; ACP không phải tool protocol chung; API Server không phải fallback provider.
- Auxiliary models là routing cho task phụ trợ, không phải mặc định thay model chính.

## Hiểu sai thường gặp

- Có fallback provider là đã giải quyết mọi reliability issue.
- MCP, ACP, và API Server chỉ là ba tên khác nhau của cùng một lớp integration.
- Auxiliary routing là cách đổi model chính cho nhanh.
- Provider routing và credential pool là một thứ.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 03B về reliability và integration boundary.

Objective:
- Giúp tôi audit cách tôi đang hiểu reliability stack của Hermes.
- Tạo một routing plan ngắn để tôi dùng lại khi cấu hình hoặc review hệ thống.

Context:
- Tôi đã đọc `docs/levels/level-3-automation-integrations.md`.
- Tôi muốn tách đúng 6 lớp: provider routing, fallback, auxiliary, MCP, ACP, API Server.

Guardrails:
- Không giảng lại toàn bộ level.
- Nếu cần command/config example, chỉ nêu mức tối thiểu để tôi tự kiểm.
- Phải nói rõ lớp nào là reliability strategy và lớp nào là integration surface.
- Nếu một lớp dễ bị nhầm với lớp khác, phải chỉ ra anti-pattern đó.

Deliverables:
- Một routing/reliability plan ngắn.
- Một bảng phân biệt 6 lớp: provider routing, fallback, auxiliary, MCP, ACP, API Server.
- 3 tình huống lỗi mẫu và lớp giải pháp đúng cho mỗi tình huống.

Quality standard:
- Không được trộn reliability với integration.
- Phải có ít nhất 1 anti-pattern phổ biến và cách sửa.
- Plan cuối phải đủ ngắn để dùng lại trong review hoặc design session.

Cuối cùng, trả về:
1. Routing/reliability plan
2. Bảng phân biệt 6 lớp
3. 3 tình huống lỗi mẫu
4. Boundary nào tôi cần đọc lại ở Level 3
```

## Kết quả mong đợi

Output tốt sẽ có:
- bảng phân biệt ngắn nhưng sắc,
- 3 tình huống lỗi giúp thấy rõ khi nào dùng fallback, auxiliary, hay integration surface khác,
- một plan đủ ngắn để dùng lại khi audit config.

Dấu hiệu cần làm lại:
- vẫn lẫn MCP với ACP hoặc API Server,
- nói về fallback như câu trả lời mặc định cho mọi sự cố,
- không tách rõ reliability strategy với integration surface,
- output quá dài nhưng vẫn không rõ anti-pattern.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, khi bạn review config hoặc thiết kế hệ thống mới, hãy giao cho Jarvis việc tách reliability strategy khỏi integration surface trước.

Prompt ngắn để dùng lại:

```text
Jarvis, với hệ thống Hermes này, hãy tách giúp tôi:
- lớp reliability strategy,
- lớp integration surface,
- anti-pattern dễ nhầm nhất,
- và routing plan ngắn nên dùng.
```
