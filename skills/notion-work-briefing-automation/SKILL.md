---
name: notion-work-briefing-automation
description: Use this skill when a user wants to create, update, or share a Codex automation that generates polished daily work briefings in Notion from a task database plus recent work-log pages. Use it especially when the output must be a final Notion page, not a process log, with today's priorities, tomorrow's work, recent progress, per-person summaries, risks, and source links.
---

# Notion Work Briefing Automation

## Overview

Create or maintain a recurring Codex automation that generates a daily Notion work briefing. The briefing should synthesize both structured tasks and unstructured work records, then write a polished page under a target Notion parent page.

The final Notion page is a result document. It must not expose search steps, permission notes, debug logs, failed query details, or "supplement" sections.

## Required Inputs

Ask the user for these values if they are not already provided:

- `TASK_DATABASE_URL`: Notion task database URL.
- `TASK_DATA_SOURCE_ID`: Notion task data source ID, if available.
- `WORK_LOG_SOURCE_URL`: Notion page, database, or workspace hub that contains recent work records.
- `WORK_LOG_DATA_SOURCE_ID`: Optional Notion data source ID for project/work-log records.
- `OUTPUT_PARENT_PAGE_URL`: Notion page where briefing pages should be created.
- `CORE_MEMBERS`: People who must appear in the per-person summary.
- `RUN_TIME`: Daily run time, usually `10:00`.
- `TIMEZONE`: User timezone.

Use placeholders when packaging this skill for sharing. Do not commit private Notion URLs, tokens, or personal workspace IDs unless the user explicitly wants a private repository to contain them.

## Automation Prompt Template

Use the template in `templates/automation-prompt.zh-CN.md` as the base prompt. Replace placeholders before creating or updating an automation.

The prompt must require:

- Read the task database.
- Read recent work records from the work-log source.
- For each run, treat the execution date as "today".
- Summarize the last two calendar days of work records: yesterday and today.
- For a backfilled report, use the requested report date and its previous day.
- Create or update a child page under the output parent page.
- Use page title format: `yyyy-MM-dd 「工作早报」`.
- If the page already exists, replace or reconstruct the body as one complete final report. Do not append a "补充" section.

## Notion Page Structure

Use this Chinese structure by default:

```markdown
# 今日重点

- ...

# 今日应该做什么

## <负责人或事项>

- ...

# 明天要做什么

- ...

# 近两天工作提炼

- ...

# 昨天团队进展

## <核心成员一>

- ...

# 风险与提醒

- ...

# 数据来源

- <page url="...">...</page>
```

Keep the report concise and decision-ready:

- Put the most important items first.
- Prefer synthesis over raw extraction.
- Include dates, task status, project/goal, and owner when available.
- For missing evidence, write `未找到明确记录`; do not invent progress.
- The final page should read like a morning briefing, not like a retrieval audit.

## Data Collection Rules

### Task Database

Use Notion data-source query when available. If it is unavailable, use Notion search scoped to the task database/data source and fetch the relevant pages.

Prioritize:

- tasks due today,
- tasks requested today,
- tasks due tomorrow,
- in-progress or not-started tasks,
- overdue tasks,
- recently updated tasks,
- tasks assigned to core members.

### Work Log Source

The work-log source is equally important. Many real work items may not be in the task database.

Search within the work-log source by:

- `yyyy-MM-dd`,
- `M月d日`,
- `MM-DD`,
- project names,
- core member names,
- words such as `会议`, `复盘`, `体验`, `推进`, `总结`, `行动项`.

Fetch matching pages and synthesize:

- actual work completed or advanced,
- experience or methods captured,
- new tasks,
- risks and follow-ups.

Do not conclude "no progress" from the task database alone.

## Creating The Automation

Use Codex app automations when available:

- `kind`: `cron`
- schedule: daily at the user's requested local time
- execution environment: local, unless the user requests another environment
- status: `ACTIVE`, unless the user asks to pause it
- prompt: rendered final prompt from `templates/automation-prompt.zh-CN.md`

If updating an existing automation, preserve its schedule and environment unless the user asks to change them.

## Creating Or Updating Notion Briefings

Before writing:

1. Fetch the output parent page to verify access.
2. Search under the output parent page for the exact report title.
3. If the page exists, replace the content with a complete rewritten report.
4. If it does not exist, create a new child page.

Never leave:

- `自动生成`,
- `补充`,
- `当前连接器不能...`,
- SQL/query failure details,
- search notes,
- "未读取到所以可能没有" process language.

Those details may be mentioned in the final chat response only if they materially affect confidence.

## Final Response To User

Keep the response short:

```text
已创建/更新：<Notion link>
自动化已更新为每天 10:00 生成最终版工作早报。
```

Mention limitations only when necessary.
