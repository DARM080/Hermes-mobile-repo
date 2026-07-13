# Hermes Mobile — AI Agent on Your Old Android Phone

Turn that old Android phone collecting dust in your drawer into a fully functional AI agent server. Runs **Hermes Agent** (by Nous Research) inside Termux + proot — the same agent that powers coding assistants, research tools, and automation workflows, now running on repurposed mobile hardware.

No cloud GPU needed. No monthly fees. The phone becomes your own personal AI server that you message from anywhere.

## What This Setup Can Do

### Always-On AI Assistant
Chat with your phone like it's your own personal Jarvis. Text it on Telegram, send voice messages, get voice replies back. It never sleeps, never takes a day off.

**Talk to it naturally:**
- *"Search the web and summarize the latest AI news"*
- *"Send an email to my boss about the project status"*
- *"Remind me every Monday at 9am to check the Nixon devices"*
- *"Scrape this URL and save the product prices"*
- *"Generate an image of a cyberpunk city"*

### Multi-Platform Reach
One agent, everywhere you chat:
- **Telegram** — text + voice messages both ways
- **WhatsApp** — send/receive messages and media
- **Discord** — full chat with thread support
- **Signal** — encrypted messaging
- **SMS** — good old text messages
- **iMessage** — blue bubbles (no Mac relay needed)
- **Email** — send/receive via Gmail, Outlook, etc.

### Web Search & Scraping
Search the web, fetch articles, scrape any public URL — works out of the box, no extra setup. Useful for research, monitoring competitors, tracking prices, or just getting quick answers on the go.

### Gmail & Email Automations
Send, read, draft, and forward emails through Hermes. Schedule email reports, auto-reply to certain senders, check your inbox on demand. Requires a free app password from Google.

### Voice-to-Voice (Free)
Send a voice message on Telegram → Hermes transcribes it → thinks → responds with a voice message. Uses Edge TTS (free, no API key). You can speak in English or Afrikaans and get a reply in the same voice.

### Image Generation
Generate images by just asking. Uses MiniMax, OpenAI DALL-E, or Stability AI — no GPU needed on the phone since the work happens in the cloud.

### Parallel Subagent Workers
This is a big one — Hermes can spawn **Claude Code, OpenAI Codex, or OpenCode CLI** as background workers while you keep chatting. You can ask it to:
- *"Refactor my Python project while I keep asking questions"*
- *"Research topic A and topic B in parallel"*
- *"Write unit tests in the background and tell me when done"*

### Cron Jobs & Reminders
Schedule anything to run on repeat — every 30 minutes, daily at 8am, every Monday, or a one-shot date. All deliveries come to your Telegram:
- *"Check disk space every hour and alert if low"*
- *"Scrape news headlines daily at 7am"*
- *"Remind me to call Hein every Friday at 3pm"*
- *"Send a weekly usage report to my email"*

### Persistent Memory
It remembers you across sessions — your name, your projects, your preferences, past conversations. Correct it once and it doesn't make the same mistake twice. No resetting context every time.

### Skills & Self-Improvement
Every time you solve a complex problem together, Hermes can save that workflow as a **skill**. Over time it builds a library of procedures specific to your needs — it literally gets better the more you use it.

### Full Git & Coding
Clone repos, write code, debug, push commits. Full Python, bash, and JS support through the terminal. You can even use it to write and update its own guide.

### MCP Servers (Plug Anything In)
MCP (Model Context Protocol) lets you plug external tools and APIs directly into Hermes. Databases, business tools, home automation, custom APIs — if it has an API, you can connect it.

### Webhooks
Trigger Hermes from external events — GitHub push, cron job, IoT sensor, website form. Set it up once and it runs autonomously when events fire.

### Filesystem Access & Checkpoints
Read, write, search any file on the phone. Plus **checkpoint/rollback** — tell it to make a snapshot before a risky operation and roll back if something breaks.

### Kanban Task Board
Built-in multi-agent task board. Create tasks, assign them, track progress. Useful for managing projects alongside your AI.

### Session Search
Every conversation is saved and searchable. Ask *"What did we say about that PWA bug last week?"* and it finds the exact session instantly.

### OpenAI-Compatible Proxy
Expose Hermes as a local OpenAI API endpoint (`http://phone-ip:port`). Point any tool (Aider, Cline, Continue, etc.) at it and use your provider's models through the phone.

### Multi-Profile Support
Run multiple independent Hermes instances on the same phone — one for work, one for personal, one for experiments. Each has its own config, skills, memory, and sessions.

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
┌─────────────┐     ┌──────────┐     ┌──────────────┐
│  Telegram    │◄───►│ Hermes   │◄───►│ OpenRouter /  │
│  (you)       │     │ Gateway  │     │ OpenCode Zen  │
└─────────────┘     └────┬─────┘     │ MiniMax / etc │
                         │           └──────────────┘
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

