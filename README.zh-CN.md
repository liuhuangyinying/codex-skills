# Codex Skills

[![English](https://img.shields.io/badge/Language-English-blue)](README.md)

个人常用 Codex skills 合集。

## Skills

| Skill | 用途 | 路径 |
| --- | --- | --- |
| `feishu-meeting-notes-to-notion` | 读取飞书会议、妙记、AI notes 和逐字稿，并整理成 Notion 会议纪要。 | `skills/feishu-meeting-notes-to-notion` |

## 安装单个 Skill

从本仓库安装某一个 skill：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/feishu-meeting-notes-to-notion
```

安装完成后重启 Codex，让新 skill 被自动发现。

## 使用飞书会议纪要 Skill

示例提示词：

```text
使用 $feishu-meeting-notes-to-notion 读取 2026-06-20 的飞书会议和妙记，整理会议纪要到这个 Notion 页面下：<Notion链接>
```

预期输出风格：

- 在目标 Notion 页面下创建新的子页面。
- 页面标题格式：`🐘 YYYY-MM-DD <会议主题>`。
- 正文包含会议日期、会议主题、参会人、精炼纪要、行动项和飞书原始来源链接。
- 来源链接只放实际读取并作为正文依据的飞书妙记、AI notes 或逐字稿链接。
- 最终 Notion 纪要中不要包含 `读取情况`、API 调用失败、权限缺失、未读取片段等调试信息。

## 依赖

使用 `feishu-meeting-notes-to-notion` 前，需要：

- 已安装并配置 `feishu-cli`。
- 已完成飞书会议、妙记、逐字稿和文档读取所需授权。
- Codex 已连接 Notion。

常用检查命令：

```bash
feishu-cli auth status
feishu-cli doctor
```

## 仓库结构

每个 skill 单独放在 `skills/` 下：

```text
skills/
└── skill-name/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

`SKILL.md` 是必需文件。`agents/openai.yaml` 可选，但建议添加，用于提供更好的 Codex UI 展示名称和默认提示词。

## 新增一个 Skill

1. 在 `skills/<skill-name>/` 下创建新文件夹。
2. 添加合法的 `SKILL.md`，包含 frontmatter：

```markdown
---
name: skill-name
description: Clear description of when to use this skill.
---
```

3. 可选添加 `agents/openai.yaml`。
4. 提交并推送。

其他用户即可通过以下命令安装：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/<skill-name>
```
