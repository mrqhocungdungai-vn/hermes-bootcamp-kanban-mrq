# Lab 04 — Kanban Jarvis -> PM -> Coder -> Reviewer

**Mục tiêu:** dựng một board cơ bản và nhìn thấy rõ workflow **Jarvis -> PM -> Coder -> Reviewer** theo cách **prompt-first**: bạn giao objective + guardrails cho Jarvis, còn Jarvis tự tạo graph, tự siết handoff, và để dispatcher/worker thực thi.  
**Thời lượng:** 45-90 phút.  
**Quan trọng:** command syntax bên dưới dùng cho **bootstrap / quan sát / recovery**. Trọng tâm lab này **không** phải là bạn tự tay copy-paste cả chuỗi `hermes kanban create ...` để đóng vai PM thay Jarvis.  
**Khuyến nghị thực hành:** làm lab này trên **fork/local copy** của repo course, hoặc map cùng workflow sang một practice repo riêng nếu bạn muốn gắn với code thật.

## Lý thuyết cần nắm

Lab này dạy Kanban như một execution system bền vững: human giữ control plane chiến lược, Jarvis giữ orchestration, worker profiles chạy runtime work, và review artifact tạo recovery loop. Nếu lẫn các surface này, multi-agent workflow sẽ rất nhanh sụp.

### Sơ đồ mental model

```text
Control plane vs runtime

[Human]
  -> đặt objective + guardrails
  -> quan sát + can thiệp khi cần

[Jarvis]
  -> dựng graph
  -> siết task body / handoff

[Board / Dispatcher]
  -> chuyển task ready thành worker execution

[PM] -> spec artifact
[Coder] -> code + summary
[Reviewer] -> review artifact

Recovery:
list/show -> context -> runs -> reassign/reclaim
```

### Framing quan trọng trước khi bắt đầu

Nếu bạn tự chạy toàn bộ lệnh tạo task, dependency, và handoff bằng tay, bạn đang luyện **operator thủ công**.

Nếu bạn dùng prompt để Jarvis:
- hiểu objective,
- tạo orchestration contract,
- dựng graph Jarvis -> PM -> Coder -> Reviewer,
- tự phát hiện chỗ mơ hồ rồi siết lại task body,

thì bạn đang luyện **Jarvis trưởng thành hơn như một orchestrator**.

**Lab này chọn hướng thứ hai.**

### Bước 0 — Preconditions

Bạn nên đã qua:
- Level 1
- Level 2
- hiểu profile/workspace cơ bản

Bạn cũng cần nhớ 3 rule Level 4:
1. `dir:<path>` phải là **absolute path**.
2. `worktree` hợp với coding task hơn shared repo trực tiếp.
3. Dispatcher chạy **trong gateway mặc định**, nên task `ready` sẽ không tự chạy nếu gateway chưa lên.

**Khuyến nghị thực hành:** làm lab này trên **fork/local copy** của repo course, hoặc thay `BOOTCAMP_REPO` bằng practice repo riêng nếu bạn muốn benchmark chạy trên code thật.

### Bước 1 — xem control-plane surfaces để biết bạn chỉ can thiệp ở đâu

```bash
hermes profile list
hermes kanban --help
hermes kanban boards --help
hermes kanban list --help
hermes kanban show --help
hermes kanban context --help
hermes kanban runs --help
hermes kanban reassign --help
hermes kanban dispatch --help
hermes gateway status
```

**Điều cần hiểu ngay ở đây:**
- các lệnh trên là **human/operator control plane**,
- worker thật do dispatcher spawn sẽ dùng `kanban_*` tools,
- ở lab này bạn dùng CLI chủ yếu để **preflight, quan sát, recovery**,
- phần **thiết kế graph** nên được giao lại cho Jarvis qua prompt.

### Bước 2 — bootstrap tối thiểu cho môi trường lab

Phần này vẫn là việc của operator, vì bạn cần một board đang active và các profile thật sự dùng được.

**Quan trọng:** đừng copy nguyên path `/home/mrq-rd/...` từ ví dụ của tác giả. Trên máy của bạn, hãy tự chỉ rõ **absolute path thật** tới nơi project sẽ chạy — có thể là fork/local copy của repo course, hoặc practice repo riêng.

```bash
export BOOTCAMP_REPO="/absolute/path/to/your/fork-or-practice-repo"
export BOARD_SLUG="todoapp-bootcamp"

cd "$BOOTCAMP_REPO" || exit 1

hermes kanban init
hermes kanban boards create "$BOARD_SLUG" \
  --name "TodoApp Bootcamp" \
  --description "Prompt-first Jarvis -> PM -> Coder -> Reviewer lab" \
  --switch
hermes kanban boards show
```

**Success:** active board là `todoapp-bootcamp`.

### Bước 3 — chuẩn bị profiles đúng cách

