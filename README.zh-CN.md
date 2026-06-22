# Codex Skills

[![English](https://img.shields.io/badge/Language-English-blue)](README.md)

个人常用 Codex skills 合集。

## Skills

| Skill | 用途 | 路径 |
| --- | --- | --- |
| `feishu-meeting-notes-to-notion` | 读取飞书会议、妙记、AI notes 和逐字稿，并整理成 Notion 会议纪要。 | `skills/feishu-meeting-notes-to-notion` |
| `notion-work-briefing-automation` | 创建或维护 Notion 工作早报自动化，从任务面板和工作记录生成每日早报。 | `skills/notion-work-briefing-automation` |

## 插件集成指南

Codex 插件、连接器和外部工具的配置说明放在 [`plugin-integration-guides/`](plugin-integration-guides/)。

| 指南 | 用途 |
| --- | --- |
| [Codex 授权 Notion 操作指引](plugin-integration-guides/notion-authorization.zh-CN.md) | 说明如何在 Codex 中连接 Notion，并验证是否授权成功。 |

## 安装单个 Skill

从本仓库安装某一个 skill：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/feishu-meeting-notes-to-notion
```

安装 Notion 工作早报自动化 skill：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/notion-work-briefing-automation
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

## 使用 Notion 工作早报自动化 Skill

示例提示词：

```text
使用 $notion-work-briefing-automation 帮我创建一个每天 10:00 的 Notion 工作早报自动化。

任务面板：<Notion 任务数据库链接>
工作记录页面：<Notion 工作记录/知识库链接>
输出到：<Notion 工作日志父页面链接>
核心成员：Ellen、胡琛、Linda、沐公子
```

预期输出风格：

- 每天在目标 Notion 页面下创建或更新 `yyyy-MM-dd 「工作早报」`。
- 同时读取任务面板和工作记录页面。
- 正文包含今日重点、今日事项、明日事项、近两天工作提炼、昨天团队进展、风险提醒和数据来源。
- 最终早报只呈现结果，不包含检索过程、权限限制、失败日志或“补充”段落。

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

## 在 Codex 中安装和配置 feishu-cli

`feishu-cli` 是飞书开放平台命令行工具，当前仓库里的飞书相关 skill 会通过它读取会议、妙记、AI notes、逐字稿和飞书文档。官方仓库见 [riba2534/feishu-cli](https://github.com/riba2534/feishu-cli)。

### 1. 安装 feishu-cli

推荐使用官方一键安装脚本：

```bash
curl -fsSL https://raw.githubusercontent.com/riba2534/feishu-cli/main/install.sh | bash
```

已安装的用户再次执行同一命令即可更新。安装完成后检查：

```bash
which feishu-cli
feishu-cli --version
feishu-cli --help
```

如果 Codex 终端提示 `command not found`，确认二进制所在目录已经加入 `PATH`。常见位置包括：

```bash
/usr/local/bin/feishu-cli
~/.local/bin/feishu-cli
```

也可以使用 Go 安装：

```bash
go install github.com/riba2534/feishu-cli@latest
```

### 2. 创建或配置飞书应用凭证

推荐让 CLI 自动创建应用并保存凭证：

```bash
feishu-cli config create-app --save
```

按终端提示打开授权链接，用飞书扫码确认。成功后，配置会写入：

```bash
~/.feishu-cli/config.yaml
```

如果已经有飞书开放平台应用，也可以手动配置：

```bash
feishu-cli config init
```

然后编辑 `~/.feishu-cli/config.yaml`，或使用环境变量：

```bash
export FEISHU_APP_ID="cli_xxx"
export FEISHU_APP_SECRET="xxx"
```

配置优先级通常是：环境变量 > 配置文件。

### 3. 开通权限 scope

在飞书开放平台的应用权限管理中，为应用开通需要的权限。不同能力需要不同 scope：

- 读取妙记、AI notes、逐字稿：会议、妙记、文档读取相关权限。
- 搜索文档或消息：搜索相关权限。
- 读取群聊或私聊：消息历史、群聊、用户相关权限。
- 写入或导出飞书文档：云文档、云空间相关权限。

不建议一次性申请过宽权限。遇到 `missing scope` 报错时，按错误提示补充对应 scope，再重新授权。

### 4. 完成 User Access Token 登录

很多读取类能力需要用户身份，而不仅是 Bot 身份。使用 OAuth Device Flow 登录：

```bash
feishu-cli auth login
```

按终端输出打开链接或扫码完成授权。

也可以按场景申请推荐权限：

```bash
feishu-cli auth login --domain search --recommend
```

或显式指定 scope：

```bash
feishu-cli auth login --scope "minutes:minutes.basic:read minutes:minutes.transcript:export"
```

检查当前登录状态：

```bash
feishu-cli auth status
feishu-cli auth status --verify -o json
```

检查某个 scope 是否已授权：

```bash
feishu-cli auth check --scope "search:docs:read"
```

刷新 token：

```bash
feishu-cli auth refresh
```

退出登录：

```bash
feishu-cli auth logout
```

### 5. 健康检查

配置完成后运行：

```bash
feishu-cli doctor
```

建议在让 Codex 调用飞书 skill 前，至少确认：

- `config` 正常。
- `user_token` 正常，或当前任务确实只需要 Bot Token。
- 网络和飞书 endpoint 可访问。
- 本地依赖检查通过。

### 6. 常用验证命令

验证文档创建：

```bash
feishu-cli doc create --title "Hello Feishu"
```

验证飞书会议/妙记能力：

```bash
feishu-cli vc search --start 2026-06-20 --end 2026-06-20 --page-size 10 -o json
```

验证文档搜索：

```bash
feishu-cli auth login --domain search --recommend
feishu-cli search docs "关键词" -o json
```

验证当前可用命令：

```bash
feishu-cli --help
feishu-cli auth --help
feishu-cli vc --help
```

### 7. 在 Codex 中使用

安装并配置完成后，可以直接让 Codex 使用对应 skill。例如：

```text
使用 $feishu-meeting-notes-to-notion 读取 2026-06-20 的飞书会议和妙记，整理会议纪要到这个 Notion 页面下：<Notion链接>
```

Codex 执行过程中如果遇到权限缺失，应只补充缺失的 scope，然后继续原任务。最终写入 Notion 的纪要不应包含读取失败日志、权限调试信息或命令输出。

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
