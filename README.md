# Hermes Mobile — AI Agent on Your Old Android Phone

Turn that old Android phone collecting dust in your drawer into a fully functional AI agent server. Runs **Hermes Agent** (by Nous Research) inside Termux + proot — the same agent that powers coding assistants, research tools, and automation workflows, now running on repurposed mobile hardware.

**Best part: everything below works for free.** You don't need paid subscriptions to run this. If you already have paid API keys (Anthropic, OpenAI, etc.) you can use those too, but the free setup handles 90% of what people need.

## Free vs Paid at a Glance

| Capability | Free option | Paid upgrade |
|------------|-------------|--------------|
| AI chat | DeepSeek v4 Flash Free (no key needed) | Claude, GPT-4o |
| Voice replies | Edge TTS (no key needed) | — |
| Image generation | Stability AI free tier | MiniMax, DALL-E |
| Web search & scraping | Built-in, free | — |
| Telegram messaging | Free with @BotFather | — |
| Cron jobs & reminders | Built-in, free | — |
| Gmail / email | Free with app password | — |
| Everything else | All included, free | — |

## What This Setup Can Do

### Always-On AI Assistant
Chat with your phone like it's your own personal Jarvis. Text it on Telegram, send voice messages, get voice replies back. It never sleeps, never takes a day off.

Talk to it naturally:
- *"Search the web and summarize the latest AI news"* — ✅ Free
- *"Send an email to my boss about the project status"* — ✅ Free
- *"Remind me every Monday at 9am to check the Nixon devices"* — ✅ Free
- *"Scrape this URL and save the product prices"* — ✅ Free
- *"Generate an image of a cyberpunk city"* — 💰 Needs API key (or free tier)

### Multi-Platform Reach
One agent, everywhere you chat — all free to set up:
- **Telegram** — text + voice messages both ways
- **WhatsApp** — send/receive messages and media
- **Discord** — full chat with thread support
- **Signal** — encrypted messaging
- **SMS** — good old text messages
- **iMessage** — blue bubbles (no Mac relay needed)
- **Email** — send/receive via Gmail, Outlook, etc.

### Web Search & Scraping — ✅ Free
Search the web, fetch articles, scrape any public URL — works out of the box, no extra setup. Useful for research, monitoring competitors, tracking prices, or just getting quick answers on the go.

### Gmail & Email Automations — ✅ Free
Send, read, draft, and forward emails through Hermes. Schedule email reports, auto-reply to certain senders, check your inbox on demand. Requires a free app password from Google (no paid account needed).

### Voice-to-Voice — ✅ Free
Send a voice message on Telegram → Hermes transcribes it → thinks → responds with a voice message. Uses **Edge TTS** — free, no API key needed. You can speak in English or Afrikaans and get a reply in the same voice.

### Image Generation — ✅ Free tier available
Generate images by just asking. Use Stability AI free tier (free) or upgrade to MiniMax / DALL-E for higher quality (paid). No GPU needed on the phone since the work happens in the cloud.

### Parallel Subagent Workers — ✅ Free
Spawn **Claude Code, OpenAI Codex, or OpenCode CLI** as background workers while you keep chatting:
- *"Refactor my Python project while I keep asking questions"*
- *"Research topic A and topic B in parallel"*
- *"Write unit tests in the background and tell me when done"*

### Cron Jobs & Reminders — ✅ Free
Schedule anything to run on repeat — every 30 minutes, daily at 8am, every Monday, or a one-shot date. All deliveries come to your Telegram:
- *"Check disk space every hour and alert if low"*
- *"Scrape news headlines daily at 7am"*
- *"Remind me to call Hein every Friday at 3pm"*
- *"Send a weekly usage report to my email"*

### Persistent Memory — ✅ Free
It remembers you across sessions — your name, your projects, your preferences, past conversations. Correct it once and it doesn't make the same mistake twice. No resetting context every time.

### Skills & Self-Improvement — ✅ Free
Every time you solve a complex problem together, Hermes can save that workflow as a **skill**. Over time it builds a library of procedures specific to your needs — it literally gets better the more you use it.

### Full Git & Coding — ✅ Free
Clone repos, write code, debug, push commits. Full Python, bash, and JS support through the terminal.

### MCP Servers (Plug Anything In) — ✅ Free
MCP (Model Context Protocol) lets you plug external tools and APIs directly into Hermes. Databases, business tools, home automation, custom APIs — if it has an API, you can connect it.

### Webhooks — ✅ Free
Trigger Hermes from external events — GitHub push, cron job, IoT sensor, website form. Set it up once and it runs autonomously when events fire.