Xem profile hiện có:

```bash
hermes profile list
```

Nếu bạn chưa có đúng 4 profile `jarvis`, `pm`, `coder`, `reviewer`, **ưu tiên clone config từ profile đang dùng** để worker có sẵn `config.yaml`, `.env`, và `SOUL.md` cơ bản:

```bash
hermes profile create jarvis --clone
hermes profile create pm --clone
hermes profile create coder --clone
hermes profile create reviewer --clone
```

**Vì sao không khuyên tạo blank profile ở lab này?**  
Vì blank profile có thể chưa có provider/model/API key, khiến dispatcher spawn worker xong nhưng worker không làm được việc. Nếu bạn vẫn muốn profile rỗng, phải tự chạy thêm `jarvis setup`, `pm setup`, `coder setup`, `reviewer setup`.

### Bước 4 — đưa cho Jarvis một prompt seed thay vì tự tạo graph bằng tay

Mở session Jarvis của bạn và paste prompt này. Mục tiêu ở đây là **Jarvis tự tạo board graph** bằng tool/runtime phù hợp trong session của nó, chứ không phải bạn tạo 4 task thủ công bằng CLI.

**Trước khi paste:** thay toàn bộ `<ABSOLUTE_PROJECT_PATH>` bằng absolute path thật trên máy bạn. Nếu bạn để nguyên path mẫu của người khác, workspace `dir:...` sẽ sai và worker có thể fail ngay từ đầu.

```text
Bạn đang đóng vai Jarvis orchestrator cho board Kanban đang active tên `todoapp-bootcamp`.

Objective:
- Dựng workflow Jarvis -> PM -> Coder -> Reviewer cho benchmark Todo App v1.
- Tôi muốn học cách Jarvis tự tạo và tự siết workflow, không muốn tự tay chạy chuỗi lệnh tạo task bằng CLI.

Repo/workspace:
- Repo path: <ABSOLUTE_PROJECT_PATH>
- Với Jarvis / PM / Reviewer: dùng workspace `dir:<ABSOLUTE_PROJECT_PATH>`
- Với Coder: dùng workspace `worktree`

Profiles/assignees phải dùng:
- jarvis
- pm
- coder
- reviewer

Yêu cầu orchestration:
1. Tạo task orchestrator cho Jarvis trước.
2. Tạo các child task theo graph: Jarvis -> PM -> Coder -> Reviewer.
3. PM phải được giao tạo spec artifact markdown trong repo, nêu rõ scope, acceptance criteria, và handoff cho coder.
4. Coder phải được giao đọc spec artifact của PM, implement tối thiểu theo spec, và ghi handoff summary rõ changed files + tests.
5. Reviewer phải được giao tạo review artifact markdown trong repo với verdict, findings theo mức độ, và next actions rõ ràng cho coder.
6. Handoff phải durable, không phụ thuộc transcript sống.
7. Nếu thấy objective hoặc artifact path còn mơ hồ, hãy tự siết lại task body trước khi kết thúc.

Output tôi cần ngay trong chat:
- danh sách task đã tạo,
- task ID của từng node,
- dependency graph,
- artifact paths dự kiến,
- chỗ nào còn blocker hoặc prerequisite thiếu.
```

**Boundary note:** nếu session Jarvis của bạn không có quyền/tool để thao tác Kanban, hãy coi đó là **gap cần sửa ở môi trường hoặc tool access**, không phải tín hiệu để quay lại cách làm thủ công ngay lập tức.

### Bước 5 — yêu cầu Jarvis tự review graph vừa tạo

Sau khi Jarvis báo đã dựng xong graph, gửi tiếp một prompt ngắn để ép Jarvis trưởng thành hơn ở khâu handoff design:

```text
Hãy tự review lại graph vừa tạo như một PM vận hành nghiêm túc:
- task body nào còn mơ hồ thì sửa lại,
- artifact path nào chưa đủ rõ thì chốt rõ,
- child nào đang phụ thuộc parent nhưng chưa được chỉ đúng file/đầu ra cần đọc thì bổ sung,
- đảm bảo reviewer luôn để lại review artifact trong repo chứ không chỉ summary trong chat.

Sau khi siết xong, báo lại:
- task nào đã được cập nhật,
- vì sao phải cập nhật,
- graph hiện tại đã sẵn sàng dispatch chưa.
```

**Ý chính:** ở bước này bạn đang huấn luyện Jarvis biết **tự phát hiện và sửa design debt trước khi runtime bắt đầu**, thay vì chờ fail rồi người vận hành mới cứu.

### Bước 6 — quan sát queue từ control plane, không giành việc với Jarvis

Sau khi Jarvis đưa cho bạn các task ID, dùng CLI để **quan sát**:

