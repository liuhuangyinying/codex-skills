# Codex Skills

[![中文](https://img.shields.io/badge/语言-中文-blue)](README.zh-CN.md)

Personal Codex skills collection.

## Skills

| Skill | Purpose | Path |
| --- | --- | --- |
| `feishu-meeting-notes-to-notion` | Read Feishu/Lark meetings, Minutes, AI notes, and transcripts, then create polished meeting notes in Notion. | `skills/feishu-meeting-notes-to-notion` |

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
