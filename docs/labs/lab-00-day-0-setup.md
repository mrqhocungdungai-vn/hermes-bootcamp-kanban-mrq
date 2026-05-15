# Lab 00 — Day-0 Setup và Health Check

**Mục tiêu:** xác nhận môi trường Hermes của bạn thật sự sẵn sàng cho bootcamp.  
**Thời lượng:** 15-25 phút.  
**Đầu ra:** checklist pass/fail rõ ràng.

## Lý thuyết cần nắm

Lab này dùng để khóa mental model Day-0: “đã cài Hermes” chưa bao giờ đồng nghĩa với “đã sẵn sàng học và vận hành”. Bạn phải xác minh đúng repo, đúng profile, đúng provider/model, và một cuộc chat thật trước khi đi tiếp sang level sau hoặc bật thêm gateway/Kanban/Docker.

### Sơ đồ mental model

```text
Day-0 readiness flow

[Đúng repo path]
      -> [Hermes CLI sống]
      -> [provider/model usable]
      -> [chat thật chạy được]
      -> [profile hiện tại rõ ràng]
      -> [Docker / Kanban prerequisites nếu cần]
      -> [Go / No-Go cho level tiếp theo]

Nếu fail ở tầng nào:
- sửa tầng đó trước
- chưa layer thêm gateway / Kanban / automation
```

### Verified on host

Các command shape dưới đây đã được kiểm tra khi viết repo này.

### Bước 1 — vào đúng repo

**Quan trọng:** đừng copy nguyên path `/home/mrq-rd/...` từ máy tác giả. Nếu bạn chạy trên máy của mình, hãy tự chỉ rõ **absolute path thật** tới fork/local copy hoặc practice repo mà bạn sẽ dùng cho bootcamp.

```bash
export BOOTCAMP_REPO="/absolute/path/to/your/hermes-bootcamp-kanban-mrq"
cd "$BOOTCAMP_REPO" || exit 1
pwd
```

**Success:** thấy đúng absolute path repo bootcamp của chính bạn, không phải path hardcode từ máy người khác.

### Bước 2 — xác nhận Hermes sống và có đường recovery

```bash
hermes --version
hermes doctor
hermes status --deep
hermes model
```

**Quan sát:**
- `hermes --version` phải in version cụ thể
- `hermes doctor` không có blocking issue cho workflow bạn định học
- `hermes status --deep` giúp nhìn nhanh các component liên quan
- `hermes model` là entrypoint chuẩn theo Quickstart để kiểm tra hoặc sửa provider/model nếu chat chưa chạy ổn

> Ghi chú: `hermes auth list` vẫn hữu ích để inspect pooled credentials, nhưng **không phải** lúc nào cũng là bằng chứng duy nhất rằng provider đã usable. Quickstart của Hermes đặt `hermes model` / `hermes setup` làm đường chính để xác nhận provider.

### Bước 3 — xác nhận profile hiện tại để tránh lẫn với session

```bash
hermes profile show default
```

**Success:** nhìn rõ profile name, path, model, gateway status. Đây là lớp state của agent, không phải một hội thoại cụ thể.

### Bước 4 — nếu định học backend isolation / Docker backend

```bash
docker --version
docker compose version
```

**Success:** cả 2 command chạy được.

### Bước 5 — nếu định học Kanban sớm, verify board CLI shape

```bash
hermes kanban boards --help
hermes kanban create --help
hermes kanban dispatch --help
```

**Success:** hiểu được ít nhất `boards`, `create`, `dispatch` có tồn tại và syntax cơ bản ra sao.

### Bước 6 — verify cuộc chat thật trước khi thêm feature khác

```bash
hermes chat -q "Nói ngắn gọn Hermes session khác profile thế nào?" -Q
```

**Success:** Hermes trả lời bình thường, không lỗi provider/model.  
**Ý nghĩa:** đây là rule quan trọng nhất của Quickstart — nếu base chat chưa ổn, chưa nên bật gateway, kanban, cron, hay routing.

### Bước 7 — ghi log học tập của riêng bạn

