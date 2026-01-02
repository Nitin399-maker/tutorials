# How to Use OpenCode with LLM Foundry + Public APIs (Windows 11)

This guide helps you set up **OpenCode** to work across:

- **LLM Foundry APIs** (Straive router): OpenAI, Azure OpenAI, Gemini, OpenRouter
- **Public APIs**: OpenAI, Anthropic, OpenRouter, Kimi (Moonshot)

It is written in the same “install → configure → verify” style needed for Anand’s compatibility matrix.

---

## 1. Install prerequisites

### Option A — Install via official installer (easiest)

1. Open the Node.js download page: https://nodejs.org/
2. Download **LTS** (click on Get Nodejs)![](getnode.webp)

3. Download and Run the **.msi** installer → keep defaults (make sure it includes **npm**)![](msi.webp)
4. Restart your terminal (**Command Prompt** / **PowerShell**).

    ### Verify Installation

    ```bash 
    node -v
    npm -v
    ```
### Option B (recommended): Use WSL (Ubuntu)
Codex CLI works best in a Linux-like shell.

1. Install WSL + Ubuntu from Microsoft Store
2. Open **Ubuntu** (WSL) and run:

```bash
sudo apt update
sudo apt install -y nodejs npm
node -v
npm -v
```

> If your Ubuntu repo has an older Node.js, install a newer Node via NodeSource or nvm.



---

## 1. Install VS Code

Download [Visual Studio Code](https://code.visualstudio.com/) and install it.

[![How Install Visual Studio Code on Windows 11 (VS Code) - 6 min](https://i.ytimg.com/vi_webp/cu_ykIfBprI/sddefault.webp)](https://youtu.be/cu_ykIfBprI)

Then open any folder in VS Code.

![](vscode-open-folder.webp)



## 1) Install OpenCode

### Install via npm (requires Node.js)
```bash
npm i -g opencode-ai

```

---

## 2) Where OpenCode stores config 

**Global config (for all projects)**  
   - Windows: `C:\Users\<you>\.config\opencode\opencode.json`  
   - Linux/macOS: `~/.config/opencode/opencode.json`

---

## 3) Add credentials (API keys) for providers

Run:
```powershell
opencode auth login
```

### 3.1 Add LLM Foundry credentials (repeat 4 times)

Each time:
- Select provider: **Other**
- Enter provider id: one of the following
- Enter your API key: paste your **LLMFOUNDRY_TOKEN** (same token every time)

Do these 4 credentials:

1) `foundry-openai`  
2) `foundry-openrouter`  
3) `foundry-gemini`  
4) `foundry-azure`

### 3.2 Add Public API credentials (repeat per provider)

Run `opencode auth login` again and add:

- Provider: **OpenAI** (built-in) → paste your OpenAI key
- Provider: **OpenRouter** (built-in) → paste your OpenRouter key
- Provider: **Anthropic** (built-in) → paste your Anthropic key
- Provider: **Moonshot AI** (if shown) → paste your Moonshot/Kimi key  
  - If Moonshot is not shown, use provider **Other** and provider id `moonshot`.

---

## 4) Create global `opencode.json` (LLM Foundry endpoints + Kimi)

Create the folder (if needed) and open the file:

```powershell
mkdir $env:USERPROFILE\.config\opencode -Force
notepad $env:USERPROFILE\.config\opencode\opencode.json
```

Paste the following **complete** config and save:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "foundry-openai": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LLM Foundry - OpenAI",
      "options": {
        "baseURL": "https://llmfoundry.straive.com/openai/v1"
      },
      "models": {
        "gpt-5": { "name": "gpt-5" },
        "gpt-5-mini": { "name": "gpt-5-mini" }
      }
    },
    "foundry-openrouter": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LLM Foundry - OpenRouter",
      "options": {
        "baseURL": "https://llmfoundry.straive.com/openrouter/v1"
      },
      "models": {
        "openai/gpt-5-codex": { "name": "openai/gpt-5-codex" },
        "openai/gpt-4o": { "name": "openai/gpt-4o" }
      }
    },
    "foundry-gemini": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LLM Foundry - Gemini",
      "options": {
        "baseURL": "https://llmfoundry.straive.com/gemini/v1beta/openai"
      },
      "models": {
        "gemini-2.5-flash": { "name": "gemini-2.5-flash" }
      }
    },
    "foundry-azure": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LLM Foundry - Azure",
      "options": {
        "baseURL": "https://llmfoundry.straive.com/azure/openai/deployments/gpt-5",
        "queryParams": {
          "api-version": "2025-04-01-preview"
        }
      },
      "models": {
        "gpt-5": {
          "name": "gpt-5",
        }
      }

    },
    "foundry-anthropic": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LLM Foundry - Anthropic",
      "options": {
        "baseURL": "https://llmfoundry.straive.com/anthropic/v1"
      },
      "models": {
        "claude-sonnet-4": { "name": "claude-sonnet-4" }
      }
    }
  }
}

```

Notes:
- The 4 `foundry-*` providers are **OpenAI-compatible** endpoints behind LLM Foundry.
- The `moonshot` provider points to Kimi’s public endpoint (Moonshot).

---

## 5) Restart OpenCode

Close and reopen your terminal (important after config/credential changes).

---

## 6) Verify (Anand “Hi” smoke test)

Run these one by one:

```powershell
opencode run -m foundry-openai/gpt-5.2 "Hi"
opencode run -m foundry-openrouter/openai/gpt-5-codex "Hi"
opencode run -m foundry-gemini/gemini-2.5-flash "Hi"
opencode run -m moonshot/kimi-k2-thinking "Hi"
```

---
