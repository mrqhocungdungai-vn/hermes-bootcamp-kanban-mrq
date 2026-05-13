# Docs Index — Chọn lộ trình học Hermes đúng level

File này không phải file dump. Nó là **router** để bạn tìm đúng điểm bắt đầu trong dưới 30 giây.

## Nếu bạn là ai, hãy bắt đầu ở đâu?

### 1. Tôi mới với Hermes
Bắt đầu:
1. `docs/levels/level-0-khoi-dong-va-mental-model.md`
2. `docs/labs/lab-00-day-0-setup.md`
3. `docs/labs/lab-01-first-chat-sessions.md`

### 2. Tôi đã cài Hermes, nhưng mental model còn mơ hồ
Bắt đầu:
1. skim `docs/levels/level-0-khoi-dong-va-mental-model.md`
2. đọc kỹ `docs/levels/level-1-core-operator.md`
3. làm `docs/labs/lab-01-first-chat-sessions.md`
4. làm tiếp `docs/labs/lab-02-models-tools-skills-memory.md`
5. rồi `docs/labs/lab-02b-context-files-profiles.md`

### 3. Tôi muốn học team workflow chuẩn Jarvis -> PM -> Coder -> Reviewer
Đừng nhảy thẳng ngay. Hãy đọc:
1. `docs/levels/level-1-core-operator.md`
2. `docs/levels/level-2-context-skills-memory-profiles.md`
3. `docs/levels/level-4-agent-teams-kanban.md`
4. `docs/labs/lab-04-kanban-pm-coder-reviewer.md`

### 4. Tôi muốn automation / bot / MCP / gateway
Đọc:
1. `docs/levels/level-1-core-operator.md`
2. `docs/levels/level-3-automation-integrations.md`
3. `docs/labs/lab-03-automation-gateway.md`
4. `docs/labs/lab-03b-routing-reliability.md`

### 5. Tôi muốn build/extend Hermes như một builder
Đọc:
1. `docs/levels/level-2-context-skills-memory-profiles.md`
2. `docs/levels/level-3-automation-integrations.md`
3. `docs/levels/level-5-advanced-builder-capstone.md`
4. `docs/labs/lab-05-builder-capstone.md`

## Reading flow chuẩn

**Canonical path cho đa số learner:**  
`README.md` -> `docs/index.md` -> `level-0` -> `lab-00` -> `level-1` -> `lab-01` -> `level-2` -> `lab-02` -> `lab-02b` -> `level-3` -> `lab-03` -> `lab-03b` -> `level-4` -> `lab-04` -> `level-5` -> `lab-05`

Quick path chỉ là **đường tắt có điều kiện**. Nếu bạn thấy mơ hồ ở bất kỳ boundary nào, quay về canonical path.

## Level map

| Level | File | Câu hỏi level này trả lời |
|---|---|---|
| 0 | `docs/levels/level-0-khoi-dong-va-mental-model.md` | Hermes là gì? Setup/doctor/auth/session/profile nghĩa là gì? |
| 1 | `docs/levels/level-1-core-operator.md` | Làm sao dùng Hermes như operator an toàn và có kiểm soát? |
| 2 | `docs/levels/level-2-context-skills-memory-profiles.md` | Làm sao dạy Hermes, nhớ đúng, nạp context đúng, tách profile đúng? |
| 3 | `docs/levels/level-3-automation-integrations.md` | Làm sao mở rộng Hermes sang trigger/service/integration/reliability layers mà không trộn lẫn chúng? |
| 4 | `docs/levels/level-4-agent-teams-kanban.md` | Làm sao để Jarvis làm orchestrator và điều phối agent team bền vững bằng Kanban? |
| 5 | `docs/levels/level-5-advanced-builder-capstone.md` | Làm sao thành builder/operator nâng cao và tự thiết kế capstone? |

## Quick paths theo mục tiêu

### Quick path A — Solo operator mạnh hơn
**Prereq tối thiểu:** skim Level 0, hoàn thành Lab 01.  
Đường đi: Level 0 -> Level 1 -> Level 2 -> Lab 02 -> Lab 02B

### Quick path B — Team assistant / bot cho group
**Prereq tối thiểu:** skim Level 0, hiểu Level 1, hoàn thành Lab 01.  
Đường đi: Level 0 -> Level 1 -> Level 3 -> Lab 03 -> Lab 03B

### Quick path C — Jarvis orchestrator điều phối nhiều agent
**Prereq tối thiểu:** Level 1 + Level 2 + Lab 02B.  
Đường đi: Level 0 -> Level 1 -> Level 2 -> Lab 02B -> Level 4 -> Lab 04

### Quick path D — Builder / plugin / internals
**Prereq tối thiểu:** Level 1 + Level 2; nên đã xem ít nhất một lab automation.  
Đường đi: Level 1 -> Level 2 -> Level 3 -> Lab 03B -> Level 5 -> Lab 05

## Nếu muốn skip level

- Muốn vào thẳng Kanban: vẫn phải skim Level 1 và Level 2, vì nếu không bạn sẽ lẫn `profile`, `workspace`, `skill`, `review artifact`.
- Muốn vào thẳng MCP/API/plugin: vẫn nên có Level 1, vì config/provider/toolset là nền.
- Muốn vào thẳng gateway: vẫn nên biết session isolation và approvals trước.

## Rule để tự đánh giá tiến độ

Chỉ được coi là “qua level” khi bạn đạt **exit criteria** trong file level tương ứng, không phải chỉ đọc xong.
