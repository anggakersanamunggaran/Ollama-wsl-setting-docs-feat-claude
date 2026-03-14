# Ollama on Windows + WSL Cheatsheet

## Setup (One-time)

### 1. Install Ollama on Windows
Download and install from: https://ollama.com/download/windows

After install, Ollama runs automatically as a Windows service on port `11434`.

---

### 2. Configure WSL to reach Ollama

Add this to your `~/.bashrc` (or `~/.zshrc`):

```bash
export OLLAMA_HOST=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):11434
```

Apply immediately:
```bash
source ~/.bashrc
```

---

### 3. Verify connection

```bash
curl http://$OLLAMA_HOST/api/tags
```

Expected: JSON response listing installed models (empty array `[]` if none yet).

---

## Managing Models

### Pull a model
```bash
# From Windows PowerShell or CMD
ollama pull llama3.2
ollama pull mistral
ollama pull codellama
ollama pull nomic-embed-text   # for embeddings
```

> Models are stored on Windows. Pull from Windows, not WSL.

### List installed models
```bash
# Windows
ollama list

# From WSL
curl http://$OLLAMA_HOST/api/tags
```

### Remove a model
```bash
# Windows
ollama rm llama3.2
```

---

## Running Models

### Interactive chat (Windows terminal)
```bash
ollama run llama3.2
ollama run mistral
```

### One-shot prompt (Windows)
```bash
ollama run llama3.2 "Explain dependency injection in NestJS"
```

---

## Calling Ollama from WSL / Node.js

### REST API — Generate (single response)
```bash
curl http://$OLLAMA_HOST/api/generate \
  -d '{
    "model": "llama3.2",
    "prompt": "Hello, what can you do?",
    "stream": false
  }'
```

### REST API — Chat (multi-turn)
```bash
curl http://$OLLAMA_HOST/api/chat \
  -d '{
    "model": "llama3.2",
    "messages": [
      { "role": "user", "content": "What is NestJS?" }
    ],
    "stream": false
  }'
```

### Node.js — Using `ollama` npm package
```bash
npm install ollama
```

```typescript
import Ollama from 'ollama';

const ollama = new Ollama({ host: process.env.OLLAMA_HOST ?? 'http://localhost:11434' });

// Generate
const response = await ollama.generate({
  model: 'llama3.2',
  prompt: 'Explain NestJS interceptors',
  stream: false,
});
console.log(response.response);

// Chat
const chat = await ollama.chat({
  model: 'llama3.2',
  messages: [{ role: 'user', content: 'Hello!' }],
});
console.log(chat.message.content);
```

### Node.js — Streaming response
```typescript
const stream = await ollama.chat({
  model: 'llama3.2',
  messages: [{ role: 'user', content: 'Tell me a story' }],
  stream: true,
});

for await (const chunk of stream) {
  process.stdout.write(chunk.message.content);
}
```

---

## Embeddings

```bash
curl http://$OLLAMA_HOST/api/embeddings \
  -d '{
    "model": "nomic-embed-text",
    "prompt": "NestJS is a Node.js framework"
  }'
```

```typescript
const embed = await ollama.embeddings({
  model: 'nomic-embed-text',
  prompt: 'NestJS is a Node.js framework',
});
console.log(embed.embedding); // number[]
```

---

## Environment Variable for NestJS / Node.js projects

Add to your `.env`:
```env
OLLAMA_HOST=http://<windows-host-ip>:11434
```

Get your Windows host IP from WSL:
```bash
cat /etc/resolv.conf | grep nameserver | awk '{print $2}'
```

> Note: The IP may change on WSL restart. For a stable setup, use `localhost` if Ollama is configured to bind to `0.0.0.0` (default on Windows install).

---

## Quick Test — check if it's all working

```bash
# 1. Check Ollama is reachable
curl http://$OLLAMA_HOST/api/tags

# 2. Run a quick prompt
curl http://$OLLAMA_HOST/api/generate \
  -d '{"model":"llama3.2","prompt":"say hi","stream":false}' \
  | jq '.response'
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `Connection refused` | Open Ollama on Windows, check it's running in system tray |
| `OLLAMA_HOST` not set | Re-run `source ~/.bashrc` |
| Model not found | Pull the model first on Windows: `ollama pull <model>` |
| Slow responses | Normal for CPU; GPU is used automatically if available on Windows |
| IP keeps changing | Hardcode the IP or use Windows hostname: `$(hostname).local` |