### Filesystem Access & Checkpoints — ✅ Free
Read, write, search any file on the phone. Plus **checkpoint/rollback** — tell it to make a snapshot before a risky operation and roll back if something breaks.

### Kanban Task Board — ✅ Free
Built-in multi-agent task board. Create tasks, assign them, track progress.

### Session Search — ✅ Free
Every conversation is saved and searchable. Ask *"What did we say about that PWA bug last week?"* and it finds the exact session instantly.

### OpenAI-Compatible Proxy — ✅ Free
Expose Hermes as a local OpenAI API endpoint (`http://phone-ip:port`). Point any tool (Aider, Cline, Continue, etc.) at it.

### Multi-Profile Support — ✅ Free
Run multiple independent Hermes instances on the same phone — one for work, one for personal, one for experiments.

## Requirements

| Item | Minimum | Recommended |
|------|---------|-------------|
| Android phone | Any with Android 11+ | Xiaomi Mi 8 Pro (6GB RAM) or similar |
| Storage | 4 GB free | 8 GB+ free |
| RAM | 3 GB | 4 GB+ |
| Android version | 11 | 12+ |
| Internet | WiFi or mobile data | Stable connection |
| Time to set up | ~30 minutes | — |

## How It Works

```
┌─────────────┐     ┌──────────┐     ┌──────────────────┐
│  Telegram    │◄───►│ Hermes   │◄───►│ DeepSeek v4 Flash │
│  (you)       │     │ Gateway  │     │ Free (OpenCode)   │
└─────────────┘     └────┬─────┘     └──────────────────┘
                         │
                    ┌────▼─────┐
                    │ Termux   │
                    │ + proot  │
                    │ (Ubuntu) │
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │ runsv    │
                    │ services │
                    │ (auto-   │
                    │ restart) │
                    └──────────┘
```

## Quick Setup — 30 Minutes

### Step 1: Install Termux

