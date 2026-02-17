# OpenClaw Ollama Bug Report - Project Folder

## Contents

```
openclaw-ollama-bug/
├── BUG-REPORT.md          ← Full bug report (submit this to GitHub)
├── README.md              ← This file
└── screenshots/
    ├── 01-ollama-install-and-pull.png         ← Ollama installed successfully
    ├── 02-onboarding-no-ollama-provider.png   ← BUG: Ollama missing from provider list
    ├── 03-openclaw-install-and-onboard.png    ← OpenClaw installed successfully
    ├── 04-manual-model-entry-warning.png      ← BUG: "Unknown model" warning
    ├── 05-onboarding-complete.png             ← Onboarding finished
    ├── 06-chat-no-response.png                ← BUG: Chat stuck with no response
    ├── 07-ollama-1b-pulled-gateway-logs.png   ← Proof Ollama works in terminal
    └── 08-chat-still-no-response.png          ← BUG: Still no response after fix attempt
```

## How to Submit

1. Go to the OpenClaw GitHub repository
2. Click **Issues** → **New Issue**
3. Copy and paste the contents of `BUG-REPORT.md`
4. Attach the screenshots from the `screenshots/` folder
5. Submit!

## System Info

- **Machine:** Microsoft Surface Laptop 2
- **OS:** Ubuntu 24.04 (migrated from Windows)
- **CPU:** Intel i5-8250U (4 cores, no GPU)
- **RAM:** 7.7GB
- **OpenClaw:** 2026.2.15
- **Ollama:** Latest
- **Models:** llama3.2:latest, llama3.2:1b
