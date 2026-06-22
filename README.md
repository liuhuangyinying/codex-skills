# Codex Skills

Personal Codex skills collection.

## Skills

| Skill | Purpose | Path |
| --- | --- | --- |
| `feishu-meeting-notes-to-notion` | Read Feishu/Lark meetings, Minutes, AI notes, and transcripts, then create polished meeting notes in Notion. | `skills/feishu-meeting-notes-to-notion` |

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
使用 $feishu-meeting-notes-to-notion 读取 2026-06-20 的飞书会议和妙记，整理会议纪要到这个 Notion 页面下：<Notion链接>
```

Expected output style:

- Create a new Notion child page.
- Page title format: `🐘 YYYY-MM-DD <会议主题>`.
- Body includes meeting date, meeting theme, attendees, concise notes, action items, and original Feishu source links.
- Source links include only Feishu Minutes, AI notes, or transcript links that were actually read and used.
- Do not include debug sections such as `读取情况`, failed API calls, missing permissions, or unread clips in the final Notion note.

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
