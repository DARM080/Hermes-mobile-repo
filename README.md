# Hermes Mobile — You Don't Need Money to Run an AI Agent

Got an old Android phone in a drawer? You can turn it into a personal AI server that messages you on Telegram — **and spend exactly R0.**

This guide shows you what works for free and how to set it up. No subscriptions, no API bills, no cloud GPU needed.

## What You Get (All Free)

| Capability | Free? | How |
|------------|-------|-----|
| AI chat | ✅ Free | DeepSeek v4 Flash Free via OpenCode Zen — no API key |
| Voice replies | ✅ Free | Edge TTS — no API key |
| Web search & scraping | ✅ Free | Built-in, works out of the box |
| Telegram messaging | ✅ Free | Create a bot with @BotFather |
| Cron jobs & reminders | ✅ Free | Runs locally, delivers to Telegram |
| Persistent memory | ✅ Free | Remembers you across sessions |
| Skills system | ✅ Free | Saves workflows, self-improves |
| Git & coding | ✅ Free | Full Python/bash/JS, git, GitHub |
| Subagent workers | ✅ Free | Spawn Claude Code, Codex, OpenCode workers |
| Image generation | ✅ Free | Stability AI or other free-tier providers |
| Multi-platform chat | ✅ Free | Telegram, WhatsApp, Discord, Signal, SMS, Email |
| Session search | ✅ Free | Find past conversations instantly |
| Profiles | ✅ Free | Run multiple independent instances |
| MCP servers | ✅ Free | Plug in any API |
| Webhooks | ✅ Free | Trigger from GitHub, IoT, forms |
| OpenAI proxy | ✅ Free | Expose as local API endpoint |

**Everything in the table above works without spending a cent.**

## What Costs Money (Optional Upgrades)

You don't need these, but if you want better models:

- **Claude Opus/Sonnet** — needs Anthropic API key (paid)
- **GPT-4o** — needs OpenAI API key (paid)
- **MiniMax** — better image gen, pay-as-you-go (cheap)
- **SMS gateway** — Twilio if you want SMS support

The free setup handles 90% of what people use AI for.

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

## Quick Setup — 30 Minutes, Free

### Step 1: Install Termux (Free)

