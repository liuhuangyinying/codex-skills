---
name: feishu-meeting-notes-to-notion
description: Use this skill when a user asks to read Feishu/Lark meetings, Minutes, AI notes, or transcripts and create or update a polished meeting note page in Notion. Use it especially for same-day meetings split across multiple recordings, when the output should be a new Notion child page with a date-based title, attendees, concise meeting minutes, and source links.
---

# Feishu Meeting Notes To Notion

## Overview

Turn Feishu meeting records, Minutes, AI notes, and transcripts into a clean Notion meeting note. The final page should read like a finished internal document, not an extraction log.

## Workflow

### 1. Confirm Destination And Date

- Identify the target Notion parent page from the user-provided URL.
- Resolve relative dates using the current timezone and exact date.
- Create a new Notion child page unless the user explicitly asks to update an existing page.
- Use an elephant icon for the Notion page when the user's workspace convention expects it.

### 2. Collect Feishu Sources

Use `feishu-cli` when available:

```bash
feishu-cli vc search --start YYYY-MM-DD --end YYYY-MM-DD --page-size 30 -o json
feishu-cli vc recording --meeting-ids <id1,id2> -o json
feishu-cli vc notes --minute-tokens <token1,token2> --with-artifacts --download-transcript --output-dir <tmpdir> --overwrite -o json
feishu-cli vc notes --meeting-ids <id1,id2> --with-artifacts -o json
feishu-cli doc export <docx_id_or_url> --output <file.md>
```

If a command returns a missing-scope error, ask for only the missing authorization scope instead of requesting broad permissions. After the user authorizes, continue with the saved device code.

When multiple recordings belong to the same meeting, group them by:

- same calendar date and topic,
- repeated meeting titles,
- AI notes or transcript references,
- organizer/participant continuity,
- user confirmation that they are the same meeting.

Only use sources that were actually readable as evidence in the final minutes. Do not cite inaccessible recordings as source links.

### 3. Extract Meeting Metadata

Capture:

- meeting date,
- meeting theme,
- attendees,
- source documents/Minutes links.

Attendees should come from Feishu AI notes, transcript speaker names, or meeting metadata. Use a concise line such as:

```text
参会人：Derrick、Kannen、刘黄银瀛、彭宏伟、steven、Olivia 等
```

If attendees are uncertain, say `参会人：从会议记录中识别为 ... 等` only in the internal reasoning, and keep the Notion page concise.

### 4. Title And Icon Rules

Use this Notion page title format:

```text
🐘 YYYY-MM-DD <会议主题>
```

The title must include the date and a human-readable meeting theme. Do not use vague titles like `AI开发落地` if the meeting source gives a more precise theme. Prefer the final meeting theme from the user's edited page or Feishu AI notes.

Inside the body, set:

```text
会议主题：<会议主题>
```

Do not include the elephant in the body `会议主题` line unless the user explicitly asks for it. The icon belongs in the page title/icon.

### 5. Notion Page Structure

Use this structure by default:

```markdown
# 会议概况
会议日期：YYYY-MM-DD
会议主题：<会议主题>
参会人：<参会人>
整理说明：根据 <已读取来源> 合并整理。

# 核心结论
1. ...
2. ...
3. ...

# <主题模块一>
...

# <主题模块二>
...

# 行动项
- ...

# 飞书原会议链接
- [<已读取来源名称>](<feishu-url>)
- [<已读取来源名称>](<feishu-url>)
```

Keep the page as a polished meeting note:

- Include `会议概况`, `核心结论`, topic sections, `行动项`, and `飞书原会议链接`.
- Do not include process/debug sections such as `读取情况`, permission failures, failed API calls, or inaccessible clips.
- If some clips were found but unreadable, mention them to the user in the final response, not in the Notion page, unless the user asks for an audit trail.
- `飞书原会议链接` must include only sources that were actually read and used in the body.
- Prefer original Feishu source links such as Minutes URLs, AI notes docx URLs, and transcript docx URLs. Do not link unrelated meeting list fragments.

### 6. Numbering And Formatting

- For `核心结论`, normal Markdown numbered lists are acceptable.
- For process sections where Notion may render every item as `1.`, use subheadings instead:

```markdown
## 1. 明确业务 SOP
...

## 2. 需求拆解
...
```

- Avoid nested bullets unless necessary.
- Keep paragraphs readable and concise.
- Avoid turning every source statement into a timestamped transcript summary; synthesize decisions, discussion, and action items.

### 7. Source Link Rules

The source links section should be compact and evidence-based:

```markdown
# 飞书原会议链接
- [11:58 技术研讨｜妙记](https://...)
- [15:14 AI驱动产品开发落地经验分享会｜AI notes](https://...)
- [15:14 AI驱动产品开发落地经验分享会｜逐字稿](https://...)
```

Do not include:

- searched but unread recordings,
- inaccessible recordings,
- implementation logs,
- temporary local file paths,
- API error details.

### 8. Create Or Update In Notion

Before creating a page, fetch the parent Notion page to confirm it is the intended destination.

When creating:

- parent: user-provided Notion page ID/URL,
- page property `title`: `🐘 YYYY-MM-DD <会议主题>`,
- icon: `🐘`,
- content: final polished Markdown.

When updating an existing page:

- fetch it first,
- apply targeted `update_content` changes for small edits,
- use `replace_content` only when replacing the full document is intended.

### 9. Final Response To User

Return the Notion page link and a concise status. Mention any source limitations only if they matter, e.g.:

```text
已创建：<Notion link>
已合并 15:14 AI notes/逐字稿和 11:58 妙记内容。09:57 录制可定位但当前无正文读取权限，未写入正文。
```

Do not include credentials, tokens, or long command output.
