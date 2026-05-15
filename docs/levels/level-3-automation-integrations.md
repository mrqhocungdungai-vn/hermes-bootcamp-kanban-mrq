# Level 3 — Automation và Integrations

## Mục tiêu level

Sau level này, bạn phải nhìn Hermes không chỉ như một CLI chat tool, mà như một **hệ vận hành agent có trigger, service lifecycle, integration boundaries, và reliability layers**.

Nói ngắn gọn: Level 3 dạy bạn cách đưa **Jarvis** đi ra khỏi terminal đơn lẻ để trở thành một hệ thống:
- chạy theo lịch,
- phản ứng với event,
- sống trên nhiều kênh chat,
- nối vào tool server ngoài,
- hiện diện trong editor hoặc qua OpenAI-compatible API,
- và vẫn có cost/reliability discipline.

## Source docs

**Docs gốc đối chiếu chính cho Level 3:**
- Cron Jobs
- Hooks
- Batch Processing
- Messaging Gateway overview + platform pages
- Voice Mode
- Browser
- Vision
- Text-to-Speech
- MCP (Model Context Protocol)
- ACP Editor Integration
- API Server
- Provider Routing
- Fallback Providers
- Credential Pools
- Configuration (đặc biệt phần `auxiliary.*` và browser/gateway-related config)

## Lesson 3.1 — Chọn đúng trigger primitive: cron, hook, webhook, gateway, batch

**Probe**  
Vì sao docs không gom mọi thứ vào một chữ “automation”?

**Reveal**  
Vì mỗi primitive giải một loại trigger khác nhau:

- **cron** = chạy theo lịch.
- **hook** = phản ứng tại các lifecycle points **bên trong Hermes**.
- **webhook** = phản ứng với event **từ ngoài đẩy vào**.
- **gateway** = biến Hermes thành một **long-lived front door** trên platform chat.
- **batch processing** = chạy số lượng lớn trajectory/job để tạo dữ liệu hoặc benchmark — không phải default daily automation cho operator.

Mental model đúng:
- đừng dùng cron để giả event-driven inbound system,
- đừng dùng webhook để thay thế lifecycle hooks nội bộ,
- đừng dùng gateway như batch runner,
- đừng dùng batch processing như cách “cho team chat với bot”.

Trong framing của repo này, đây là tầng giúp **Jarvis** biết lúc nào nên:
- thức dậy theo lịch,
- nghe tín hiệu hệ thống,
- nhận event từ ngoài,
- hay đứng ở “cửa trước” cho cả team.

**Lab**  
- `docs/labs/lab-03-automation-gateway.md`

**Feynman check**  
Giải thích 1 câu cho từng primitive: cron, hook, webhook, gateway, batch.

## Lesson 3.2 — Hooks không phải webhooks: hook là điểm chạm vào lifecycle nội bộ của Hermes

**Probe**  
Điểm khác nhau cốt lõi giữa `hook` và `webhook` là gì?

**Reveal**  
- **Hook** gắn vào lifecycle nội bộ của Hermes: session start/reset/finalize, pre-tool-call, approval flow, gateway dispatch, subagent stop...
- **Webhook** là endpoint để **thế giới bên ngoài** gửi event vào Hermes.

Docs hooks nhấn mạnh đây là layer cho:
- logging,
- policy interception,
- notifications,
- audit,
- message rewrite/skip,
- session lifecycle side effects.

Vì vậy:
- hook = “Hermes vừa/đang/sắp làm gì?”
- webhook = “Hệ thống bên ngoài vừa đẩy event gì vào Hermes?”

Đây là boundary quan trọng nếu sau này bạn muốn **Jarvis có governance** chứ không chỉ có capability.

**Lab**  
- `docs/labs/lab-03-automation-gateway.md`

**Feynman check**  
Nếu muốn log lại mỗi lần subagent hoàn tất, bạn cần hook hay webhook? Vì sao?

## Lesson 3.3 — Messaging Gateway là background service + security boundary, không chỉ là “chat bot chạy được”

**Probe**  
Tại sao docs gateway nói nhiều về allowlist, pairing, reset policy, service install/start/status?

**Reveal**  
Vì khi Hermes sống trên Telegram/Discord/Slack/... nó trở thành một **shared system**, không còn là công cụ cá nhân trong terminal.

Docs gốc cho thấy gateway là:
- **một background process duy nhất** nối tới nhiều platform,
- có **per-chat session store**,
- chạy luôn **cron scheduler**,
- có **service lifecycle** (`run`, `install`, `start`, `stop`, `status`),
- có **authorization boundary** (allowlist hoặc DM pairing),
- có **slash-command access tiers** giữa admin và regular user,
- có **reset policies** theo daily/idle.

Nghĩa là “bot trả lời được” mới chỉ là tầng demo.  
Để dùng an toàn cho team, bạn còn phải nghĩ về:
- ai được dùng,
- trong scope nào,
- thread/session tách ra sao,
- slash commands nào được phép,
- service này chạy foreground hay cài background,
- logs và restart xử lý ra sao.

