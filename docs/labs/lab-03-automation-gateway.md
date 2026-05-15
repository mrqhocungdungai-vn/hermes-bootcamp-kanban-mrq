# Lab 03 — Automation Surfaces: Cron, Hooks, Webhooks, Gateway

**Mục tiêu:** chọn đúng primitive automation của Hermes cho từng use case thay vì gom mọi thứ vào một chữ “bot”.  
**Thời lượng:** 35-45 phút.  
**Đọc trước:** `docs/levels/level-3-automation-integrations.md`

## Lý thuyết cần nắm

Lab này chỉ dùng lại khung chọn primitive của Level 3. Nếu bạn còn mơ hồ giữa cron, hooks, webhooks, gateway, và batch, hãy quay lại level trước khi làm lab.

### Sơ đồ mental model

```text
"Chạy theo lịch" ------------> Cron
"Hermes vừa xong lifecycle" -> Hook
"Hệ ngoài bắn event vào" ---> Webhook
"Con người chat với bot" ---> Gateway
"Tạo nhiều trajectories" ---> Batch
```

Boundary ngắn cần nhớ:
- Gateway là service boundary, không chỉ là nơi chat được.
- Webhook là external event ingress, không phải lịch chạy.
- Hooks bám vào lifecycle của Hermes, không phải mọi sự kiện ngoài đời.
- Batch là kiểu xử lý hàng loạt, không phải bot chat.

## Hiểu sai thường gặp

- Có message platform là tự động đồng nghĩa với gateway.
- Cron và webhook khác nhau chỉ ở cú pháp.
- Hook là tên khác của webhook.
- Batch processing chỉ là một cách gọi fancy của cron.

## Prompt lab cho Jarvis

```text
Jarvis, hãy giúp tôi làm Lab 03 về automation surfaces.

Objective:
- Giúp tôi chọn đúng primitive Hermes cho các use case automation phổ biến.
- Tạo một decision note ngắn để tôi dùng lại lần sau.

Context:
- Tôi đã đọc `docs/levels/level-3-automation-integrations.md`.
- Tôi muốn phân biệt đúng cron, hooks, webhooks, gateway, và batch.

Guardrails:
- Không giảng lại toàn bộ Level 3.
- Nếu cần command, chỉ nêu command tối thiểu để tôi tự kiểm môi trường hiện tại.
- Mỗi use case phải map về đúng primitive chính, và nếu có primitive phụ thì nói rõ vì sao.
- Nếu một use case đụng gateway, phải nói thêm service/security boundary ngắn gọn.

Deliverables:
- Một bảng quyết định cho 5 primitive: cron, hooks, webhooks, gateway, batch.
- Ít nhất 5 use case mẫu và primitive đúng cho mỗi use case.
- Một checklist ngắn để tôi dùng lại khi thiết kế automation mới.

Quality standard:
- Không được lẫn hook với webhook.
- Không được biến gateway thành đáp án mặc định cho mọi nhu cầu automation.
- Mỗi use case phải có lý do chọn primitive rõ ràng.
- Checklist cuối phải ngắn và thực dụng.

Cuối cùng, trả về:
1. Bảng quyết định
2. 5 use case mẫu
3. Checklist tái sử dụng
4. Boundary nào tôi cần đọc lại ở Level 3
```

## Kết quả mong đợi

Output tốt sẽ có:
- một decision table ngắn, dễ nhìn,
- use case đủ thực tế để phân biệt đúng từng primitive,
- checklist giúp bạn tránh chọn sai primitive ngay từ đầu.

Dấu hiệu cần reject:
- trả lời như lesson dài dòng,
- hook và webhook vẫn lẫn nhau,
- gateway bị dùng như đáp án mặc định,
- không nhắc gì đến service/security boundary khi nói về gateway.

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, trước khi bạn build bất kỳ automation nào, hãy giao cho Jarvis việc chọn primitive đúng trước.

Prompt ngắn để dùng lại:

```text
Jarvis, với use case automation này, hãy chọn giúp tôi primitive chính trong Hermes:
cron, hook, webhook, gateway, hay batch.

Trả lời ngắn:
- primitive chính,
- vì sao,
- primitive nào dễ bị nhầm,
- và một rủi ro boundary tôi cần nhớ.
```