1. Download **Termux** from [F-Droid](https://f-droid.org/en/packages/com.termux/) — the Play Store version is outdated, use F-Droid
2. Also download **Termux:Boot** from F-Droid (for auto-start)
3. Open Termux and install dependencies:

```bash
pkg update && pkg upgrade -y
pkg install termux-services proot-distro git curl -y
```

### Step 2: Install Ubuntu (Free)

```bash
proot-distro install ubuntu
proot-distro login ubuntu
```

Inside Ubuntu:

```bash
apt update && apt upgrade -y
apt install python3 python3-pip git curl wget -y
```

### Step 3: Install Hermes (Free, Open Source)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Restart proot:

```bash
exit
proot-distro login ubuntu
```

### Step 4: Set Up the Free AI Model

**Option A — OpenCode Zen (truly free, no API key):**

Run the OAuth setup:

```bash
hermes auth add opencode
```

This opens a browser link — log in with your GitHub account. You now have **DeepSeek v4 Flash Free** running, exactly like this conversation you're reading right now.

**Option B — OpenRouter (free tier, needs an account):**

1. Go to https://openrouter.ai/keys and make a free account
2. Generate an API key
3. Run:

```bash
hermes config set model.provider openrouter
hermes config set model.default openrouter/auto
echo 'OPENROUTER_API_KEY=your_key_here' >> ~/.hermes/.env
```

OpenRouter's free tier gives you access to Mistral, Llama, and other models. Rate-limited but genuinely free.

> **Either option works fine. The rest of this guide assumes you went with OpenCode Zen (no money needed).**

### Step 5: Set Up Telegram (Free)

1. Open Telegram, search for [@BotFather](https://t.me/BotFather)
2. Send `/newbot` and pick a name — you get a bot token
3. Configure Hermes:

```bash
hermes gateway setup
```

Select Telegram, paste your token. Then start it:

```bash
hermes gateway install
hermes gateway start
```

That's it. Message your bot and Hermes replies.

### Step 6: Enable Voice Replies (Free)

Send a voice message and get a voice reply back — no API key needed:

```bash
hermes config set tts.provider edge
hermes config set stt.enabled true
```

Pick a voice:

```bash
hermes config set tts.voice en-US-AriaNeural    # English
hermes config set tts.voice af-ZA-WillemNeural  # Afrikaans
```

Now just send a voice message to your Telegram bot.

### Step 7: Keep It Alive

Stop Android from killing Termux:

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

Install Termux:Boot from F-Droid if not already, and reboot. Hermes starts automatically.

## Disable Battery Optimization

This is the most common issue — Android kills Termux to save power. Fix it:

**HyperOS (Xiaomi):** Security app → Battery → App battery saver → Termux → No restrictions

**Other Android:** Settings → Apps → Termux → Battery → Unrestricted

Also lock Termux in recent apps (long-press the recents icon → Lock).

## What You Can Do With It (All Free)

### Web Search & Scraping
Search the web or fetch any URL. Works out of the box.

*"Search for the latest news on AI"*
*"Go to wikipedia.org and summarize Raspberry Pi"*
*"Scrape this URL and tell me the prices"*

### Gmail & Email Automations
Send and receive emails. You need an app password from Google (free).

```bash
hermes config set email.enabled true
hermes config set email.smtp_host smtp.gmail.com
hermes config set email.smtp_port 587
hermes config set email.smtp_user your.email@gmail.com
hermes config set email.imap_host imap.gmail.com
hermes config set email.imap_port 993
echo 'EMAIL_PASSWORD=your_app_password_here' >> ~/.hermes/.env
```

Then: *"Send an email to john@example.com about the meeting"*

### Image Generation (Free Tier)
Generate images using free-tier providers or paid for better quality.

```bash
hermes config set image_gen.provider minimax
echo 'MINIMAX_API_KEY=your_key_here' >> ~/.hermes/.env
```

Then: *"Generate an image of a cow in a field"*

### Cron Jobs & Reminders
Schedule anything to run on repeat. Deliveries go to your Telegram.

```bash
hermes cron create --schedule "every day at 7am" --prompt "News briefing"
hermes cron create --schedule "30m" --prompt "Check disk space"
hermes cron create --schedule "every monday 9am" --prompt "Weekly summary"
```

Or just ask naturally: *"Remind me in 2 hours to call Hein"*

### Manage jobs:
```bash
hermes cron list          # View all
hermes cron remove ID     # Delete
hermes cron run ID        # Run now
```

### Coding & Git
Clone repos, write code, push commits. Full Python, bash, JS.

*"Clone my repo, fix the bug in app.py, and push"*
*"Write a Python script to monitor disk space"*
*"Research library X vs library Y"*

### Subagent Workers
Spawn Claude Code, Codex, or OpenCode in the background while you keep chatting.

*"Spawn Codex to write unit tests while you help me debug"*
*"Research topic A and topic B in parallel"*

### Persistent Memory
Correct it once and it remembers. No resetting context.

### Skills
Saves workflows that work for you. Gets better over time.

## Real-World Examples (Tested on a Redmi A3x)

**Business owner:**
- "Check my Gmail for unpaid invoices"
- "Scrape my competitor's website for prices"
- "Remind me every morning to check sales"

**Developer:**
- "Clone my GitHub repo and fix the bug"
- "Write a Python script to watch a website for changes"
- "Spawn Codex to write tests while you help me debug"

**Daily:**
- "Search for flights to Cape Town"
- "Summarize today's top tech news"
- "Generate a logo for my side project"
- "What's the weather tomorrow?"

**Automation:**
- "Alert me if disk space drops below 20%"
- "Scrape this crypto price every 30 minutes"
- "Send a notification if my server goes down"

## What NOT to Do

- ❌ Don't paste API keys into chat — use `.env` files
- ❌ Don't install Termux from the Play Store (use F-Droid)
- ❌ Don't expect desktop speed — it's an old phone
- ❌ Don't try to run local LLMs — no GPU for that

## Project Layout

```
~/.hermes/
├── config.yaml         # Settings (free)
├── .env                # API keys (optional)
├── skills/             # Saved workflows (free)
├── sessions/           # Chat history (free)
├── logs/               # Error logs (free)
└── state.db            # Session store (free)
```

## How Much Does This Actually Cost?

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
| Internet | Whatever you already pay |
| **Total** | **R0** |

No monthly fees. No subscription. Just the phone you already own.

## Why This Works

Old Android phones have good CPUs, 3-6GB RAM, and built-in batteries. The only thing they lack is a GPU — but with Hermes, the heavy AI runs on free cloud models while the phone handles the agent logic and messaging.

The phone you stopped using because it was "too slow" is fast enough for this.

---

*Built and tested on a Xiaomi Redmi A3x (2.7GB RAM) running OpenCode Zen (DeepSeek v4 Flash Free) — the exact free setup described above. If the cheapest Redmi works, yours will too.*