Tự tạo một note ngoài repo hoặc trong notebook cá nhân với 5 dòng:

- Hermes version:
- Provider/model hiện dùng:
- Profile hiện tại:
- Docker status:
- Blocker lớn nhất hiện tại:

## Hiểu sai thường gặp

- Cài xong CLI là coi như đã pass Day-0.
- Profile hiện tại và session hiện tại là cùng một thứ.
- Doctor không báo đỏ nghĩa là chat thật chắc chắn chạy được.
- Có thể copy nguyên absolute path từ máy tác giả sang máy mình.

### Failure modes thường gặp

- Có Hermes nhưng chưa cấu hình provider/model đúng
- Có provider nhưng chat thật vẫn fail
- Muốn học Docker backend nhưng máy chưa có Docker/Compose
- Tưởng gateway/kanban sẽ tự chạy dù chưa đọc syntax/help

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

Mở Hermes Agent thật rồi paste prompt này. Lab này không nên bị học như một checklist shell command tách rời agent loop.

```text
Jarvis, hãy đóng vai mentor Day-0 cho bootcamp Hermes của tôi.

Objective:
- kiểm tra môi trường Hermes của tôi đã thật sự sẵn sàng chưa,
- dẫn tôi đi qua các health checks cần thiết,
- kết thúc bằng verdict pass/fail rõ ràng cho Day-0.

Context:
- Repo bootcamp tôi sẽ dùng nằm ở: <ABSOLUTE_BOOTCAMP_REPO_PATH>
- Tôi đang học Lab 00 — Day-0 Setup và Health Check.

Guardrails:
- đừng giả định path của máy tác giả là path của máy tôi,
- chỉ yêu cầu tôi chạy những check an toàn như version/status/doctor/model,
- nếu thấy môi trường chưa sẵn sàng thì chỉ rõ blocker lớn nhất,
- nếu thiếu thông tin, hãy hỏi ngắn gọn thay vì đoán.

Deliverables:
- một checklist health-check ngắn gọn theo đúng thứ tự chạy,
- một bảng pass/fail gồm: Hermes CLI, provider/model, chat thật, profile hiện tại, Docker (nếu cần), readiness cho level tiếp theo,
- một handoff note ngắn: blocker lớn nhất hiện tại là gì và bước tiếp theo nên là gì.

Quality standard:
- chỉ dùng path placeholder hoặc yêu cầu tôi thay bằng path thật trên máy tôi,
- chỉ yêu cầu các check an toàn như version/status/doctor/model,
- verdict cuối phải rõ ràng: pass / fail / pass có điều kiện,
- phải tách rõ hạng mục bắt buộc với hạng mục tùy chọn như Docker nếu lab sau không cần.
```

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

Bạn pass Lab 00 khi:

- Hermes CLI chạy được
- biết đường recovery/provider qua `hermes model` hoặc `hermes setup`
- có ít nhất 1 cuộc chat thật chạy được
- nhìn rõ profile hiện tại khác với session
- biết mình có/không có Docker
- biết liệu mình đã sẵn sàng vào Kanban labs hay chưa

### Dấu hiệu cần reject hoặc làm lại

- Jarvis chỉ nói chung chung kiểu “môi trường có vẻ ổn” nhưng không có bảng pass/fail rõ ràng
- Jarvis hardcode path máy tác giả thay vì yêu cầu bạn thay bằng path thật
- output không chỉ ra blocker lớn nhất hoặc bước tiếp theo
- Jarvis làm bạn lẫn giữa `profile` và `session` ngay từ Day-0

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis phần preflight Day-0: kiểm readiness, nhắc blocker lớn nhất, và trả về bảng pass/fail ngắn gọn trước khi bạn đầu tư thời gian vào lab khác. Sau khi pass, hãy yêu cầu Jarvis đóng gói kết quả thành một checklist Day-0 tái sử dụng cho chính bạn hoặc cho người học khác.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```

### Feynman check

Trả lời thành lời: “Day-0 của Hermes khác gì với chỉ cài xong một CLI thông thường?”
