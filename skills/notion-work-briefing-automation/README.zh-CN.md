# Notion 工作早报自动化

这个 skill 用于创建或维护 Codex 自动化：每天从 Notion 任务面板和工作记录页面读取信息，生成一份完整的工作早报。

## 适用场景

- 每天固定时间生成工作早报。
- 同时读取结构化任务和非结构化工作记录。
- 输出到 Notion 指定父页面下。
- 已有早报需要整体重构，而不是追加“补充”。
- 最终页面只呈现结果，不暴露检索过程。

## 安装

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo liuhuangyinying/codex-skills \
  --path skills/notion-work-briefing-automation
```

安装后重启 Codex。

## 使用示例

```text
使用 $notion-work-briefing-automation 帮我创建一个每天 10:00 的 Notion 工作早报自动化。

任务面板：<Notion 任务数据库链接>
工作记录页面：<Notion 工作记录/知识库链接>
输出到：<Notion 工作日志父页面链接>
核心成员：Ellen、胡琛、Linda、沐公子
```

## 文件说明

- `SKILL.md`：Codex skill 主说明。
- `templates/automation-prompt.zh-CN.md`：自动化提示词模板。
- `config.example.md`：配置占位符示例。

## 输出页面格式

页面标题：

```text
yyyy-MM-dd 「工作早报」
```

正文结构：

- 今日重点
- 今日应该做什么
- 明天要做什么
- 近两天工作提炼
- 昨天团队进展
- 风险与提醒
- 数据来源

## 重要原则

- 不要只看任务面板。
- 工作记录/知识库页面也要作为重要来源。
- 不要在 Notion 正文里写检索过程、权限限制、失败日志。
- 已有早报要重构成完整成稿，不要追加“补充”。
- 没有证据时写“未找到明确记录”，不要编造。
