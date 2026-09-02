# opencode-to-api

A lightweight API gateway that transforms the [OpenCode](https://opencode.ai) CLI into a standard OpenAI-compatible REST API. Use powerful free models (Kimi, GLM, MiniMax, DeepSeek, etc.) in any AI client that supports the OpenAI format (Cursor, Claude Code, OpenClaw, etc.).

---

## Features

- OpenAI-compatible REST API (`/v1/chat/completions`, `/v1/models`)
- Streaming and non-streaming support
- Auto-detects and launches OpenCode backend
- Tool calling support (enabled by default)
- Basic auth for backend security
- Plugin mode for OpenClaw integration

---

## Prerequisites

1. **Node.js**: Version 18.0 or higher.
2. **OpenCode CLI**: Must be installed on your system.
   - **Windows**: `npm install -g opencode-ai`
   - **Linux / macOS**: `curl -fsSL https://opencode.ai/install | bash`

---

## Quick Start (Standalone Mode)

```bash
git clone https://github.com/SirHumza/opencode-to-api.git
cd opencode-to-api
npm install
cp config.json.example config.json
node index.js
```

The gateway starts on `http://127.0.0.1:8083` by default.

---

## Configuration

Edit `config.json` or use environment variables:

| Option | Env Var | Default | Description |
|--------|---------|---------|-------------|
| `PORT` | `PORT` | `8083` | Proxy listen port |
| `API_KEY` | `API_KEY` | `""` | API key for client auth |
| `BIND_HOST` | `BIND_HOST` | `127.0.0.1` | Bind address |
| `OPENCODE_SERVER_URL` | `OPENCODE_SERVER_URL` | `http://127.0.0.1:14097` | Backend URL |
| `OPENCODE_PATH` | `OPENCODE_PATH` | `opencode` | Path to opencode binary |
| `DISABLE_TOOLS` | `OPENCODE_DISABLE_TOOLS` | `false` | Disable tool calling |
| `OPENCODE_SERVER_USERNAME` | `OPENCODE_SERVER_USERNAME` | `""` | Backend auth username |
| `OPENCODE_SERVER_PASSWORD` | `OPENCODE_SERVER_PASSWORD` | `""` | Backend auth password |

---

## OpenClaw Plugin Mode

Install as an OpenClaw plugin:

```bash
openclaw plugins install https://github.com/SirHumza/opencode-to-api
openclaw gateway restart
```

Then sync models:

```bash
openclaw models auth login --provider opencode-to-openai --method local
```

---

## API Endpoints

### Health Check
```bash
curl http://127.0.0.1:8083/health
```

### List Models
```bash
curl http://127.0.0.1:8083/v1/models
```

### Chat Completions
```bash
curl -X POST http://127.0.0.1:8083/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "opencode/mimo-v2.5-free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "stream": false
  }'
```

### Streaming
```bash
curl -X POST http://127.0.0.1:8083/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "opencode/mimo-v2.5-free",
    "messages": [{"role": "user", "content": "Hello!"}],
    "stream": true
  }'
```

---

## Available Models

The proxy exposes all models configured in your OpenCode installation:

- `opencode/mimo-v2.5-free`
- `opencode/nemotron-3-ultra-free`
- `opencode/nemotron-3.5-lightning-free`
- `opencode/ling-3.0-flash-fin-free`
- `opencode/muse-spark-1.2-contributor-free`
- `opencode/muse-spark-1.3-contributor-free`
- `opencode/big-pickle`
- And all Nvidia, Google, and other provider models...

---

## License

MIT