```bash
hermes kanban list
hermes kanban stats
hermes kanban show <JARVIS_ID>
hermes kanban show <PM_ID>
hermes kanban show <CODER_ID>
hermes kanban show <REVIEW_ID>
hermes kanban context <CODER_ID>
hermes kanban dispatch --dry-run
```

**Điều cần hiểu:**
- `PM_ID` chưa ready nếu `JARVIS_ID` chưa done
- `CODER_ID` chưa ready nếu `PM_ID` chưa done
- `REVIEW_ID` chưa ready nếu `CODER_ID` chưa done
- `dispatch --dry-run` giúp bạn thấy scheduler logic mà chưa thực sự spawn worker
- bước này là **observe and verify**, không phải quay lại tự tay viết lại toàn bộ graph bằng shell

### Bước 7 — bật dispatcher thật bằng gateway

Nếu bạn muốn task thực sự được pickup, hãy đảm bảo gateway đang chạy:

```bash
hermes gateway status
hermes gateway start
hermes gateway status
```

**Ý chính:** trong Hermes docs hiện tại, dispatcher chạy **bên trong gateway** theo mặc định. Nếu gateway không chạy, task `ready` sẽ chỉ nằm chờ.

Sau khi gateway sẵn sàng, bạn có thể nói ngắn với Jarvis:

```text
Tôi giữ vai trò operator quan sát. Hãy tiếp tục theo graph đã dựng, chỉ escalate khi có blocker thật hoặc prerequisite thiếu.
```

### Bước 8 — theo dõi runtime khi worker bắt đầu làm việc

```bash
hermes kanban list
hermes kanban show <JARVIS_ID>
hermes kanban runs <JARVIS_ID>
hermes kanban runs <CODER_ID>
hermes kanban runs <REVIEW_ID>
```

Khi workflow chạy thật, bạn nên để ý:
- task nào đang `ready`
- task nào đã sang `running`
- run history của từng task có summary/handoff gì
- parent output có đủ để child đọc tiếp không
- Jarvis có đang giữ đúng vai trò orchestrator thay vì ôm luôn implementation không

### Bước 9 — review artifact tiêu chuẩn

Reviewer không nên chỉ nói trong chat. Reviewer nên để lại file kiểu:

```text
docs/reviews/YYYY-MM-DD-todoapp-v1-review.md
```

File đó ít nhất nên có:
- verdict: `approve` hoặc `request-changes`
- changed files reviewed
- critical / important / minor findings
- next actions for coder

**Rule:** coder follow-up nên đọc file artifact này, không phụ thuộc vào transcript sống của reviewer session.

### Bước 10 — recovery commands tối thiểu phải biết

Nếu task bị kẹt hoặc bạn muốn chẩn đoán worker context, dùng:

```bash
hermes kanban runs <CODER_ID>
hermes kanban context <CODER_ID>
hermes kanban show <CODER_ID>
```

Nếu một task đang chạy bị kẹt và bạn muốn chuyển sang profile khác, cú pháp là:

```bash
# Ví dụ cú pháp; thay coder-v2 bằng profile có thật của bạn
hermes kanban reassign <CODER_ID> coder-v2 --reclaim --reason "switch to alternate coder profile"
```

Nếu bạn chưa có profile thay thế, ít nhất hãy hiểu `reassign --reclaim` là lối recovery chuẩn hơn so với ngồi chờ mãi một worker lỗi.

## Hiểu sai thường gặp

- Kanban chỉ là phiên bản phức tạp hơn của `delegate_task`.
- Người vận hành nên tự tay tạo graph và micromanage từng worker thay vì giao orchestration cho Jarvis.
- Tạo profile rỗng là đủ để worker làm việc ổn định.
- Review artifact chỉ là “nice to have”, không phải điều kiện cho coder follow-up chuẩn.

### Anti-pattern phải tránh trong lab này

- tự tay chạy 4 block `hermes kanban create ...` chỉ vì thấy shell làm được
- thấy task body mơ hồ nhưng không yêu cầu Jarvis tự siết lại trước dispatch
- để reviewer kết luận trong chat mà không lưu artifact
- để Jarvis ôm luôn vai coder thay vì thật sự điều phối PM -> Coder -> Reviewer
- gặp gap tool access rồi lập tức quay về manual workflow thay vì sửa môi trường trước

## Prompt lab cho Jarvis

Paste trực tiếp prompt dưới đây cho Jarvis. Mục tiêu là bạn giữ vai trò định hướng + review, còn Jarvis giữ vai trò orchestration + execution support.

Nếu bạn muốn bắt đầu nhanh, paste prompt ngắn này vào Jarvis trước. Sau đó đi tiếp xuống Bước 4 và Bước 5 để dùng bản prompt orchestration đầy đủ hơn.