Với repo này, đây là bước **Jarvis chuyển từ trợ lý riêng sang front door chung cho team**.

**Lab**  
- `docs/labs/lab-03-automation-gateway.md`

**Feynman check**  
Tại sao “có Discord bot” chưa đủ để kết luận “đã có team assistant an toàn”?

## Lesson 3.4 — Batch Processing là pipeline tạo trajectory/eval data, không phải primitive mặc định cho daily ops

**Probe**  
Khi thấy chữ “batch”, rất dễ nghĩ đó là automation thường ngày. Docs gốc có đồng ý không?

**Reveal**  
Không. Docs Batch Processing mô tả nó chủ yếu cho:
- **training data generation**,
- **evaluation**,
- chạy nhiều prompt song song ở quy mô lớn,
- xuất structured trajectories + tool stats + checkpointing.

Tức là batch runner thiên về:
- dataset JSONL,
- worker processes,
- checkpoint resume,
- statistics,
- toolset distributions,
- reasoning coverage.

Đây **không phải** primitive đầu tiên để giải bài toán “mỗi sáng gửi summary”, “team chat với bot”, hay “GitHub push thì xử lý”.

Rule học tập:
- daily ops -> nghĩ cron / webhook / gateway trước,
- large-scale data generation or eval -> nghĩ batch processing.

## Lesson 3.5 — MCP, ACP, API Server là ba kiểu tích hợp khác nhau, đừng gộp thành một cục “integration”

**Probe**  
Cả ba đều là integration. Vậy vì sao docs tách riêng?

**Reveal**  
Vì role kiến trúc khác nhau:

- **MCP**: Hermes **kết nối ra ngoài** để dùng external tool servers.
- **ACP**: editor/IDE **nói chuyện với Hermes** như một coding agent server.
- **API Server**: frontend/app khác gọi Hermes qua **OpenAI-compatible HTTP API**.

### MCP
Docs MCP nhấn mạnh:
- có **stdio servers** và **HTTP servers**,
- tool được auto-discover ở startup,
- có **per-server filtering**,
- utility wrappers chỉ xuất hiện khi server support,
- tool names được prefix để tránh collision.

MCP là cách sạch để cho Jarvis có thêm “tay chân” ngoài Hermes core.

### ACP
Docs ACP nhấn mạnh:
- Hermes chạy như **ACP server** cho editor,
- có curated `hermes-acp` toolset,
- approvals/file diffs/tool activity render trong editor,
- session behavior và working directory ràng buộc với editor workspace.

ACP là đường để Jarvis xuất hiện **trong môi trường code editor**.

### API Server
Docs API Server nhấn mạnh:
- Hermes lộ diện như **OpenAI-compatible backend**,
- frontend như Open WebUI/LobeChat/LibreChat có thể dùng Hermes làm backend,
- request vẫn đi qua agent với full toolset, không chỉ text completion thường.

API Server là đường để **hệ thống khác gọi Jarvis như backend**.

**Lab**  
- `docs/labs/lab-03b-routing-reliability.md`

**Feynman check**  
Cho 1 ví dụ thực tế để phân biệt MCP, ACP, và API Server.

## Lesson 3.6 — Reliability stack của Hermes có nhiều tầng: credential pools -> fallback providers -> auxiliary routing

**Probe**  
Tại sao docs lại có riêng `provider routing`, `fallback providers`, `credential pools`, và `auxiliary` config?

**Reveal**  
Vì production-ish automation không chỉ là “chọn model nào”, mà là bài toán:
- quota exhaustion,
- same-provider key rotation,
- cross-provider failover,
- cost/speed/privacy routing,
- side-task cost control.

### 1. Credential Pools
Dùng khi bạn có **nhiều key cho cùng một provider**.  
Đây là layer đầu tiên để giảm rate-limit/failure cho cùng provider.

### 2. Fallback Providers
Dùng khi **provider/model chính fail** và bạn muốn Hermes chuyển sang provider:model khác.  
Docs gốc mô tả đây là resilience layer kế tiếp, khác với credential pools.

### 3. Auxiliary task routing
Vision, browser screenshot analysis, web extract, approval, compression, session search... có thể đi model/provider riêng.  
Docs hiện nhấn mạnh rằng `auxiliary.*.provider: auto` **mặc định dùng main model**, nên nếu main model đắt thì side tasks cũng đắt theo.

### 4. Provider Routing
`provider_routing` chỉ có ý nghĩa khi dùng **OpenRouter**.  
Nó điều khiển sub-providers bên dưới OpenRouter theo cost/speed/order/privacy, chứ không phải general fallback cho mọi provider.

Tóm gọn:
- nhiều key cùng provider -> **credential pools**,
- primary provider/model fail -> **fallback providers**,
- muốn chọn sub-provider trong OpenRouter -> **provider routing**,
- muốn side task đi model khác -> **auxiliary config**.

Đây là layer “operator maturity” rất quan trọng cho Jarvis nếu bạn muốn chạy bền và có kiểm soát chi phí.

**Lab**  
- `docs/labs/lab-03b-routing-reliability.md`

