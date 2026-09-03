# opencode-to-api

<p align="center">
  <strong>OpenAI-compatible API gateway for <a href="https://opencode.ai">OpenCode</a></strong><br>
  Use free AI models in Cursor, Claude Code, OpenClaw, and any OpenAI-compatible client.
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#configuration">Config</a> •
  <a href="#openclaw-plugin">OpenClaw</a> •
  <a href="#api">API</a> •
  <a href="#models">Models</a>
</p>

---

## What is this?

opencode-to-api turns your [OpenCode CLI](https://opencode.ai) into a local OpenAI-compatible API server. Point any OpenAI-supported client at it and access all your configured models — free tier included.

```
┌─────────────┐      ┌──────────────────┐     ┌─────────────┐
│   Cursor    │────▶│  opencode-to-api │────▶│   OpenCode  │
│ Claude Code │      │   :8083/v1       │     │   Backend   │
│  OpenClaw   │      └──────────────────┘     └─────────────┘
└─────────────┘
```

---

## Features

- **Full OpenAI API** — `/v1/chat/completions`, `/v1/models`, streaming support
- **Tool Calling** — enabled by default, works with agent workflows
- **Auto-Backend** — launches OpenCode server automatically if not running
- **Basic Auth** — secure backend with username/password
- **OpenClaw Plugin** — native integration with graphical config
- **Cross-Platform** — Linux, macOS, Windows

---

## Quick Start

### Prerequisites

- **Node.js** 18+
- **OpenCode CLI** — [Install Guide](https://opencode.ai)

```bash
# Install OpenCode (Linux/macOS)
curl -fsSL https://opencode.ai/install | bash

# Ubuntu snap alternative
sudo snap install opencode --classic
```

### Run the Proxy

```bash
git clone https://github.com/SirHumza/opencode-to-api.git
cd opencode-to-api
npm install
cp config.json.example config.json
node index.js
```

Gateway runs at `http://127.0.0.1:8083`

---

## Configuration

Edit `config.json` or set environment variables:

```json
{
  "PORT": 8083,
  "API_KEY": "",
  "BIND_HOST": "127.0.0.1",
  "DISABLE_TOOLS": false,
  "OPENCODE_SERVER_URL": "http://127.0.0.1:14097",
  "OPENCODE_PATH": "opencode",
  "OPENCODE_SERVER_USERNAME": "",
  "OPENCODE_SERVER_PASSWORD": ""
}
```

| Setting | Env Var | Default | Description |
|---------|---------|---------|-------------|
| `PORT` | `PORT` | `8083` | Listen port |
| `API_KEY` | `API_KEY` | `""` | Client auth (empty = no auth) |
| `DISABLE_TOOLS` | `OPENCODE_DISABLE_TOOLS` | `false` | Disable tool calls |
| `OPENCODE_SERVER_USERNAME` | `OPENCODE_SERVER_USERNAME` | `""` | Backend auth user |
| `OPENCODE_SERVER_PASSWORD` | `OPENCODE_SERVER_PASSWORD` | `""` | Backend auth pass |

---

## OpenClaw Plugin

```bash
openclaw plugins install https://github.com/SirHumza/opencode-to-api
openclaw gateway restart
openclaw models auth login --provider opencode-to-api --method local
```

---

## API

### Health
```bash
curl http://127.0.0.1:8083/health
# {"status":"ok","backend":"http://127.0.0.1:14097"}
```

### List Models
```bash
curl http://127.0.0.1:8083/v1/models
```

### Chat (Non-Streaming)
```bash
curl -X POST http://127.0.0.1:8083/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "opencode/mimo-v2.5-free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "stream": false
  }'
```

### Chat (Streaming)
```bash
curl -X POST http://127.0.0.1:8083/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "opencode/mimo-v2.5-free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "stream": true
  }'
```

### With Auth
```bash
curl -H "Authorization: Bearer your-api-key" http://127.0.0.1:8083/v1/models
```

---

## Models

All models from your OpenCode installation are available:

**Free Models:**
- `opencode/mimo-v2.5-free`
- `opencode/nemotron-3-ultra-free`
- `opencode/nemotron-3.5-lightning-free`
- `opencode/ling-3.0-flash-fin-free`
- `opencode/muse-spark-1.2-contributor-free`
- `opencode/muse-spark-1.3-contributor-free`
- `opencode/big-pickle`

**Nvidia NIM:**
- `nvidia/deepseek-ai/deepseek-v4-flash`
- `nvidia/meta/llama-3.3-70b-instruct`
- `nvidia/qwen/qwen3.5-397b-a17b`
- `nvidia/moonshotai/kimi-k3`
- ...and 100+ more

**Google:**
- `google/gemini-2.5-flash`
- `google/gemini-2.5-pro`
- `google/gemma-4-31b-it`

---

## Client Setup

### Cursor
1. Settings → Models → OpenAI API Key
2. Set base URL to `http://127.0.0.1:8083/v1`
3. Select any model from the list

### Claude Code
```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:8083/v1
claude
```

### Any OpenAI Client
Set base URL to `http://127.0.0.1:8083/v1` and use any model ID.

---

## License

MIT