```text
Jarvis, hãy đóng vai orchestrator cho Lab 04 của tôi.

Objective:
- dựng workflow Jarvis -> PM -> Coder -> Reviewer cho benchmark Todo App,
- để bạn tự tạo graph và tự siết handoff thay vì tôi phải hand-author toàn bộ bằng CLI.

Context:
- Tôi đang học Lab 04 — Kanban Jarvis -> PM -> Coder -> Reviewer.
- Tôi sẽ cung cấp absolute path thật tới repo/practice repo của tôi.
- Tôi muốn CLI chỉ đóng vai trò bootstrap / quan sát / recovery.

Guardrails:
- nếu objective, workspace, artifact path, hoặc dependency còn mơ hồ thì hãy siết lại trước khi dispatch,
- dùng handoff durable trong repo, không dựa vào transcript sống,
- nếu tool access cho Kanban chưa đủ thì chỉ rõ gap môi trường thay vì đẩy tôi về manual workflow.

Deliverables:
- một graph hoặc proposal rõ cho workflow Jarvis -> PM -> Coder -> Reviewer,
- danh sách artifact paths bền trong repo: spec, task handoff, review findings,
- một runtime checklist ngắn: blocker lớn nhất, bước quan sát tiếp theo, và recovery path cơ bản.

Quality standard:
- phải siết objective, workspace, artifact path trước khi dispatch thật,
- phải giữ operator ở boundary bootstrap / quan sát / recovery thay vì manual-manage toàn bộ graph,
- review findings phải sống trong repo như durable artifact, không ở transcript sống,
- nếu Kanban tool access chưa đủ thì phải chỉ rõ gap môi trường và đề xuất recovery path.
```

Prompt orchestration đầy đủ hơn vẫn nằm ở Bước 4; prompt self-review graph nằm ở Bước 5.

## Kết quả mong đợi

Nếu lab đi đúng hướng, bạn phải nhìn được output đủ cụ thể để tự đối chiếu lại bằng phần lý thuyết vừa học.

### Success criteria

- Tạo được một board riêng
- Có đủ 4 profile thực thi được workflow
- Dùng prompt để Jarvis tự tạo graph Jarvis -> PM -> Coder -> Reviewer
- Dùng prompt để Jarvis tự siết lại task body/handoff trước runtime
- Chọn đúng workspace cho spec / code / review
- Hiểu rõ vì sao operator chỉ nên giữ bootstrap / quan sát / recovery loop
- Hiểu rõ vì sao review artifact phải sống trong repo
- Biết ít nhất một recovery path cơ bản: `runs` / `context` / `reassign --reclaim`

### Dấu hiệu cần reject hoặc làm lại

- operator vẫn phải tự hand-author toàn bộ task graph bằng tay thay vì dùng Jarvis orchestration
- không có artifact path rõ cho spec / code / review
- review verdict chỉ nằm trong chat thay vì được ghi thành file trong repo
- output không nói rõ blocker môi trường hoặc recovery path tối thiểu

## Sau lab, từ nay giao gì cho Jarvis

Từ nay, hãy giao cho Jarvis việc bootstrap graph công việc, giữ handoff note, và nhắc bạn khi nào phải can thiệp ở control plane thay vì lao xuống làm thay worker. Sau lab, yêu cầu Jarvis rút workflow thành team-playbook + review-artifact checklist để dùng lại lâu dài và chia sẻ cho người khác.

### Prompt đóng gói để dùng lại

```text
Jarvis, dựa trên lab tôi vừa hoàn thành, hãy làm 4 việc:
1. Tóm tắt boundary/mental model cốt lõi tôi vừa học bằng tiếng Việt ngắn gọn.
2. Rút workflow vừa làm thành một prompt template hoặc checklist tái sử dụng.
3. Chỉ ra 3 pitfalls dễ lặp lại nhất và cách tự kiểm lại output.
4. Viết một handoff note ngắn để lần sau tôi hoặc người khác có thể dùng lại mà không phải học lại từ đầu.
```

### Feynman check

Trả lời không nhìn tài liệu:

1. Vì sao lab này nhấn mạnh **prompt-first** thay vì bắt bạn tự tạo cả graph bằng CLI?
2. Vì sao task reviewer nên dùng `dir:$BOOTCAMP_REPO` thay vì `scratch`?
3. Vì sao task coder thường hợp với `worktree` hơn shared repo trực tiếp?
4. Kanban khác delegation ở tính durable như thế nào?
5. Vì sao nói CLI/dashboard là human control plane, còn `kanban_*` là worker runtime surface?
6. Vì sao gateway lại quan trọng với lab này?
7. Nếu coder task bị kẹt, bạn sẽ nhìn `runs` hay chỉ nhìn `list`? Vì sao?
8. Nếu Jarvis tạo graph xong nhưng artifact path còn mơ hồ, tại sao nên yêu cầu Jarvis tự sửa trước khi dispatch?
