# Hermes Mobile — AI Agent on Your Old Android Phone

Turn that old Android phone collecting dust in your drawer into a fully functional AI agent server. Runs **Hermes Agent** (by Nous Research) inside Termux + proot — same agent that powers coding assistants, research tools, and automation workflows, now running on repurposed mobile hardware.

No cloud dependency required. No monthly fees. The phone becomes your own personal AI server.

## What You Get

- **AI Chat** — text, voice-to-voice, image generation
- **Internet Access** — web search and content extraction
- **Telegram Gateway** — message it from anywhere via Telegram
- **Cron Jobs** — scheduled reminders, web scrapers, automations
- **Persistent Memory** — remembers you across sessions
- **Git Integration** — clone, edit, and push code repos
- **Coding Tools** — write and debug Python, bash, JS, and more
- **Subagent Delegation** — spawn Claude Code, Codex, or OpenCode workers
- **Skills System** — saves workflows and improves over time
- **Always Online** — runsv service with wake-lock keeps it alive

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

### Step 4: Configure a Provider

Hermes needs an AI model provider to function. The easiest option is **OpenRouter** (free tier available):

1. Go to https://openrouter.ai/keys and create a free account
2. Generate an API key
3. Set it in your Hermes config:

```bash
hermes config set model.provider openrouter
hermes config set model.default openrouter/auto  # auto-picks best model
echo 'OPENROUTER_API_KEY=your_key_here' >> ~/.hermes/.env
```

**Alternative providers:** Anthropic, OpenAI, Google Gemini, MiniMax, DeepSeek, xAI/Grok, and 20+ more. Run `hermes model` to pick.

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

## Image Generation

Hermes can generate images via hosted providers (no GPU needed on the phone):

```bash
hermes config set image_gen.provider minimax
echo 'MINIMAX_API_KEY=your_key_here' >> ~/.hermes/.env
```

Then just ask: *"Generate an image of a cow in a field"*

Supported providers: MiniMax, OpenAI DALL-E, Stability AI, and more.

## Cron Jobs & Reminders

Schedule recurring tasks:

```bash
hermes cron create --schedule "every day at 8am" --prompt "Give me a news briefing"
hermes cron create --schedule "30m" --prompt "Check disk space and alert if low"
```

All deliveries come through your connected gateway (Telegram, etc.).

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