1. Download **Termux** from [F-Droid](https://f-droid.org/en/packages/com.termux/) (NOT the Play Store version — it's outdated)
2. Download **Termux:Boot** from F-Droid (for auto-start on boot)
3. Open Termux and run:

```bash
pkg update && pkg upgrade -y
pkg install termux-services proot-distro git curl -y
```

### Step 2: Install a Linux Distro (proot)

```bash
proot-distro install ubuntu
proot-distro login ubuntu
```

Inside Ubuntu:

```bash
apt update && apt upgrade -y
apt install python3 python3-pip git curl wget -y
```

### Step 3: Install Hermes

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Restart proot:

```bash
exit
proot-distro login ubuntu
```

### Step 4: Configure an AI Provider

Hermes needs an AI model to function. You have free options and paid upgrades:

| Provider | What You Get | Cost |
|----------|-------------|------|
| **OpenCode Zen** | DeepSeek v4 Flash Free | ✅ Free (no API key) |
| **OpenRouter** | 200+ models, free tier available | ✅ Free tier |
| **Anthropic Claude** | Sonnet, Haiku, Opus | 💰 Paid API key |
| **OpenAI (GPT-4o)** | GPT-4o, GPT-4, GPT-3.5 | 💰 Paid API key |
| **MiniMax** | M3 text + image generation | 💰 Paid (cheap) |
| **Google Gemini** | Gemini 2.0 Flash, Pro | 💰 Paid API key |
| **DeepSeek** | DeepSeek V3, R1 | 💰 Paid API key |
| **xAI Grok** | Grok 2/3 models | 💰 Paid API key |

**Free option — OpenCode Zen (no API key, no credit card):**

```bash
hermes auth add opencode
```

This opens a browser link — log in with GitHub. Done. You're now running **DeepSeek v4 Flash Free**, exactly like this conversation you're reading now.

**Free option — OpenRouter (free tier):**

1. Go to https://openrouter.ai/keys and create a free account
2. Generate an API key
3. Run:

```bash
hermes config set model.provider openrouter
hermes config set model.default openrouter/auto
echo 'OPENROUTER_API_KEY=your_key_here' >> ~/.hermes/.env
```

**If you already have paid API keys** (Anthropic, OpenAI, etc.), set them the same way and run `hermes model` to pick.

### Step 5: Set Up Telegram Gateway

Message your agent from anywhere via Telegram:

1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram
2. Copy the bot token
3. Configure Hermes:

```bash
hermes gateway setup
```

Select Telegram, paste your token, then:

```bash
hermes gateway install
hermes gateway start
```

Now message your bot on Telegram and it replies through Hermes. Voice messages work too — all free.

### Step 6: Enable Voice Replies (Free)

Send a voice message on Telegram and get a voice reply back — no API key needed:

```bash
hermes config set tts.provider edge
hermes config set stt.enabled true
```

Pick a voice:

```bash
hermes config set tts.voice en-US-AriaNeural  # English
hermes config set tts.voice af-ZA-WillemNeural  # Afrikaans
```

Now just send a voice message to your bot.

### Step 7: Keep It Alive

Prevent Android from killing Termux when the screen is off:

```bash
termux-wake-lock
```

Auto-start on boot:

```bash
mkdir -p ~/.termux/boot/
cat > ~/.termux/boot/hermes-start << 'EOF'
#!/data/data/com.termux/files/usr/bin/bash
termux-wake-lock
sv up hermes-gateway
EOF
chmod +x ~/.termux/boot/hermes-start
```

Install Termux:Boot from F-Droid and reboot — Hermes starts automatically.

## Disable Battery Optimization

This is the most common issue. Android will kill Termux to save battery unless you tell it not to.

**For HyperOS (Xiaomi):**
1. Open the **Security** app (shield icon)
2. Tap **Battery** → **App battery saver**
3. Find **Termux** → select **No restrictions**

**For other Android versions:**
- Settings → Apps → Termux → Battery → Unrestricted / No restrictions

Also lock Termux in recent apps (long-press the app icon in the recents menu → Lock).

## What This Costs

| Item | Cost |
|------|------|
| Old Android phone | Already in your drawer |
| Termux (F-Droid) | Free |
| Ubuntu (proot) | Free |
| Hermes Agent | Free, open source |
| AI model (DeepSeek v4 Flash Free) | Free |
| Telegram | Free |
| Voice (Edge TTS) | Free |
| GitHub | Free |
| **Total with free options** | **R0** |
| **With paid API keys** | Whatever you spend on tokens |

No monthly fee required. The phone you stopped using because it was "too slow" is fast enough for this.

## Real-World Examples

**Business owner:**
- *"Check my Gmail for unpaid invoices and list them"* — ✅ Free
- *"Scrape my competitor's website and tell me their prices"* — ✅ Free
- *"Remind me every morning to check yesterday's sales report"* — ✅ Free
- *"Send a bulk email to my customers about the new promotion"* — ✅ Free

**Developer:**
- *"Clone my GitHub repo, fix the bug in app.py, and push the fix"* — ✅ Free
- *"Research library X vs library Y and recommend which to use"* — ✅ Free
- *"Write a Python script that monitors a website for changes"* — ✅ Free
- *"Spawn Codex to write unit tests while you help me debug this"* — ✅ Free

**Daily life:**
- *"Search for flights to Cape Town next weekend"* — ✅ Free
- *"Summarize today's top tech news"* — ✅ Free
- *"Set a reminder to buy milk on my way home"* — ✅ Free
- *"What's the weather like tomorrow?"* — ✅ Free

**Home automation:**
- *"Send me a notification if my server goes down"* — ✅ Free
- *"Check the disk space every hour and alert me below 20%"* — ✅ Free
- *"Scrape this cryptocurrency price every 30 minutes"* — ✅ Free

## What NOT to Do

- ❌ Don't paste API keys or passwords into any chat — use `.env` files
- ❌ Don't install Hermes from the Play Store version of Termux (use F-Droid)
- ❌ Don't expect desktop-level performance — this is repurposed mobile hardware
- ❌ Don't run heavy local models — the phone doesn't have a GPU for inference

## Project Layout

```
~/.hermes/
├── config.yaml         # Main configuration
├── .env                # API keys and secrets
├── skills/             # Installed skills (procedural memory)
├── sessions/           # Chat history
├── logs/               # Gateway and error logs
└── state.db            # Session store
```

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Agent | [Hermes](https://hermes-agent.nousresearch.com) by Nous Research |
| Runtime | Termux + proot (Ubuntu 26.04) |
| Service manager | runsv (termux-services) |
| AI providers | OpenCode Zen (free), OpenRouter (free tier), or paid upgrades |
| Messaging | Telegram gateway (auto-restart) |
| Voice | Edge TTS (free) |
| Wake lock | termux-wake-lock |
| Python | 3.14 via uv |

## Why This Works

Old Android phones have perfectly capable CPUs, plenty of RAM (3-6GB), and built-in batteries with network connectivity. The only thing they lack is a GPU for local model inference — but with Hermes, the heavy AI runs on free cloud models while the phone handles the agent logic, tool execution, and messaging. It's a thin client that behaves like a fat client.

The phone you stopped using because it was "too slow" is fast enough for this.

## License

MIT — do what you want with it.

---

*Built and tested on a Xiaomi Redmi A3x (2.7GB RAM) and targeting a Xiaomi Mi 8 Pro (6GB RAM). Your mileage may vary, but the principles are the same across most Android devices.*
