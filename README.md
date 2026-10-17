# EastClub Agent 🧠

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Cloudflare%20Workers-orange)](#deploy)
[![Version](https://img.shields.io/badge/version-v1.4.2-8A2BE2)](#changelog)
[![Free](https://img.shields.io/badge/powered%20by-GLM%20%2B%20Gemini%20%28free%29-green)](#ai-models)

> **A single-file AI companion that runs entirely on Cloudflare Workers — zero build, zero server, deploy in minutes.**

EastClub Agent is a self-contained Web application packed into **one `worker.js`**. It gives you a password-protected chat with a **persona-driven agent (CDY)** who has his own personality, memory and daily "dreams" — plus image generation, a community of numbered accounts, a chatroom, polls, an admin panel, **LaTeX math rendering**, multiple visual themes and a treasure trove of easter eggs. Everything uses **free AI tiers**, so it costs nothing to run.

---

## ✨ Features

- 🔐 **Password-gated access** — visitors unlock the app with a password before anything loads.
- 🎭 **Persona-driven agent (CDY)** — the model name is presented as *"a rational CDY (powered by …)"*, giving each model a distinct personality.
- 🧠 **Persistent memory + Dreaming** — every conversation is distilled into agent memories; a nightly cron ("Dreaming mode") organizes them into a growing memory document, so CDY's personality keeps maturing (OpenClaw-style).
- 🖼️ **Image generation** — free **Pollinations.ai** drawing, saved to your gallery, conversation and memories.
- 📚 **LaTeX math rendering** — inline `$x^2+1$` and display `$$\frac{a}{b}=\frac{c}{d}$$` rendered with **KaTeX**.
- 👥 **Community** — 1–56 numbered accounts with signup/login, hashed passwords, daily check-ins, CDY-coin economy, a leaderboard, a **chatroom** and a **poll station**, all managed from one admin panel.
- 💬 **Cross-model context** — switch models mid-conversation and keep the text context; image history is auto-omitted for text-only models.
- 🎨 **Multiple themes** — Skylake, Matrix, Midnight, Sakura, Paper + liquid-glass visuals, dynamic light effects and mobile-first UI.
- 🌐 **Bilingual** — Chinese / English toggle.
- 🔎 **Free web search** — DuckDuckGo (no paid grounding).
- 🥚 **Easter eggs & hidden commands** — Matrix intro, Konami code, hidden `/commands`, logo-tap surprises and more.

### AI Models (all free tiers)

| Provider | Model |
|----------|-------|
| BigModel (智谱) | GLM-4.7-Flash, GLM-4.5-Flash, GLM-4-Flash-250414, GLM-4.6V-Flash (vision), GLM-4.1V-Thinking-Flash (vision+thinking) |
| Gemini | Gemini-3.8-Flash, Gemini-3.5-Flash-Lite, Gemini-3.1-Flash-Lite |

No paid models, no paid tools.

---

## 🚀 Deploy to Cloudflare Workers

1. **Get the code** — clone this repo, or grab `worker.js` directly.
2. **Create a Worker** in the Cloudflare Dashboard → Workers & Pages → *Create* → *Upload* → paste `worker.js` content → **Save and Deploy**.
3. **Bind a KV namespace** (required for memory, sessions, gallery & community):
   - Create a KV namespace named **`ZG_KV`** (or `CDY_KV` / `KV`) and bind it to the Worker.
4. **Set a Cron trigger** (for Dreaming mode) — schedule `0 18 * * *` (daily) under *Triggers → Cron Triggers*.
5. **Add API keys as Secrets** (Settings → Variables & Secrets):
   - `ZHIPU_API_KEY` — for GLM models (from [BigModel](https://open.bigmodel.cn))
   - `GEMINI_API_KEY` — for Gemini models (from [Google AI Studio](https://aistudio.google.com))
   - `CHAT_PASSWORD` — access password (⚠️ **change the default on a public deploy**)
   - `ADMIN_PASSWORD` — admin password
6. **Open your Worker URL** and enjoy. 🎉

> **Need keys?** Get a free GLM key at [open.bigmodel.cn](https://open.bigmodel.cn) and a free Gemini key at [aistudio.google.com](https://aistudio.google.com).

---

## ⚙️ Configuration

| Secret / Binding | Purpose | Required |
|------------------|---------|----------|
| KV binding `ZG_KV` | Sessions, memory, gallery, accounts, chatroom, polls | ✅ |
| `ZHIPU_API_KEY` | GLM model access | one of the two keys |
| `GEMINI_API_KEY` | Gemini model access | one of the two keys |
| `CHAT_PASSWORD` | Access gate (default `0404`) | ✅ change on public |
| `ADMIN_PASSWORD` | Admin panel (default `910`) | ✅ change on public |
| Cron `0 18 * * *` | Nightly Dreaming memory consolidation | optional |

---

## 🎮 Hidden Commands & Easter Eggs

Try typing these in the chat:

```
/matrix      Enter the Matrix-style scene
/theme       Cycle / pick a theme (Skylake, Matrix, Midnight, Sakura, Paper)
/font        Switch fonts (incl. an "Anthropic-style" font)
/lang        Toggle 中文 / English
/glass       Toggle liquid-glass visuals
/motion      Toggle animations
/surprise    You'll see
```

Plus the **Konami code** and tapping the logo… we won't spoil everything. 🔍

---

## 🧠 How It Works

- **Single file** — the entire frontend, backend, KV logic and scheduling live in one `worker.js`; no bundler, no dependencies at runtime.
- **Free-first** — only free model tiers and free services (Pollinations, DuckDuckGo) are wired in; paid features are intentionally excluded.
- **Memory pipeline** — conversations → memory events → user memory documents → nightly "Dreaming" consolidation into a global personality document for CDY.
- **Security basics** — salted password hashes (SHA-256), HttpOnly cookies, rate limiting, image validation, per-session ownership checks.

---

## 🛡️ Security Notes

- The default passwords `0404` / `910` are for local/demo use. **Always override them with `CHAT_PASSWORD` / `ADMIN_PASSWORD` Secrets on any public deployment.**
- Rate limiting is on by default (30 req/min/IP, configurable via `RATE_LIMIT_PER_MIN`).

---

## 📜 License

[MIT](LICENSE) © Harry Lyu

---

⭐ **Enjoying EastClub Agent? Give the repo a star — it helps more people discover it!** ⭐
