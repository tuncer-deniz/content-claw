# Content Claw v1.0.0 🦞

An [OpenClaw](https://openclaw.ai) skill that saves you hours of content consumption every week.

Send your agent a link — YouTube video, article, tweet thread, podcast — with the word **"review"** and it will:

1. **Transcribe or extract** the full content (sub-agent powered)
2. **Cross-reference** it against your goals, existing knowledge, and frameworks
3. **Deliver a verdict**: 🎬 Watch, 📖 Read, 👀 Skim, or ⏭️ Skip
4. **Extract key insights** either way — so you get the value regardless
5. **File it** — winners go to `caught/`, the rest get `released/` with auto-purge

## Why?

Most content is derivative. The signal-to-noise ratio is brutal. Your time is the scarcest resource.

This skill lets your AI agent be the first filter. It extracts the value so you can stay focused on what actually moves the needle.

## Trigger Phrases

Any URL combined with:
- `review` / `analyze` / `evaluate`
- `worth my time?` / `should I watch?` / `should I read?`
- `content claw`
- Just a URL followed by `?`

## Example Output

```
You: https://youtube.com/watch?v=xyz review this

Agent: ⏭️ Skip

Already have this framework. But here's the one new insight:

📌 Actions:
- Add the "3-step delegation" template to your SOPs

💡 Insights:
- Author reframes delegation as "investment" not "handoff" — useful mental model

Filed → released/skip/xyz-delegation-framework.md (auto-purge in 14 days)
```

## Verdict Scale

| Verdict | Meaning |
|---|---|
| ⏭️ **Skip** | Already know this or not relevant. Key notes captured. |
| 👀 **Skim** | Some useful bits, not worth full attention. Highlights extracted. |
| 📖 **Read** | Worth reading the summary. Key frameworks extracted. |
| 🎬 **Watch** | Visual/demo content that loses value in text. Worth the time. |

## Folder Structure

```
docs/content-claw/
├── caught/              ← 📖 Read and 🎬 Watch verdicts. Permanent keepers.
└── released/
    ├── skim/            ← 👀 Monthly batch review, then purged.
    └── skip/            ← ⏭️ Auto-purge after 14 days. Zero maintenance.
```

**Caught** = content worth your time. Your curated knowledge shelf.

**Released** = value extracted, original not worth keeping. Skips auto-delete after 14 days. Skims get a monthly one-line summary — reply with numbers to promote to `caught/` or "clear all."

## Features

- **🧹 URL sanitization** — Strips tracking params (utm_*, fbclid, Twitter `s`/`t`, YouTube `si`) before fetching
- **📎 Rich source context** — Every review includes who shared it, when, and what you were working on (for future memory recall)
- **🗑️ Auto-purge** — Skip reviews auto-delete after 14 days. Skim reviews get monthly batch review.
- **Duration-aware** — Warns before transcribing 60+ minute videos, offers alternatives
- **Output verification** — Validates sub-agent actually extracted content (not just planned to)
- **Actions vs Insights** — Separates actionable items (📌) from interesting-but-passive info (💡)
- **Duplicate detection** — Checks if a URL was already reviewed before running extraction
- **Multi-format** — YouTube, articles, tweets, podcasts, Substack, Medium

## Content Types Supported

- **YouTube videos** — Transcribed via yt-dlp + Whisper or transcript services
- **Articles & blog posts** — Extracted via web fetch
- **Tweet threads** — Unwound via fxtwitter API
- **Podcasts** — Transcript search or audio transcription

## Installation

```bash
# Clone the repo
git clone https://github.com/tuncer-deniz/content-claw.git

# Copy to your OpenClaw skills directory
cp -r content-claw ~/.openclaw/skills/

# Restart OpenClaw gateway
openclaw gateway restart
```

The skill auto-registers from `~/.openclaw/skills/`. No additional configuration required.

## Requirements

- **OpenClaw** — [openclaw.ai](https://openclaw.ai)
- **Tools used by this skill:**
  - `web_fetch` — article and transcript extraction (built into OpenClaw)
  - `web_search` — finding transcripts and summaries (built into OpenClaw)
  - `exec` — sub-agent spawning and file operations (built into OpenClaw)
  - `yt-dlp` *(optional)* — YouTube audio download. Install: `pip install yt-dlp`
  - `whisper` *(optional)* — local audio transcription. Install: `pip install openai-whisper`

> **Note on yt-dlp:** YouTube blocks yt-dlp from datacenter/VPS IPs. If you're running OpenClaw on a server, the skill automatically falls back to web-based transcript extraction.

## Workspace Configuration

The skill saves review files relative to your agent's workspace. By default this is `~/.openclaw/workspace/`. If you've configured a custom workspace via the `CLAWD_WORKSPACE` environment variable, review files will be saved there instead:

```bash
export CLAWD_WORKSPACE=/path/to/your/workspace
```

## Adapts to Your Setup

The skill reviews content against whatever goals, knowledge base, and doc structure your agent already has. Add a `USER.md` file to your workspace describing your interests and goals for better-targeted verdicts.

## Files

```
content-claw/
├── SKILL.md                        # Skill definition (agent reads this)
├── eval.yaml                       # Automated eval suite
├── references/
│   └── sub-agent-prompt.md         # Template for spawning extraction sub-agents
├── LICENSE                         # MIT
└── README.md                       # You're reading it
```

## Changelog

### v1.5
- **File naming scheme** — `{category}--{source}--{title-slug}.md` for at-a-glance sorting
- **Duplicate check (Step -1)** — Searches all caught/released folders before running extraction

### v1.4 — Content Claw
- **Renamed** from "content-review" to "Content Claw" 🦞
- **Caught/Released folder structure**
- **URL sanitization** — Strips tracking params before fetching
- **Rich source context** — Sharer, channel, date, conversation context
- **Auto-purge skips** — 14-day auto-delete
- **Monthly skim review** — Numbered list for promote-or-purge

### v1.3
- **Context window protection** — Tier 2+ extraction via sub-agents only
- **Model routing** — Cheaper models for extraction, main agent handles judgment

### v1.2
- Pre-flight checks, tiered fallback hierarchy, retry limits
- Output verification
- 92% token reduction vs v1.1

### v1.1
- Sub-agent output verification with retry
- 60+ minute duration check
- Action vs insight separation

### v1.0
- Initial public release.

## License

MIT
