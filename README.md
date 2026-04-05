# Essential Skills for OpenClaw

A collection of high-quality, production-ready skills for [OpenClaw](https://github.com/openclaw/openclaw) — the extensible agent framework.

## What is OpenClaw?

OpenClaw is an extensible AI agent framework that lets you add capabilities via skills — modular instruction sets that teach your agent how to perform specific tasks.

## Skills in This Repository

| Skill | Description |
|-------|-------------|
| **codex-delegate** | Mandatory delegation for programming tasks using OpenAI's Codex CLI. |
| **reddit-poster** | Create and submit posts to Reddit using old.reddit.com with CSS selectors. |

### About the codex-delegate skill

OpenClaw comes bundled with a generic skill called **"coding-agent"** that works with multiple coding agents (Claude Code, Codex, and Pi). This repository's **codex-delegate** skill is specifically tailored for **OpenAI's Codex CLI**, providing optimized prompts and workflow for that particular tool.

## Installation

Clone this repository into your OpenClaw skills directory:

```bash
cd ~/.openclaw/skills
git clone https://github.com/balazspiller/essential-skills.git
```

Or copy individual skill folders to your OpenClaw skills directory.

## Contributing

Contributions welcome! Each skill should include:
- A `SKILL.md` with clear usage instructions
- Any required scripts or dependencies documented
- Practical examples where applicable

## License

MIT — Use freely, modify, improve, share.

---

*Curated by [balazspiller](https://github.com/balazspiller) and [remi8910](https://github.com/remi8910)*
