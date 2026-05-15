# Lab 03 — Automation Surfaces: Cron, Hooks, Webhooks, Gateway

**Mục tiêu:** phân biệt đúng các primitive trigger/automation của Hermes và hiểu vì sao gateway là một service boundary chứ không chỉ là “bot chạy được”.  
**Thời lượng:** 40-60 phút.

## Lý thuyết cần nắm

Lab này dạy bạn chọn đúng primitive automation thay vì đổ mọi use case vào một chữ “bot”. Cron, hooks, webhooks, gateway, và batch processing là các lớp khác nhau; gateway còn là service boundary và security boundary chứ không chỉ là nơi chat được với Hermes.

### Sơ đồ mental model

```text
Use case picker

"8h mỗi ngày chạy" -----------> [Cron]
"Hermes vừa xong một lifecycle" -> [Hook]
"GitHub/CI bắn event vào" -----> [Webhook]
"Người chat với bot" ----------> [Gateway]
"Tạo nhiều trajectories" ------> [Batch]

Gateway còn thêm 2 lớp:
[service lifecycle] + [security boundary / pairing / allowlist]
```

### Nguồn đối chiếu chính

- Cron Jobs
- Hooks
- Messaging Gateway overview
- Webhooks
- Discord / Telegram platform pages (để hiểu thêm auth + thread/session behavior nếu cần)

### Verified on host trong lab này

Các command dưới đây đã được verify shape trên host hiện tại:

```bash
hermes cron list --help
hermes webhook --help
hermes webhook subscribe --help
hermes webhook test --help
hermes hooks --help
hermes hooks list --help
hermes hooks doctor --help
hermes gateway --help
hermes gateway setup --help
hermes gateway status --help
hermes pairing list --help
```

### Bước 1 — nhìn command surfaces trước khi nghĩ use case

Chạy:

```bash
hermes cron list --help
hermes webhook --help
hermes webhook subscribe --help
hermes webhook test --help
hermes hooks --help
hermes hooks list --help
hermes hooks doctor --help
hermes gateway --help
hermes gateway setup --help
hermes gateway status --help
hermes pairing list --help
```

### Tự trả lời ngay sau khi đọc output

1. Command nào nhìn như lịch chạy theo thời gian?
2. Command nào nhìn như event từ ngoài đi vào Hermes?
3. Command nào nhìn như kiểm tra/cấu hình lifecycle nội bộ?
4. Command nào rõ ràng là service management?
5. `pairing` thuộc capability hay security boundary?

### Bước 2 — mapping use case -> primitive đúng nhất

Ghép từng use case vào primitive phù hợp nhất:

1. “Mỗi sáng 8h gửi daily summary.”
2. “GitHub/CI gửi event vào Hermes khi build fail.”
3. “Mỗi lần subagent xong thì log audit record.”
4. “Team chat chung với Hermes trong Discord/Telegram.”
5. “Muốn bot chạy nền, có start/stop/status như service.”
6. “Muốn tạo rất nhiều agent trajectories từ dataset JSONL.”

**Expected mapping:**
1. cron
2. webhook
3. hook
4. gateway
5. gateway service lifecycle
6. batch processing

### Bước 3 — gateway readiness thinking

Đọc lại output của:

```bash
hermes gateway --help
hermes gateway setup --help
hermes gateway status --help
hermes pairing list --help
```

Sau đó trả lời:

- khi nào dùng `run`?
- khi nào nên nghĩ `install` + `start` + `status`?
- vì sao gateway docs nói nhiều về allowlist/pairing?
- tại sao đây là vấn đề **service lifecycle + authorization**, không chỉ là command chat?

### Gợi ý theo docs gốc

Gateway là một background process:
- kết nối nhiều platform,
- giữ per-chat session store,
- chạy cron scheduler,
- áp security boundary cho người dùng thật.

### Bước 4 — hook vs webhook

Đọc lại output của:

```bash
hermes hooks --help
hermes webhook --help
```

Rồi viết 2 câu theo format:

- **Hook là ...**
- **Webhook là ...**

### Expected insight

- hook = điểm chạm vào lifecycle nội bộ Hermes,
- webhook = cửa nhận event từ hệ thống ngoài.

