# Hermes Mobile — AI Agent on Your Old Android Phone

Turn that old Android phone collecting dust in your drawer into a fully functional AI agent server. Runs **Hermes Agent** (by Nous Research) inside Termux + proot — the same agent that powers coding assistants, research tools, and automation workflows, now running on repurposed mobile hardware.

No cloud GPU needed. No monthly fees. The phone becomes your own personal AI server that you message from anywhere.

## What This Setup Can Do

### AI Chat & Image Gen
Talk to it like any AI, send voice messages, generate images — all through Telegram or directly in Termux.

### Web Search & Scraping
Ask it to search the web, fetch articles, scrape websites, or research topics. Works with any public URL or search query.

### Gmail & Email Automations
Send emails, read your inbox, draft replies, automate email workflows. Can be set up with Gmail, Outlook, or any IMAP/SMTP provider.

### Cron Jobs & Reminders
Schedule recurring tasks — "remind me every day at 8am", "check a website every hour", "send a weekly report". Deliveries come straight to your Telegram.

### Coding & Git
Clone repos, write and debug code (Python, bash, JS, etc.), push commits, run scripts. Full git integration.

### Subagent Delegation
Spawn Claude Code, OpenAI Codex, or OpenCode CLI as subagents for parallel work — while you keep chatting.

### Skills & Memory
Saves workflows as reusable skills. Remembers preferences and context across sessions. Gets better the more you use it.

### Always Online
Runs as a service with auto-restart and wake-lock. Message it anytime — even when the screen is off.

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

### Step 6: Keep It Alive

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
