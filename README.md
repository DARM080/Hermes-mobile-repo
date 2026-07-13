# Hermes Mobile — AI Agent on Your Old Android Phone

Turn that old Android phone collecting dust in your drawer into a fully functional AI agent server. Runs **Hermes Agent** (by Nous Research) inside Termux + proot — the same agent that powers coding assistants, research tools, and automation workflows, now running on repurposed mobile hardware.

**Best part: everything below works for free.** You don't need paid subscriptions to run this. If you already have paid API keys (Anthropic, OpenAI, etc.) you can use those too, but the free setup handles 90% of what people need.

## Free vs Paid at a Glance

| Capability | Free option | Paid upgrade |
|------------|-------------|--------------|
| AI chat | DeepSeek v4 Flash Free (no key needed) | Claude, GPT-4o |
| Voice replies | Edge TTS (no key needed) | — |
| Image generation | ❌ Needs API key (MiniMax, etc.) | MiniMax, DALL-E, Stability AI |
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

### Image Generation — 💰 Needs API key
Generate images by just asking. Works with MiniMax, OpenAI DALL-E, or Stability AI — all need an API key. No GPU needed on the phone since the work happens in the cloud.

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

Before you start: enable **Install from unknown apps** for your file browser (or F-Droid) so you can install APKs.

### Step 1: Install Termux

1. If you don't have F-Droid yet, install it from **https://f-droid.org** (tap the download button, install the APK)
2. Open F-Droid, tap the search icon, search for **Termux**
3. Tap **Termux** → **Install** (not the Play Store version — it's outdated and broken)
4. Also search for and install **Termux:Boot** (needed for auto-start later)
5. Open Termux and run these two commands:

```bash
pkg update && pkg upgrade -y
pkg install termux-services proot-distro git curl -y
```

Wait for both to finish — the second one installs the tools we need.

### Step 2: Install Ubuntu (the Linux system)

Run these two commands one at a time:

```bash
proot-distro install ubuntu
proot-distro login ubuntu
```

You'll see the command prompt change — you're now inside Ubuntu. Update it:

```bash
apt update && apt upgrade -y
apt install python3 python3-pip git curl wget -y
```

> **What is proot?** It lets you run Ubuntu apps without rooting your phone. Think of it as a "fake root" that gives Hermes a normal Linux environment to work in.

### Step 3: Install Hermes

Now install Hermes itself:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

This downloads and runs the installer. It installs `uv` (a Python tool), creates a virtual environment, and sets up the `hermes` command. Takes about a minute.

Restart the Ubuntu session so the `hermes` command becomes available:

```bash
exit
proot-distro login ubuntu
```

You should now be able to type `hermes --version` and get a version number back. If you get "command not found", something went wrong — re-run the curl command.

### Step 4: Set Up the Free AI Model

Hermes needs an AI brain to work. The easiest free option is **OpenCode Zen** — no API key, no credit card, nothing.

Run this command:

```bash
hermes auth add opencode
```

This prints a long URL that starts with `https://github.com/login/oauth/authorize?`. 

**On your phone, do this:**
1. Long-press the URL in Termux and copy it (or tap and drag to select)
2. Open your phone browser and paste the URL
3. Log into GitHub if asked (your normal GitHub account)
4. GitHub shows an "Authorize" button — tap it
5. You'll see a page saying "Redirecting to localhost..." — the page will fail to load, **that's normal**
6. Copy the full URL from the browser's address bar (it starts with `http://localhost?code=...`)
7. Go back to Termux, paste it and press Enter

You're now authenticated. The prompt will reappear. Type `hermes model` to confirm — it should show **DeepSeek v4 Flash Free** as active.

> **If the OAuth URL thing doesn't work**, use OpenRouter instead — it's also free, just needs a quick account:
> 1. Go to **https://openrouter.ai/keys** in your browser, sign up free, generate an API key
> 2. In Termux run:
> ```bash
> hermes config set model.provider openrouter
> hermes config set model.default openrouter/auto
> echo 'OPENROUTER_API_KEY=paste_your_key_here' >> ~/.hermes/.env
> ```

Either way, you now have a working AI agent in Termux. You can test it by just typing `hermes` and chatting right there. But the real magic is via Telegram — keep going.

### Step 5: Set Up Telegram Gateway

This lets you message your agent from anywhere — even when Termux isn't open.

First, create a Telegram bot:
1. Open Telegram, search for **@BotFather**
2. Send `/newbot`
3. It asks for a name — send any name, e.g. `My Personal AI`
4. It asks for a username — must end in `bot`, e.g. `mypersonal_ai_bot`
5. BotFather replies with a **token** — a long string like `7234567890:AAHdqTcvCH1vGWJxfSeOfS...`
6. Copy that token, you need it next

Now connect it to Hermes:

```bash
hermes gateway setup
```

This starts an interactive setup. It will ask:
- **Which platform?** — type `telegram` and Enter
- **Bot token?** — paste the token BotFather gave you and Enter
- **Other questions** — just press Enter for defaults

When it's done, start the gateway:

```bash
hermes gateway install
hermes gateway start
```

**Test it:** Open Telegram, find your bot, and send it a message like *"Hello, are you working?"* You should get a reply within a few seconds. If not, wait 10 seconds and try again (gateway takes a moment to connect).

### Step 6: Enable Voice Replies (Free)

This lets you send voice messages and get voice replies back — all free, no API key needed:

```bash
hermes config set tts.provider edge
hermes config set stt.enabled true
```

Pick a voice for the replies:

```bash
hermes config set tts.voice en-US-AriaNeural    # English (US woman)
hermes config set tts.voice af-ZA-WillemNeural  # Afrikaans (SA man)
```

Now send a voice message to your Telegram bot — it will reply in the same voice.

### Step 7: Keep It Alive

Prevent Android from killing Termux:

```bash
termux-wake-lock
```

This locks the wake lock so Termux stays running when the screen is off. You only need to run this once — it persists until the phone reboots.

**Auto-start on boot** (so you don't have to manually start anything):

```bash
mkdir -p ~/.termux/boot/
nano ~/.termux/boot/hermes-start
```

This opens a text editor. Type (or paste) these lines:

```
#!/data/data/com.termux/files/usr/bin/bash
termux-wake-lock
sv up hermes-gateway
```

Press **Ctrl+X** then **Y** then **Enter** to save and exit.

Make the script executable:

```bash
chmod +x ~/.termux/boot/hermes-start
```

Make sure **Termux:Boot** is installed from F-Droid. Reboot your phone. After boot, open Termux:Boot at least once to grant it permission (it's just a one-time setup — after that it runs silently). From then on, Hermes starts automatically every time the phone boots.

### Step 8: Test Everything Works

1. Reboot your phone
2. Wait 30 seconds
3. Open Telegram and message your bot: *"Search the web for today's news"*
4. You should get a reply with news summaries

If you get a reply, everything is working — congratulations, you have a free AI server in your pocket.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "hermes command not found" | Run `exit` then `proot-distro login ubuntu` to restart the session |
| Bot doesn't reply on Telegram | Run `hermes gateway status` to check if it's running. If not, `hermes gateway start` |
| OAuth URL didn't work | Use OpenRouter instead — see Step 4 above |
| Termux keeps stopping | You skipped battery optimization — see the section below |
| Voice messages not working | Run `hermes config set tts.provider edge` and `/restart` in Telegram |
| "Can't install from Play Store" | You're on the wrong version. Uninstall Termux, install F-Droid from f-droid.org, get Termux from there |
| Something else stuck | Delete `~/.hermes/config.yaml` and run `hermes setup` to start fresh

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
