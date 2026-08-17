# LingXi 社区协作

LingXi 使用 Organization Discussions 作为统一的社区入口。这里用于提出需求、发布每周任务、提交成果、分享技术札记和发布公告；进入开发后的具体工作由目标仓库的 Issue 与 Pull Request 跟踪。

## 分类

| 分类 | 用途 | slug |
| --- | --- | --- |
| 公告 | 重要动态、版本与项目进展 | `announcements` |
| 想法与需求 | 提出新的产品或工程需求 | `ideas-and-requests` |
| 共建实验室 | 每周任务与成果提交 | — |
| 每周任务 | 导师发布开放式共建任务 | `weekly-missions` |
| 任务提交 | 提交任务成果 | `submissions` |
| 技术札记 | 分享可复用的工程与研究经验 | `engineering-notes` |

分类显示名使用中文，slug 和 Discussion Form 文件名保持英文。

## 角色

- **Mentors**：发布任务、确认需求和 Issue、验收任务成果、参与 PR 审阅。
- **Builders**：参与任务、提出需求、开发 Issue、提交 PR。
- **Owner (`leecyang`)**：拥有组织管理权限，同时参与 Mentors 的工作。

PR 审阅规则：

- `leecyang` 提交的 PR：无需额外批准。
- 其他 Mentor 提交的 PR：需要 `leecyang` 批准。
- Builder 或外部贡献者提交的 PR：需要任意 2 位 Mentors 批准。

除 PR 外，需求、Issue、任务关闭和成果验收都只需要任意 1 位 Mentor 处理。

## 想法与需求

成员先在“想法与需求”中说明场景、问题和期望结果。

任意 Mentor 可以回复：

```text
/approve
/approve LingxiSkills
/decline
```

通过后会提供目标仓库的 Issue 创建入口。

## Issue

从已确认需求创建的 Issue 可以直接进入开发。

直接创建的开发 Issue 需要一位 Mentor 确认：

```text
/approve-issue
/decline-issue
```

## 每周任务

每周任务不限定为代码。可以是：

- 文档或报告
- 外部作品或链接
- 技术札记
- Pull Request
- 实验或 Demo
- 参与指定 Issue
- 无需单独提交
- 不限形式

任务发布后会自动通知 `@LingXi-Org/builders`。

任务结束时，任意 Mentor 可以回复：

```text
/close-mission
```

## 任务提交

仅在任务要求单独提交成果时使用。成果可以直接写在正文中，也可以提供链接。

任意 Mentor 可以回复：

```text
/accept
/needs-work
```

## 技术札记

“技术札记”用于沉淀架构设计、工程经验、实验结论和研究思考。内容应尽量独立、完整并可长期检索。

## 自动通知

- 新需求 → `@LingXi-Org/mentors`
- 新开发 Issue → `@LingXi-Org/mentors`
- Mentor PR → `@leecyang`
- Builder / 外部贡献者 PR → `@LingXi-Org/mentors`
- 新每周任务 → `@LingXi-Org/builders`
- 新任务提交 → `@LingXi-Org/mentors`

## 维护配置

来源仓库：`LingXi-Org/.github`

必需 Secret：

```text
ORG_GOVERNANCE_TOKEN
```

该 Secret 只用于读取 Mentors Team 成员。邀请成员使用的 `ORG_INVITE_TOKEN` 与它保持独立。

自动公告默认关闭。需要时再设置：

```text
DISCUSSIONS_AUTOMATION_ENABLED=true
```