## Quick Setup

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

You're now inside an Ubuntu environment. Update it:

```bash
apt update && apt upgrade -y
apt install python3 python3-pip git curl wget -y
```

### Step 3: Install Hermes

Inside the proot Ubuntu environment:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

This installs `uv`, creates a Python venv, and sets up the `hermes` command.

Restart the proot session to activate:

```bash
exit
proot-distro login ubuntu
```

### Step 4: Configure an AI Provider

Hermes needs an AI model to function. It supports 20+ providers — here are the most common:

| Provider | What You Get | Setup |
|----------|-------------|-------|
| **OpenRouter** | Access to 200+ models (free tier available) | Set `OPENROUTER_API_KEY` |
| **OpenCode Zen** | DeepSeek v4 Flash Free, plus spawn OpenCode CLI | OAuth via `hermes auth add opencode` |
| **Anthropic Claude** | Claude Sonnet, Haiku, Opus | Set `ANTHROPIC_API_KEY` |
| **OpenAI (GPT-4o, etc.)** | GPT-4o, GPT-4, GPT-3.5 | Set `OPENAI_API_KEY` |
| **MiniMax** | M3 text + image generation | Set `MINIMAX_API_KEY` |
| **Google Gemini** | Gemini 2.0 Flash, Pro | Set `GOOGLE_API_KEY` |
| **xAI Grok** | Grok 2/3 models | Set `XAI_API_KEY` |
| **DeepSeek** | DeepSeek V3, R1 | Set `DEEPSEEK_API_KEY` |

**Quick setup with OpenRouter (easiest, free tier):**

1. Go to https://openrouter.ai/keys and create a free account
2. Generate an API key
3. Run:

```bash
hermes config set model.provider openrouter
hermes config set model.default openrouter/auto
echo 'OPENROUTER_API_KEY=your_key_here' >> ~/.hermes/.env
```

**If you already have an OpenAI or Anthropic account,** just set the relevant key and run `hermes model` to pick your model.

**What about ChatGPT?** If you have a ChatGPT Pro/Plus subscription, you can authenticate via OpenCode OAuth (`hermes auth add openai-codex`) which gives you access to OpenAI models through the Codex provider. If you have an OpenAI API key, set `OPENAI_API_KEY` directly.

> No API key? Use OpenRouter — their free tier works immediately with rate-limited models.

### Step 5: Set Up Telegram Gateway (Optional)

Talk to your agent from anywhere via Telegram:

1. Create a bot via [@BotFather](https://t.me/BotFather) on Telegram
2. Copy the bot token
3. Configure the gateway:

```bash
hermes gateway setup
```

Select Telegram, paste your bot token, and start:

```bash
hermes gateway install
hermes gateway start
```

Now message your bot on Telegram and it replies through Hermes. Voice messages work too.

### Step 6: Enable Voice Replies (Free)

Send a voice message on Telegram and get a voice reply back — no API key needed:

```bash
hermes config set tts.provider edge
hermes config set stt.enabled true
```

Edge TTS is free and works offline. You can change the voice:

```bash
hermes config set tts.voice en-US-AriaNeural  # English (US)
hermes config set tts.voice af-ZA-WillemNeural  # Afrikaans
```

Now just send a voice message to your bot — Hermes transcribes it, thinks, and replies in voice.

### Step 7: Keep It Alive

Prevent Android from killing Termux when the screen is off:

```bash
termux-wake-lock
```

Set up a runsv service so Hermes auto-starts on boot:

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

## Web Search & Scraping

Hermes can search the web and scrape any public URL. This works out of the box — no extra setup.

**Ask naturally:**
- *"Search for latest news on repurposed Android phones"*
- *"Go to wikipedia.org and summarize the page on Raspberry Pi"*
- *"Scrape this URL and tell me the prices: https://example.com/products"*

Hermes fetches the page content, extracts the text, and responds with what you asked for. It can scrape articles, documentation, product listings, and more.

**Technical notes:**
- Works with any public URL (http/https)
- Handles redirects, mobile/desktop user-agent
- If a site blocks scraping, Hermes will tell you
- Rate limit respect — not designed for bulk scraping

## Gmail & Email Automations

Send and receive emails right from your Hermes agent. You'll need an app password from your email provider.

### Setup with Gmail

1. Enable 2-Factor Authentication on your Google account
2. Generate an app password: https://myaccount.google.com/apppasswords
3. Set up in Hermes:

```bash
hermes config set email.enabled true
hermes config set email.smtp_host smtp.gmail.com
hermes config set email.smtp_port 587
hermes config set email.smtp_user your.email@gmail.com
hermes config set email.imap_host imap.gmail.com
hermes config set email.imap_port 993
echo 'EMAIL_PASSWORD=your_app_password_here' >> ~/.hermes/.env
```

### What you can do

- *"Send an email to john@example.com with subject 'Meeting' and body 'See you at 3pm'"*
- *"Check my inbox for any emails from my boss today"*
- *"Draft a reply to the latest email about the project deadline"*
- *"Forward the email about invoices to accounts@company.com"*

Works with Gmail, Outlook, Yahoo, or any IMAP/SMTP provider. Restart Hermes after setting up email.

## Image Generation

Hermes can generate images via hosted providers — no GPU needed on the phone:

```bash
hermes config set image_gen.provider minimax
echo 'MINIMAX_API_KEY=your_key_here' >> ~/.hermes/.env
```

Then just ask: *"Generate an image of a cow in a field"*

Supported providers: MiniMax, OpenAI DALL-E, Stability AI, and others.

## Cron Jobs & Reminders

Schedule recurring tasks that run automatically and deliver results to your Telegram (or other connected platform).

### Schedule Formats

| Format | Example | Meaning |
|--------|---------|---------|
| Duration | `30m` | Every 30 minutes |
| Duration | `2h` | Every 2 hours |
| Natural | `every day at 8am` | Daily at 8am |
| Natural | `every monday 9am` | Weekly on Monday |
| Cron | `0 9 * * *` | Daily at 9am (standard cron) |
| One-shot | `2026-12-25T09:00:00` | Run once on Christmas |

### Create a Job

```bash
hermes cron create --schedule "every day at 7am" --prompt "Give me a news briefing with today's top stories"
hermes cron create --schedule "30m" --prompt "Check disk space and alert if below 10%"
hermes cron create --schedule "every monday 9am" --prompt "Write a weekly summary of what I worked on"
```

### Reminders

Just ask during a chat:
- *"Remind me in 2 hours to call Hein"*
- *"Set a reminder for tomorrow at 9am to check the Nixon devices"*
- *"Remind me every Friday to submit my timesheet"*

All reminders and cron deliveries come through your connected gateway (Telegram, etc.) so they reach your phone even when you're not in Termux.

### Manage Jobs

```bash
hermes cron list          # View all jobs
hermes cron pause ID      # Pause a job
hermes cron resume ID     # Resume a paused job
hermes cron run ID        # Trigger a job immediately
hermes cron remove ID     # Delete a job
```

## Tips & Tricks

- **Start small** — test everything on your old phone before moving to your daily driver
- **Keep it plugged in** — running an AI server drains battery faster
- **Use stable WiFi** — the gateway needs a consistent connection
- **Monitor storage** — logs and generated files can pile up over time
- **Regular updates** — `hermes update` keeps the agent current
- **Reset if stuck** — `/reset` in chat or `hermes` starts a fresh session

## Real-World Examples

Here are actual things you can do with this setup — not hypotheticals, tested on a Redmi A3x:

**Business owner:**
- *"Check my Gmail for unpaid invoices and list them"*
- *"Send a bulk email to my customers about the new promotion"*
- *"Scrape my competitor's website and tell me their prices"*
- *"Remind me every morning to check yesterday's sales report"*

**Developer:**
- *"Clone my GitHub repo, fix the bug in app.py, and push the fix"*
- *"Research library X vs library Y and recommend which to use"*
- *"Write a Python script that monitors a website for changes"*
- *"Spawn Codex to write unit tests while you help me debug this"*

**Daily life:**
- *"Search for flights to Cape Town next weekend"*
- *"Summarize today's top tech news"*
- *"Set a reminder to buy milk on my way home"*
- *"Generate a logo for my side project"*
- *"What's the weather like tomorrow?"*

**Home automation:**
- *"Send me a notification if my server goes down"*
- *"Check the disk space every hour and alert me below 20%"*
- *"Scrape this cryptocurrency price every 30 minutes"*

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
| AI providers | OpenRouter, MiniMax, Anthropic, OpenAI, etc. |
| Messaging | Telegram gateway (auto-restart) |
| Wake lock | termux-wake-lock |
| Python | 3.14 via uv |

## Why This Works

Old Android phones have perfectly capable CPUs, plenty of RAM (3-6GB), and built-in batteries with network connectivity. The only thing they lack is a GPU for local model inference — but with Hermes, the heavy AI runs on cloud providers while the phone handles the agent logic, tool execution, and messaging. It's a thin client that behaves like a fat client.

## License

MIT — do what you want with it.

---

*Built and tested on a Xiaomi Redmi A3x (2.7GB RAM) and targeting a Xiaomi Mi 8 Pro (6GB RAM). Your mileage may vary, but the principles are the same across most Android devices.*