Nếu muốn nhớ nhanh:
- hook hỏi: “Hermes đang/vừa làm gì?”
- webhook hỏi: “Bên ngoài vừa đẩy gì vào Hermes?”

### Bước 5 — draft một webhook subscription nhưng chưa cần chạy thật

Từ help của:

```bash
hermes webhook subscribe --help
```

Hãy tự viết một command mẫu cho use case này:

> Khi CI gửi event `build.failed`, Hermes gửi tóm tắt lỗi về Discord home channel.

Bạn **không cần chạy thật** nếu chưa có env/target phù hợp. Chỉ cần draft command hợp lý.

### Ví dụ template để điền

```bash
hermes webhook subscribe ci-failures \
  --events build.failed \
  --description "..." \
  --prompt "..." \
  --deliver discord
```

### Bước 6 — batch processing: nhận diện để khỏi lạm dụng

Dựa trên docs gốc của Batch Processing, tự trả lời:

- vì sao batch processing không phải primitive đầu tiên cho daily ops?
- nó nghiêng về training/eval hơn hay nghiêng về team chat hơn?
- dataset + checkpoint + worker processes gợi ý nó thuộc lớp bài toán nào?

**Expected answer:** batch processing thuộc lớp large-scale trajectory/eval/training data generation.

## Hiểu sai thường gặp

- Hook và webhook là hai tên khác nhau của cùng một cơ chế.
- Gateway chỉ là chat bot giao diện đẹp hơn CLI.
- Batch processing là primitive mặc định cho daily ops.
- Viết được draft webhook là coi như đã production-ready.

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

```text
Jarvis, hãy làm automation boundary coach cho tôi trong Lab 03.

Objective:
- giúp tôi chọn đúng primitive giữa cron, hooks, webhooks, gateway, pairing, và batch processing,
- dẫn tôi đi qua command surfaces cần xem,
- buộc tôi tự map use case -> primitive thay vì đoán theo cảm giác.

Context:
- Tôi đang học Lab 03 — Automation Surfaces: Cron, Hooks, Webhooks, Gateway.
- Tôi muốn hiểu boundary để sau này không lạm dụng gateway hay cron sai chỗ.

Guardrails:
- đừng cho đáp án ngay trước khi tôi tự map use case,
- phân biệt rõ cái gì là verified on host và cái gì phụ thuộc môi trường riêng,
- nếu tôi lẫn event-driven với time-driven thì phải sửa ngay,
- kết luận phải ở mức vận hành với Jarvis chứ không chỉ định nghĩa lý thuyết.

Deliverables:
- một bảng mapping: use case, primitive đúng, vì sao không chọn primitive khác,
- một draft webhook subscription command hợp lý cho một use case cụ thể,
- một checklist ngắn: khi nào cần gateway readiness review trước khi build tiếp.

Quality standard:
- phải phân biệt rõ verified on host với sourced from docs,
- mỗi use case phải có lý do loại trừ primitive sai chứ không chỉ chọn primitive đúng,
- phải sửa ngay nếu tôi lẫn event-driven, time-driven, hook, webhook, gateway,
- kết luận cuối phải ở mức ra quyết định vận hành với Jarvis, không chỉ là định nghĩa.
```

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

Bạn coi như hoàn thành lab khi:

- không còn nhầm cron với webhook,
- không còn nhầm hook với webhook,
- hiểu gateway là service + security boundary,
- tự draft được 1 webhook subscription command hợp lý,
- phân biệt được batch processing với daily automation bình thường.

### Dấu hiệu cần reject hoặc làm lại

- Jarvis chỉ định nghĩa cron/webhook/gateway nhưng không map vào use case cụ thể
- bảng mapping không giải thích vì sao primitive khác bị loại
- draft webhook subscription quá mơ hồ, không gắn vào event/use case thật
- output không tách rõ phần nào đã verify trên host và phần nào mới sourced from docs

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis việc map use case sang primitive đúng nhất trước khi bạn xây automation. Nếu muốn tiến xa hơn, yêu cầu Jarvis đóng gói kết quả của lab này thành một primitive-selection playbook để tái sử dụng và chia sẻ cho team/community.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```
