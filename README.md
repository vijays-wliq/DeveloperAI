# Developer AI

Instruction docs that Claude Code follows to build websites — from a Figma design to HTML, and from
HTML to a WordPress theme (custom PHP / ACF, or Elementor). Lessons from real builds are added to
these docs as new rules, so every next build starts from them.

## Instruction docs

| File | Use it for |
|---|---|
| [`PROJECT-BOOTSTRAP.md`](PROJECT-BOOTSTRAP.md) | **Figma → HTML.** One-time setup for an empty project folder. Builds a token-driven front-end starter (`vendor.css`, `global.html`, `CLAUDE.md`, folder structure) from the Figma file. |
| [`CLAUDE-WORDPRESS-CONVERSION-EXPERT.md`](CLAUDE-WORDPRESS-CONVERSION-EXPERT.md) | **HTML → custom WordPress theme** (PHP + ACF PRO). Includes the page builder choice — ACF Flexible Content / Gutenberg (ACF Blocks) / Both (§108), forms plugin choice (§109), ACF PRO only (§110) and ACF local JSON + sync (§111). |
| [`CLAUDE-ELEMENTOR-WORDPRESS-CONVERSION-EXPERT.md`](CLAUDE-ELEMENTOR-WORDPRESS-CONVERSION-EXPERT.md) | **HTML → WordPress with Elementor.** Main rule (§96): the Elementor site must match the HTML site visually and functionally. |
| [`wordpress_theme_developer_brief.md`](wordpress_theme_developer_brief.md) | Finishing a site on an **existing WordPress theme** to the client's requirements. |

## Other files

- `Skill-PDFs/` — PDF versions of the four skills: HTML Design, WordPress ACF, WordPress Elementor,
  WordPress Theme-Based.
- `HTML-Build-Time-Report.html` — build-time report for the HTML build with `PROJECT-BOOTSTRAP.md`.
- `All-Skills-Build-Time-Report.html` — build-time report for all four skills with Claude Code.

## How to use

1. Copy the right instruction doc into the project folder (next to the Figma link or the HTML site).
2. Tell Claude Code to follow it.
3. For WordPress builds, answer the questions asked at the start: page builder (§108) and forms
   plugin (§109).
4. Put the site login in `login.md` in the project folder (§110). **Never commit `login.md`.**

---

## GSD (Get Shit Done) — Installation & Setup Guide

GSD is a spec-driven development system for Claude Code: it plans a project in phases, then
discusses, plans, executes and verifies each phase.

- **Repo:** [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core) — the active home of GSD.
  The old repo [gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) is archived
  (read-only) and points to GSD Core.
- **Commands reference:** [COMMANDS.md](https://github.com/open-gsd/gsd-core/blob/next/docs/COMMANDS.md)
  (old copy: [gsd-build COMMANDS.md](https://github.com/gsd-build/get-shit-done/blob/main/docs/COMMANDS.md))
- **Install guide:** [install-on-your-runtime.md](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/install-on-your-runtime.md)

### Requirements (Mac and Windows)

- **Node.js 18+** and npm (npx comes with npm)
- **Claude Code** installed

Check Node:

```bash
node --version    # must be v18 or higher
```

### macOS

1. Install Node.js 18+ (skip if `node --version` already shows 18+):
   - Download the LTS installer from [nodejs.org](https://nodejs.org), **or**
   - with Homebrew: `brew install node`
2. Open **Terminal** and install GSD for Claude Code (all projects):

   ```bash
   npx @opengsd/gsd-core@latest --claude --global
   ```

   Only for the current project folder instead:

   ```bash
   npx @opengsd/gsd-core@latest --claude --local
   ```

3. Global install goes into Claude Code's config folder: `~/.claude/`.
4. **Restart Claude Code.**

### Windows

1. Install Node.js 18+ (skip if `node --version` already shows 18+):
   - Download the LTS installer from [nodejs.org](https://nodejs.org), **or**
   - in PowerShell: `winget install OpenJS.NodeJS.LTS`
   - Close and reopen PowerShell after installing, so `node` and `npx` are found.
2. Open **PowerShell** and install GSD for Claude Code (all projects):

   ```powershell
   npx @opengsd/gsd-core@latest --claude --global
   ```

   Only for the current project folder instead:

   ```powershell
   npx @opengsd/gsd-core@latest --claude --local
   ```

3. Global install goes into Claude Code's config folder: `%USERPROFILE%\.claude\`.
   GSD's Claude Code hooks run as PowerShell commands and work with or without Git Bash.
4. **Restart Claude Code.**

### Check that it works

1. Restart Claude Code, then type `/gsd` — the GSD commands should appear.
2. Start with:
   - `/gsd-new-project` — new project (empty folder)
   - `/gsd-onboard` — existing codebase

> **Command format:** the current GSD Core docs use the hyphen form (`/gsd-new-project`). Older
> installs (from the archived repo) show the colon form (`/gsd:new-project`). Use whichever form
> appears when you type `/gsd` in Claude Code.

### Update

Run the installer again — the same command works for a fresh install and for an update:

```bash
npx @opengsd/gsd-core@latest --claude --global
```

For update and uninstall details, see
[update-gsd.md](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/update-gsd.md).

### Troubleshooting

- **Command not found / no `/gsd` commands:** restart Claude Code; check that GSD was installed for
  Claude Code (`--claude`) and in the right place (`--global` = all projects, `--local` = this
  folder only).
- **`npx` not found:** Node.js is missing or not on PATH — reinstall Node.js 18+ and reopen the
  terminal.
- Do not copy GSD files from `agents/` or `commands/` by hand — always use the installer.

### Workflow

```text
/gsd-new-project  (or /gsd-onboard for an existing codebase)
        ↓
for each phase N:
  /gsd-discuss-phase N  →  /gsd-plan-phase N  →  /gsd-execute-phase N  →  /gsd-verify-work N  →  /gsd-ship N
        ↓
/gsd-complete-milestone
```

Not sure what's next? Run `/gsd-next` or `/gsd-progress`.

### Useful commands

| Command | What it does |
|---|---|
| `/gsd-new-project` | Start a new project with deep context gathering |
| `/gsd-onboard` | Guided onboarding for an existing codebase |
| `/gsd-spec-phase N` | Clarify what a phase delivers (SPEC.md), before discuss |
| `/gsd-discuss-phase N` | Gather phase context through questions before planning |
| `/gsd-ui-phase N` | UI design contract for frontend phases |
| `/gsd-plan-phase N` | Research, plan and verify a phase |
| `/gsd-execute-phase N` | Execute all plans in a phase |
| `/gsd-verify-work N` | User acceptance testing of the phase |
| `/gsd-ship N` | Create a PR from the finished phase |
| `/gsd-ui-review N` | Visual audit of the built frontend |
| `/gsd-complete-milestone` | Archive the milestone and prepare the next one |
| `/gsd-next` | Go to the next logical step |
| `/gsd-progress` | Show project status and what to do next |
| `/gsd-pause-work` / `/gsd-resume-work` | Save context and continue later |
| `/gsd-quick` | Small ad-hoc task with GSD guarantees |
| `/gsd-autonomous` | Run all remaining phases |
| `/gsd-debug` | Systematic debugging with saved state |
| `/gsd-undo` | Safely revert phase / plan commits |
| `/gsd-health` | Check the planning folder (`--repair` to fix) |
| `/gsd-help` | List all GSD commands |

Full list: [COMMANDS.md](https://github.com/open-gsd/gsd-core/blob/next/docs/COMMANDS.md).
