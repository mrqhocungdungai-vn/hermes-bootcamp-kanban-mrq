# Lab 00 — Day-0 Setup và Health Check

**Mục tiêu:** xác nhận môi trường Hermes của bạn thật sự sẵn sàng cho bootcamp.  
**Thời lượng:** 15-25 phút.  
**Đầu ra:** checklist pass/fail rõ ràng.

## Verified on host

Các command shape dưới đây đã được kiểm tra khi viết repo này.

## Bước 1 — vào đúng repo

```bash
export BOOTCAMP_REPO="/home/mrq-rd/projects/hermes-bootcamp-kanban-mrq"
cd "$BOOTCAMP_REPO" || exit 1
pwd
```

**Success:** thấy đúng path repo bootcamp.

## Bước 2 — xác nhận Hermes sống và có đường recovery

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

## Bước 3 — xác nhận profile hiện tại để tránh lẫn với session

```bash
hermes profile show default
```

**Success:** nhìn rõ profile name, path, model, gateway status. Đây là lớp state của agent, không phải một hội thoại cụ thể.

## Bước 4 — nếu định học backend isolation / Docker backend

```bash
docker --version
docker compose version
```

**Success:** cả 2 command chạy được.

## Bước 5 — nếu định học Kanban sớm, verify board CLI shape

```bash
hermes kanban boards --help
hermes kanban create --help
hermes kanban dispatch --help
```

**Success:** hiểu được ít nhất `boards`, `create`, `dispatch` có tồn tại và syntax cơ bản ra sao.

## Bước 6 — verify cuộc chat thật trước khi thêm feature khác

```bash
hermes chat -q "Nói ngắn gọn Hermes session khác profile thế nào?" -Q
```

**Success:** Hermes trả lời bình thường, không lỗi provider/model.  
**Ý nghĩa:** đây là rule quan trọng nhất của Quickstart — nếu base chat chưa ổn, chưa nên bật gateway, kanban, cron, hay routing.

## Bước 7 — ghi log học tập của riêng bạn

Tự tạo một note ngoài repo hoặc trong notebook cá nhân với 5 dòng:

- Hermes version:
- Provider/model hiện dùng:
- Profile hiện tại:
- Docker status:
- Blocker lớn nhất hiện tại:

## Success criteria

Bạn pass Lab 00 khi:

- Hermes CLI chạy được
- biết đường recovery/provider qua `hermes model` hoặc `hermes setup`
- có ít nhất 1 cuộc chat thật chạy được
- nhìn rõ profile hiện tại khác với session
- biết mình có/không có Docker
- biết liệu mình đã sẵn sàng vào Kanban labs hay chưa

## Failure modes thường gặp

- Có Hermes nhưng chưa cấu hình provider/model đúng
- Có provider nhưng chat thật vẫn fail
- Muốn học Docker backend nhưng máy chưa có Docker/Compose
- Tưởng gateway/kanban sẽ tự chạy dù chưa đọc syntax/help

## Feynman check

Trả lời thành lời: “Day-0 của Hermes khác gì với chỉ cài xong một CLI thông thường?”
