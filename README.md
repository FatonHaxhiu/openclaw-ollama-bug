# 🦞 OpenClaw + Ollama Bug Report

> **Local AI model (Ollama) never returns a response in OpenClaw chat — even after successful configuration.**

---

## 📋 Overview

This repository documents a bug encountered while setting up **OpenClaw v2026.2.15** with a **local Ollama model** on an Ubuntu machine with no GPU. Despite Ollama working correctly on its own, OpenClaw's chat interface never responds — it just shows a spinning "thinking" indicator indefinitely.

This repo contains:
- A full detailed bug report (`BUG-REPORT.md`)
- 8 screenshots documenting every step of the process
- All configuration details and logs needed to reproduce the issue

---

## 🖥️ Test Environment

| Component | Details |
|-----------|---------|
| **Machine** | Microsoft Surface Laptop 2 |
| **OS** | Ubuntu 24.04 (migrated from Windows) |
| **CPU** | Intel Core i5-8250U @ 1.60GHz (4 cores) |
| **RAM** | 7.7GB |
| **GPU** | ❌ None — CPU-only mode |
| **OpenClaw** | v2026.2.15 (3fe22ea) |
| **Node.js** | v22.12.0 |
| **Ollama** | Latest |
| **Models Tested** | llama3.2:latest (3B), llama3.2:1b |

---

## 🐛 Bugs Found

### Bug 1 — Ollama Missing from Onboarding Provider List
During `openclaw onboard`, Ollama does not appear as a provider option at all. OpenAI, Anthropic, Google and many others are listed, but there is no Ollama or "local model" option. The user has no way to configure Ollama through the official wizard.

### Bug 2 — "Unknown model" Error After Manual Entry
When the user manually types `ollama/llama3.2`, the onboarding wizard warns:
```
Model not found: ollama/llama3.2
No auth configured for provider "ollama". The agent may fail until credentials are added.
```
And the gateway logs confirm:
```
Error: Unknown model: ollama/llama3.2
Embedded agent failed before reply: Unknown model: ollama/llama3.2
```

### Bug 3 — `baseURL` Is Not a Valid Config Key
Adding `baseURL` to the model config in `openclaw.json` crashes the gateway:
```
agents.defaults.models.ollama/llama3.2: Unrecognized key: "baseURL"
Config invalid — Run: openclaw doctor --fix
```

### Bug 4 — Chat Never Responds Even After Workaround
After applying a manual `models.providers.ollama` config workaround using the `openai-responses` API and `/v1` endpoint, the gateway starts cleanly and confirms `agent model: ollama/llama3.2` — but the chat still never returns a response. No error appears in the logs, it just silently times out.

---

## ✅ Proof That Ollama Works Fine

Ollama responds instantly when tested directly in the terminal:

```bash
$ ollama run llama3.2:1b "say hello"
Hello. Is there something I can help you with or would you like to chat?
```

This confirms the issue is **inside OpenClaw**, not Ollama itself.

---

## 📁 Repository Structure

```
openclaw-ollama-bug/
├── BUG-REPORT.md                                   ← Full bug report with all details
├── README.md                                        ← This file
└── screenshots/
    ├── 01-ollama-install-and-pull.png              ← Ollama installed successfully
    ├── 02-onboarding-no-ollama-provider.png        ← BUG 1: Ollama missing from list
    ├── 03-openclaw-install-and-onboard.png         ← OpenClaw installed successfully
    ├── 04-manual-model-entry-warning.png           ← BUG 2: "Unknown model" warning
    ├── 05-onboarding-complete.png                  ← Onboarding finished
    ├── 06-chat-no-response.png                     ← BUG 4: Chat stuck, no response
    ├── 07-ollama-1b-pulled-gateway-logs.png        ← Proof Ollama works in terminal
    └── 08-chat-still-no-response.png               ← BUG 4: Still no response after fix
```

---

## 🔧 What Was Tried

1. ✅ Installed Ollama via official install script
2. ✅ Pulled `llama3.2:latest` and `llama3.2:1b` models
3. ✅ Installed OpenClaw via `sudo npm install -g openclaw@latest`
4. ✅ Ran `openclaw onboard` — Ollama not listed, skipped provider
5. ✅ Manually entered `ollama/llama3.2` as default model
6. ✅ Set `OLLAMA_BASE_URL=http://localhost:11434` as environment variable
7. ✅ Set `OLLAMA_API_KEY=ollama` as environment variable
8. ✅ Manually configured `models.providers.ollama` with `openai-responses` API
9. ✅ Reinstalled Ollama after binary corruption (segfault)
10. ❌ **Chat never responded in any configuration**

---

## 💡 Expected Behavior

- Ollama should appear as a provider during `openclaw onboard`
- After configuration, the chat should return a response from the local model
- CPU-only mode should be supported for users without a GPU

---

## 📎 Related

- Full bug report: [`BUG-REPORT.md`](./BUG-REPORT.md)
- OpenClaw docs: https://docs.openclaw.ai
- Ollama: https://ollama.com
