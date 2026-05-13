# Lab 03 — Automation Surfaces: Cron, Hooks, Webhooks, Gateway

**Mục tiêu:** phân biệt đúng các primitive trigger/automation của Hermes và hiểu vì sao gateway là một service boundary chứ không chỉ là “bot chạy được”.  
**Thời lượng:** 40-60 phút.

## Nguồn đối chiếu chính

- Cron Jobs
- Hooks
- Messaging Gateway overview
- Webhooks
- Discord / Telegram platform pages (để hiểu thêm auth + thread/session behavior nếu cần)

## Verified on host trong lab này

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

## Bước 1 — nhìn command surfaces trước khi nghĩ use case

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

## Bước 2 — mapping use case -> primitive đúng nhất

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

## Bước 3 — gateway readiness thinking

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

## Bước 4 — hook vs webhook

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

## Bước 5 — draft một webhook subscription nhưng chưa cần chạy thật

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

## Bước 6 — batch processing: nhận diện để khỏi lạm dụng

Dựa trên docs gốc của Batch Processing, tự trả lời:

- vì sao batch processing không phải primitive đầu tiên cho daily ops?
- nó nghiêng về training/eval hơn hay nghiêng về team chat hơn?
- dataset + checkpoint + worker processes gợi ý nó thuộc lớp bài toán nào?

**Expected answer:** batch processing thuộc lớp large-scale trajectory/eval/training data generation.

## Success criteria

Bạn coi như hoàn thành lab khi:

- không còn nhầm cron với webhook,
- không còn nhầm hook với webhook,
- hiểu gateway là service + security boundary,
- tự draft được 1 webhook subscription command hợp lý,
- phân biệt được batch processing với daily automation bình thường.