**Feynman check**  
Nếu OpenRouter key của bạn bị rate-limit thì khi nào nghĩ tới credential pools, khi nào nghĩ tới fallback providers?

## Lesson 3.7 — Browser, Vision, Voice, TTS là capability expansion; mạnh nhưng kéo theo cost và complexity

**Probe**  
Vì sao khóa học không đặt browser/vision/voice làm trung tâm ngay từ đầu?

**Reveal**  
Vì trục chính của repo là:
- Kanban,
- agent team,
- durable handoff,
- orchestration discipline.

Browser/Vision/Voice/TTS là capability rất mạnh, nhưng docs gốc cho thấy chúng kéo theo thêm nhiều layer:
- auxiliary model selection,
- timeout tuning,
- gateway/platform support khác nhau,
- browser backend/session behavior,
- voice path (CLI mic, messaging voice, Discord voice channel),
- cost của multimodal side tasks.

Rule học ở đây:
- đừng gắn quá nhiều capability lên Jarvis trước khi Jarvis có trigger discipline + security boundary + reliability boundary.
- nếu main model đắt, phải nhớ rằng `auxiliary.auto` thường kéo side tasks dùng cùng model đó.

## Lý thuyết cần nắm

- “Automation” trong Hermes thực ra gồm nhiều primitive khác nhau: cron, hook, webhook, gateway, batch.
- Hook là lifecycle nội bộ; webhook là inbound event từ bên ngoài; gateway là long-lived front door; batch thiên về data/eval.
- Messaging gateway là **service + security boundary**, không chỉ là bot trả lời được.
- MCP, ACP, API Server là 3 kiểu integration khác vai trò kiến trúc.
- Reliability stack có nhiều lớp: credential pools -> fallback providers -> provider routing/OpenRouter -> auxiliary task routing.
- Browser/Vision/Voice/TTS là capability expansion mạnh nhưng kéo theo cost và complexity.


### Sơ đồ mental model

```text
Use case đến từ đâu?
        |
        +--> Theo lịch ------------------> [Cron]
        |
        +--> Lifecycle nội bộ Hermes ---> [Hook]
        |
        +--> Event từ hệ ngoài ---------> [Webhook]
        |
        +--> Người chat với Hermes -----> [Gateway]
        |
        +--> Dataset / trajectory batch -> [Batch processing]

Reliability layer chạy ngang:
[credential pools] -> [fallback providers] -> [auxiliary routing]
```

## Hiểu sai thường gặp

1. “Cái gì tự động cũng gọi là cron.” -> Sai, trigger primitive phải chọn đúng theo bài toán.
2. “Hook và webhook chỉ khác tên.” -> Sai, chúng chạm vào hai phía khác nhau của hệ thống.
3. “Gateway là bot demo.” -> Sai, nó là service runtime có auth, pairing, session store, scheduler.
4. “MCP/ACP/API Server đều là integration như nhau.” -> Sai, direction và role kiến trúc khác nhau.
5. “Chọn model xong là reliability ổn.” -> Sai, còn nhiều lớp key rotation, fallback, auxiliary routing.

## Prompt lab cho Jarvis

```text
Jarvis, hãy đóng vai automation architect cho Level 3.

Mục tiêu:
- giúp tôi chọn đúng trigger primitive cho từng tình huống,
- giúp tôi hiểu service/security boundary của gateway,
- dẫn tôi qua các lab automation và reliability bằng tư duy kiến trúc, không chỉ command syntax.

Cách làm:
1. Đưa cho tôi 5 tình huống và bắt tôi chọn giữa cron/hook/webhook/gateway/batch.
2. Dẫn tôi qua `lab-03-automation-gateway.md` và `lab-03b-routing-reliability.md`.
3. Nếu tôi chọn sai primitive hoặc integration surface, hãy nói rõ tôi đang lẫn boundary nào.
4. Kết thúc bằng một decision memo ngắn: với hệ thống của tôi, khi nào nên dùng gateway, khi nào nên dùng webhook, khi nào cần fallback/auxiliary tuning.
```

## Kết quả mong đợi

- Phân biệt đúng cron vs hook vs webhook vs gateway vs batch.
- Giải thích được vì sao gateway là service + security boundary.
- Giải thích hook khác webhook ở đâu.
- Biết batch processing dùng cho loại bài toán nào.
- Phân biệt MCP vs ACP vs API Server.
- Phân biệt credential pools vs fallback providers vs provider routing vs auxiliary routing.
- Biết vì sao browser/vision/voice/TTS nên được thêm sau khi operator discipline đã chắc.

## Sau lab, từ nay giao gì cho Jarvis

- chọn đúng primitive automation cho từng yêu cầu mới,
- phác thảo gateway/security/reliability architecture trước khi triển khai,
- audit xem một ý tưởng nên đi qua MCP, ACP, API Server, hay không cần integration mới,
- đề xuất cấu hình fallback/auxiliary khi workload bắt đầu tốn kém hoặc dễ fail.

## Next

- `docs/levels/level-4-agent-teams-kanban.md`
- `docs/labs/lab-04-kanban-pm-coder-reviewer.md`
