# Lab 03B — Reliability, Routing, MCP, ACP, API Server

**Mục tiêu:** hiểu các lớp reliability/integration của Hermes để không trộn lẫn `MCP`, `ACP`, `API Server`, `provider routing`, `fallback providers`, `credential pools`, và `auxiliary` models.  
**Thời lượng:** 35-50 phút.

## Nguồn đối chiếu chính

- MCP (Model Context Protocol)
- ACP Editor Integration
- API Server
- Provider Routing
- Fallback Providers
- Credential Pools
- Configuration (auxiliary models)

## Verified on host trong lab này

```bash
hermes mcp --help
hermes mcp add --help
hermes mcp list
hermes acp --help
hermes fallback --help
hermes fallback list --help
hermes auth add --help
```

## Sourced from docs, chưa verify full flow end-to-end trên host này

- cấu hình MCP server thật rồi kết nối thành công tới một external server cụ thể
- editor ACP flow đầy đủ với VS Code/Zed/JetBrains
- API Server exposure cho frontend ngoài
- fallback chain chạy thật khi provider fail giữa session
- provider routing với OpenRouter sub-providers trên tài khoản thật

## Bước 1 — nhìn command surfaces trước

Chạy:

```bash
hermes mcp --help
hermes mcp add --help
hermes mcp list
hermes acp --help
hermes fallback --help
hermes fallback list --help
hermes auth add --help
```

### Tự quan sát

- command nào thiên về “thêm external capability server”?
- command nào thiên về “editor integration”?
- command nào thiên về “resilience chain”?
- command nào thiên về “credential onboarding”?

## Bước 2 — phân loại 3 kiểu integration

Điền bảng này bằng lời của bạn:

| Nhu cầu | Primitive đúng | Vì sao |
|---|---|---|
| Hermes cần dùng tool sống ở server ngoài | ? | ? |
| Muốn Hermes hiện trong VS Code/Zed/JetBrains | ? | ? |
| Muốn Open WebUI/LobeChat gọi Hermes như backend | ? | ? |

**Expected answer:**
- external tool server -> **MCP**
- editor-native coding agent -> **ACP**
- OpenAI-compatible backend -> **API Server**

## Bước 3 — hiểu reliability stack bằng 4 tình huống

Ghép từng tình huống với khái niệm đúng nhất:

1. “Tôi có 3 OpenRouter keys, muốn tự xoay vòng để giảm rate-limit.”
2. “Provider/model chính chết, tôi muốn Hermes tự thử provider:model khác.”
3. “Tôi dùng OpenRouter và chỉ muốn route qua Anthropic/Google, tránh Together.”
4. “Vision/compression/web extraction quá đắt vì đang ăn cùng model reasoning chính.”

**Expected mapping:**
1. credential pools
2. fallback providers
3. provider routing
4. auxiliary model overrides

## Bước 4 — đọc YAML mental model

### 4.1 Credential pool strategy

```yaml
credential_pool_strategies:
  openrouter: round_robin
  anthropic: least_used
```

Tự giải thích:
- đây là same-provider rotation hay cross-provider fallback?
- vì sao nó đứng trước fallback providers trong resilience stack?

### 4.2 Fallback providers

```yaml
fallback_providers:
  - provider: openrouter
    model: anthropic/claude-sonnet-4
  - provider: anthropic
    model: claude-sonnet-4
```

Tự giải thích:
- đây là cùng provider hay khác provider đều được?
- nó giải bài toán nào mà credential pool không giải được?

### 4.3 Provider routing (OpenRouter only)

```yaml
provider_routing:
  sort: price
  ignore: ["Together"]
  require_parameters: true
  data_collection: deny
```

Tự giải thích:
- cấu hình này chỉ có tác dụng khi nào?
- nó route giữa các **sub-providers bên trong OpenRouter** hay route giữa mọi provider Hermes biết?

### 4.4 Auxiliary models

```yaml
auxiliary:
  vision:
    provider: openrouter
    model: google/gemini-2.5-flash
  approval:
    provider: auto
  compression:
    timeout: 120
```

Tự giải thích:
- vì sao auxiliary là lớp cost-control quan trọng?
- `auto` hiện mặc định bám main model hay mặc định tìm model rẻ riêng?

## Bước 5 — MCP thinking

Đọc lại:

```bash
hermes mcp add --help
hermes mcp list
```

Rồi tự trả lời:

- MCP là “Hermes đi gọi tool ngoài” hay “app ngoài gọi Hermes”?
- khi nào dùng stdio server, khi nào dùng HTTP server?
- vì sao docs nhấn mạnh per-server filtering?

## Bước 6 — ACP thinking

Đọc lại:

```bash
hermes acp --help
```

Tự trả lời:

- ACP có phải chỉ là một chat UI khác không?
- vì sao docs nói ACP có curated toolset thay vì full mọi tool?
- working directory trong ACP nên gắn với editor workspace hay với cwd của server process?

## Bước 7 — API Server thinking

Dựa trên docs gốc, tự giải thích bằng 2-3 câu:

- API Server khác gateway ở đâu?
- API Server khác MCP ở đâu?
- vì sao frontend như Open WebUI có thể dùng Hermes backend mà vẫn hưởng full agent tool loop?

## Success criteria

Bạn coi như hoàn thành lab khi:

- không còn nhầm MCP với API Server,
- không còn nhầm ACP với “một gateway chat khác”,
- giải thích được 4 lớp: credential pools / fallback providers / provider routing / auxiliary overrides,
- hiểu rằng provider routing chỉ áp dụng cho OpenRouter,
- hiểu rằng `auxiliary.auto` hiện kéo side tasks bám main model nên có ảnh hưởng chi phí.
