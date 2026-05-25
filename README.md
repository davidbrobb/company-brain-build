# Company Brain Build

A guided starter company brain build that teaches Claude how your business works, builds the brain for you based on your own business — then shows you how to keep using it.

This is not the final form of a company brain. It's the first working version: a local markdown system for decisions, processes, customers, team context, and the operating knowledge your AI needs before it can be genuinely useful.

Open the kit in Claude Code and Claude walks you through the build. No prior setup, no terminal commands, no config files to write.

---

## What you need

- [Claude Code](https://claude.ai/code) installed on your desktop
- [Obsidian](https://obsidian.md) — optional but recommended for exploring your brain visually (free)
- [Wispr Flow](https://wisprflow.ai) — optional if you prefer dictating your answers or future captures (free trial)

Nothing else. No APIs or extra setup. Claude Code is the AI workspace; Obsidian and Wispr Flow are optional helpers.

---

## How to start

1. Click the green **Code** button above → **Download ZIP** → extract/unzip it

   > The extracted folder will be named `company-brain-build-main` — that's normal, it's GitHub's default.

2. Open the extracted folder in Claude Code

   > Claude reads the project config and begins the interview. If nothing starts straight away, tell Claude: **"Please read CLAUDE.md and run the setup."**

   > The actual brain template lives inside the extracted folder at `company-brain/`. Claude will customise that folder, then help you move only that folder to its final home.

**Don't `git clone` this repo.** Use Download ZIP. You're not forking it — you're using it as a one-time scaffold for your own brain, which then lives entirely on your machine. If you clone it, your notes end up tracked against this repo. That's not what you want.

---

## What you'll have after ~5 minutes

- **A starter company brain** built around your actual business, not a blank template
- **A root brain file** that links decisions, customers, processes, team context, and strategy together
- **The `/brain-entry` skill** at `skills/brain-entry/SKILL.md` if you choose to install it, so you can capture decisions, processes, team notes, and more in any session with correct formatting
- **A global Claude reference** (optional) so Claude always knows about your brain when you open Claude Code anywhere

---

## Security

Everything in this kit lives 100% locally on your computer. No cloud sync, no external services, no data leaves your machine.

Claude Code connects to Anthropic's servers to run the AI — that's the only external connection, and it's covered by [Anthropic's security and privacy policies](https://www.anthropic.com/security).

Your brain files are plain markdown — you own them completely and can open them in any text editor.

---

## For maintainers

`.gitignore` is intentionally absent — rules live in `.git/info/exclude` so they don't ship to users. After a fresh clone, re-add your local exclude rules there.

---

Built by [David Robb](https://aibc.consulting) · [AI for Business hub](https://www.notion.so/davidrobb/AI-for-Business-by-davidbrobb-363682b9900d8070b0f1c499d35ed272)
