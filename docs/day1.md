# Day 1 — Sprint 1A: Single-Agent Backend Drill

Coach: Drill Coach Hermes
Trainee: bạn
Trạng thái ngày: READY

## DAY 1 CONTRACT

═══════════ DAY 1 CONTRACT ═══════════
Mục tiêu hôm nay:    Build backend TaskHero bằng 1 agent và đo giới hạn single-agent loop
Deliverable bắt buộc:
- backend scaffold chạy được
- auth + task CRUD có test chạy thật
- file log `log/day1-observations.md` được điền
- >= 50% endpoint hoạt động
Budget token:        ~1.5M-2.5M
Budget thời gian:    ≤ 5 giờ
Anti-pattern cấm:
- yêu cầu agent build fullstack ngay trong một nhịp
- không log loop/debug/time/token
- sửa spec để agent dễ pass
══════════════════════════════════════

Xác nhận bằng `GO` để bắt đầu ngày.

## Mục tiêu học thật sự
Day 1 không phải để chứng minh 1 agent “cũng làm được”. Day 1 dùng để trainee thấy bằng evidence rằng:
- 1 agent có thể đi nhanh lúc đầu
- nhưng planning, coding, testing, debug, integration debt đều dồn vào một chỗ
- khi bug xuất hiện, token và wall time tăng rất nhanh

## Single-agent loop trong <= 200 từ
Hermes chạy theo vòng lặp: đọc context -> chọn bước nhỏ tiếp theo -> gọi tool -> đọc kết quả -> sửa kế hoạch -> lặp lại. Nếu test hoặc command fail, cùng agent phải tự debug rồi quay lại vòng lặp. Mô hình này mạnh ở chỗ tập trung và ít coordination overhead. Nó yếu ở chỗ mọi loại tải đều dồn vào một luồng: hiểu spec, viết code, chạy test, sửa lỗi, giữ context. Với backend TaskHero, bottleneck sẽ lộ rõ khi agent vừa phải tạo project scaffold, vừa phải giữ schema đúng, vừa phải tự điều tra lỗi test/runtime.

ASCII:

```text
[read SPEC]
    |
    v
[plan next slice]
    |
    v
[call tools]
    |
    v
[inspect result]
    |
    +--> fail? --> [debug] --+
    |                        |
    +--> pass --> [next slice]+
    |
    v
 [stop/report]
```

## Fixed build scope của Day 1
Hôm nay chỉ backend. Không frontend. Không CI polish. Không docs dài dòng. Không “làm luôn cho tiện”.

Backend phải bám stack cố định:
- FastAPI
- SQLAlchemy
- Postgres
- Redis
- JWT auth
- bcrypt
- pytest

Feature order khuyến nghị:
1. backend scaffold + env/config
2. auth: sign up / sign in / sign out
3. task CRUD
4. tag model cơ bản
5. filter/search cơ bản
6. activity log foundation
7. CSV export foundation
8. overdue notification skeleton
9. admin stats skeleton

## Lệnh trainee chạy

```bash
cd ~/projects/taskhero
hermes chat --toolsets "terminal,file,web" --model "anthropic/claude-sonnet-4.6"
```

## Prompt khởi động khuyến nghị

```text
Build TaskHero backend theo SPEC.md. Chỉ làm backend hôm nay. Code từng feature một, test sau mỗi feature, nếu fail thì tự debug rồi mới đi tiếp. Mỗi lần báo cáo phải ghi rõ: changed files, command đã chạy, pass/fail, bước nhỏ tiếp theo.
```

## Prompt kỷ luật mạnh hơn

```text
You are a disciplined single-agent builder for TaskHero Sprint 1A.
Read SPEC.md first. Build ONLY the backend today using FastAPI + SQLAlchemy + Postgres + Redis.
Work feature-by-feature, not all at once.
After each feature slice:
1. summarize what changed,
2. run relevant tests/checks,
3. report pass/fail honestly,
4. propose the next smallest step.

Hard constraints:
- do not build frontend today
- do not change stack
- do not reduce scope in SPEC.md
- do not claim done without runtime or test evidence
- if a test fails, debug before moving on
- keep commits/messages clean if you suggest them
```

## Steering prompts khi agent lệch hướng

Khi agent định build frontend:
```text
Stop. Hôm nay chỉ backend. Quay lại SPEC.md và tiếp tục đúng Day 1 scope.
```

Khi agent claim xong nhưng chưa có evidence:
```text
Không chấp nhận claim thiếu evidence. Chạy test hoặc command runtime thật, rồi báo lại pass/fail.
```

Khi agent đi quá to một lúc:
```text
Thu nhỏ batch. Chỉ làm 1 slice kế tiếp và verify xong rồi mới đi tiếp.
```

Khi agent loop debug vô ích:
```text
Stop looping. Tóm tắt root cause giả thuyết, chọn 1 giả thuyết mạnh nhất, kiểm chứng nó bằng command cụ thể rồi tiếp tục.
```

## Lab flow chuẩn
1. Mở session mới sạch
2. Dán prompt khởi động
3. Bắt agent scaffold backend
4. Sau mỗi slice, ép agent chạy test/check thật
5. Nếu fail, ghi log trước rồi mới fix
6. Trainee chỉ can thiệp khi agent drift, claim ẩu, hoặc debug loop quá lâu

## File log bắt buộc
Trong lúc chạy, trainee phải điền:
- `log/day1-observations.md`

Tối thiểu phải có:
- tổng số loop lớn
- số lần self-debug
- token consumed
- wall-clock time
- 3 failure đáng nhớ nhất
- trainee đã can thiệp lúc nào

## Stop condition
Dừng Day 1 khi 1 trong 2 điều xảy ra:
- backend pass 70% test target
- hoặc chạm 5 giờ wall-clock

## Acceptance criteria cuối Day 1
Coach chỉ cho PASS nếu đủ:
- backend boot được
- auth có tiến triển thực
- task CRUD có tiến triển thực
- >= 50% endpoint hoạt động
- có test chạy thật
- log observations đầy đủ

## Auto-fail conditions
FAIL ngay nếu gặp một trong các lỗi sau:
- không chạy test thật
- build cả frontend trong Day 1
- báo done không có evidence
- không ghi số liệu
- backend không boot được

## Mẫu báo cáo cuối ngày

```text
DAY 1 STATUS:
- runtime:
- tests:
- endpoint coverage:
- token used:
- wall time:
- biggest break:
- fix applied:
- coach verdict:
```

## Coach verdict template

```text
PASS: Day 1 vì backend boot được, auth/task CRUD có evidence, log số liệu đầy đủ.
```

hoặc

```text
FAIL: Day 1 vì <lý do cụ thể>. Quay lại checkpoint <x>.
```

## Điều trainee phải học sau Day 1
Nếu Day 1 làm đúng, cuối ngày trainee phải tự nói được 3 câu này:
1. 1 agent nhanh ở giai đoạn nào
2. 1 agent sụp ở giai đoạn nào
3. dấu hiệu nào cho thấy cần nhiều hơn 1 agent ở Sprint 2
