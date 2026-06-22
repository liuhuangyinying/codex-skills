# Notion 工作早报自动化配置样例

复制本文件，把占位符替换成自己的 Notion 页面和数据源。

```text
TASK_DATABASE_URL=https://app.notion.com/p/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TASK_DATA_SOURCE_ID=collection://xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

WORK_LOG_SOURCE_URL=https://app.notion.com/p/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
WORK_LOG_DATA_SOURCE_ID=collection://xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

OUTPUT_PARENT_PAGE_URL=https://app.notion.com/p/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

CORE_MEMBERS=Ellen、胡琛、Linda、沐公子
RUN_TIME=10:00
TIMEZONE=Asia/Shanghai
```

## 字段建议

任务数据库建议至少包含：

- 任务名：如 `Task name`
- 负责人：如 `Assignee`
- 状态：如 `Status`
- 计划完成日期
- 需求传递日期
- 项目
- 目标

工作记录页面可以是：

- 项目数据库
- 工作日志页
- 知识库首页
- 团队每日记录页

早报会同时读取任务数据库和工作记录页面。不要只把没有任务字段的真实工作排除在外。
