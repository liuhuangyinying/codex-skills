# Codex Skills

[![中文](https://img.shields.io/badge/语言-中文-blue)](README.zh-CN.md)

Personal Codex skills collection.

## Skills

| Skill | Purpose | Path |
| --- | --- | --- |
| `feishu-meeting-notes-to-notion` | Read Feishu/Lark meetings, Minutes, AI notes, and transcripts, then create polished meeting notes in Notion. | `skills/feishu-meeting-notes-to-notion` |
| `notion-work-briefing-automation` | Create or maintain a Notion daily work briefing automation from a task database and recent work-log pages. | `skills/notion-work-briefing-automation` |

## Integration Guides

Plugin and connector setup guides live in [`plugin-integration-guides/`](plugin-integration-guides/).

| Guide | Purpose |
| --- | --- |
| [Authorize Notion In Codex](plugin-integration-guides/notion-authorization.zh-CN.md) | Chinese guide for connecting Notion to Codex and verifying access. |

## Install A Skill

Install a single skill from this repository:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/feishu-meeting-notes-to-notion
```

Install the Notion work briefing automation skill:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/notion-work-briefing-automation
```

Restart Codex after installation so the new skill can be discovered.

## Use The Feishu Meeting Skill

Example prompt:

```text
Use $feishu-meeting-notes-to-notion to read the Feishu meetings and Minutes from 2026-06-20, then create a meeting note under this Notion page: <Notion URL>
```

Expected output style:

- Create a new Notion child page.
- Page title format: `🐘 YYYY-MM-DD <meeting theme>`.
- Body includes meeting date, meeting theme, attendees, concise notes, action items, and original Feishu source links.
- Source links include only Feishu Minutes, AI notes, or transcript links that were actually read and used.
- Do not include debug sections such as read-status logs, failed API calls, missing permissions, or unread clips in the final Notion note.

## Use The Notion Work Briefing Automation Skill

Example prompt:

```text
Use $notion-work-briefing-automation to create a Notion work briefing automation that runs every day at 10:00.

Task database: <Notion task database URL>
Work-log source: <Notion work-log or knowledge base URL>
Output parent page: <Notion parent page URL>
Core members: Ellen, 胡琛, Linda, 沐公子
```

Expected output style:

- Create or update `yyyy-MM-dd 「工作早报」` under the target Notion page.
- Read both the task database and recent work-log pages.
- Include today's priorities, today's work, tomorrow's work, recent work synthesis, yesterday's per-person progress, risks, and source links.
- Keep the final briefing clean: no retrieval process, permission notes, failed query logs, or "supplement" sections.

## Requirements

For `feishu-meeting-notes-to-notion`, the user should have:

- `feishu-cli` installed and configured.
- Feishu authorization for the needed meeting, Minutes, transcript, and document scopes.
- Notion connected in Codex.

Useful `feishu-cli` commands:

```bash
feishu-cli auth status
feishu-cli doctor
```

## Install And Configure feishu-cli For Codex

`feishu-cli` is the Feishu/Lark Open Platform CLI used by the Feishu-related skills in this repository. See the upstream project: [riba2534/feishu-cli](https://github.com/riba2534/feishu-cli).

Recommended install:

```bash
curl -fsSL https://raw.githubusercontent.com/riba2534/feishu-cli/main/install.sh | bash
```

Verify installation:

```bash
which feishu-cli
feishu-cli --version
feishu-cli --help
```

Create and save a Feishu app configuration:

```bash
feishu-cli config create-app --save
```

Or configure an existing app with `~/.feishu-cli/config.yaml` or environment variables:

```bash
export FEISHU_APP_ID="cli_xxx"
export FEISHU_APP_SECRET="xxx"
```

Many read operations need a user token. Log in with OAuth Device Flow:

```bash
feishu-cli auth login
feishu-cli auth status --verify -o json
```

For search-related work, request recommended search scopes:

```bash
feishu-cli auth login --domain search --recommend
```

Check a specific scope:

```bash
feishu-cli auth check --scope "search:docs:read"
```

Run a health check before using Feishu skills from Codex:

```bash
feishu-cli doctor
```

For detailed Chinese setup notes, see [`README.zh-CN.md`](README.zh-CN.md#在-codex-中安装和配置-feishu-cli).

## Repository Layout

Each skill lives in its own folder under `skills/`:

```text
skills/
└── skill-name/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

`SKILL.md` is required. `agents/openai.yaml` is optional but recommended for a better Codex UI label and default prompt.

## Add A New Skill

1. Create a new folder under `skills/<skill-name>/`.
2. Add a valid `SKILL.md` with frontmatter:

```markdown
---
name: skill-name
description: Clear description of when to use this skill.
---
```

3. Optionally add `agents/openai.yaml`.
4. Commit and push.

Users can then install it with:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/<skill-name>
```
