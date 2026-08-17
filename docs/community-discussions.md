# LingXi 组织 Discussions 与开发治理说明

LingXi 使用 Organization Discussions 作为跨仓库社区入口。需求讨论、每周共建、成果提交、技术文章和官方公告在这里进行；具体实现任务仍在目标仓库的 Issue 与 Pull Request 中跟踪。

## 语言与技术标识约定

社区界面、表单字段、机器人回复和文档统一使用中文。分类 slug、Discussion Form 文件名、维护命令、状态标签、仓库名和环境变量保持英文。

**分类显示名可以是中文，但 slug 必须保持英文。** 中文 slug 已通过实际 A/B 测试确认会导致 GitHub Discussion Category Form 无法正常渲染结构化表单。

## 来源仓库与分类

来源仓库：`LingXi-Org/.github`

| 中文显示名称 | 格式 | slug |
| --- | --- | --- |
| `公告` | Announcement | `announcements` |
| `想法与需求` | Open-ended | `ideas-and-requests` |
| `共建实验室` | Section | — |
| `每周任务` | Announcement | `weekly-missions` |
| `任务提交` | Open-ended | `submissions` |
| `技术札记` | Open-ended | `engineering-notes` |

对应表单文件保持英文文件名：`announcements.yml`、`ideas-and-requests.yml`、`weekly-missions.yml`、`submissions.yml`、`engineering-notes.yml`。

## Team 权限模型

LingXi 使用两个 Team：

- `Mentors`（slug：`mentors`）：导师与治理者。人数不限。
- `Builders`（slug：`builders`）：参与每周任务和项目共建的成员。

`leecyang` 是 Organization Owner，并始终具有 Mentor 治理权限；为了组织结构清晰，也建议加入 `Mentors` Team。

治理规则：

- `leecyang` 自己的 PR：无需任何人工审批，`PR 审批门禁` 自动通过。
- 其他 `Mentors` 成员的 PR：只需要 `leecyang` 一人 Approve。
- `Builders` 或外部贡献者的 PR：需要任意 2 名 Mentors Approve；`leecyang` 可以计入其中 1 人。
- Discussion 需求审批、Issue 审批、每周任务发布、任务关闭和任务提交验收：任意 1 名 Mentor 即可。
- Team 成员变化无需修改代码或用户名变量。

PR 审批只统计当前 PR head SHA 上的有效 Approve；提交新 commit 后需要按当前代码重新满足门禁。

## 自动通知

工作流会自动通知对应角色：

- 新“想法与需求” → `@LingXi-Org/mentors`
- 新开发 Issue → `@LingXi-Org/mentors`
- Mentor PR → 自动请求并 `@leecyang`
- Builder / 外部贡献者 PR → 自动请求 `Mentors` Team Review 并 `@LingXi-Org/mentors`
- 新“每周任务” → `@LingXi-Org/builders`
- 新“任务提交” → `@LingXi-Org/mentors`

## 想法与需求 → Issue

创建需求后机器人添加 `proposal:review` 并通知 Mentors。任意 Mentor 可以使用：

```text
/approve
/approve LingxiSkills
/decline
```

批准后添加 `proposal:approved`，机器人生成目标仓库的预填 Issue 创建入口。由该入口创建的 Issue 会自动识别为已审批，无需第二次审批。

## 直接创建的开发 Issue

直接在代码仓库中新建的 Issue 会添加 `issue:review` 并通知 Mentors。任意 Mentor 可以使用：

```text
/approve-issue
/decline-issue
```

对应状态：`issue:approved` / `issue:declined`。

## 每周任务

每周任务是开放式作业，不要求一定写代码。任务可以要求：

- 文档 / 报告
- 外部链接 / 作品
- Engineering Notes 技术札记
- PR
- 实验 / Demo
- 直接参与指定 Issue
- 无需独立提交
- 不限形式

只有 Owner 或 Mentors 成员应发布每周任务；`.github` 来源仓库应给 `Mentors` Team `Maintain` 权限。任务发布后自动 `@LingXi-Org/builders`。

任务结束时任意 Mentor 可以回复：

```text
/close-mission
```

## 任务提交

仅当任务要求独立提交时使用“任务提交”。成果可以直接写文档正文，也可以提供外部链接、Engineering Notes、PR、Issue 参与结果、Demo、仓库等。

任意 Mentor 可以回复：

```text
/accept
/needs-work
```

## Team membership 读取凭据

仓库自带 `GITHUB_TOKEN` 可以进行 PR/Issue/Discussion 写操作，但不能读取 Organization Team 成员，因此需要独立 Secret：

```text
ORG_GOVERNANCE_TOKEN
```

该凭据只用于读取 `Mentors` Team membership。真正的评论、标签、Reviewer 请求仍使用每个仓库自己的 `GITHUB_TOKEN`。

不要复用 `ORG_INVITE_TOKEN`；邀请成员与治理 Team membership 是两条独立权限链。

## 社区内容发布

`.github/workflows/community-publisher.yml` 提供 `bootstrap`、`sync`、`digest`。定时任务仅在以下变量为 `true` 时启用：

```text
DISCUSSIONS_AUTOMATION_ENABLED=true
```

若不希望自动发布，不设置该变量即可。组织级软件包读取仍使用独立 `ORG_PACKAGE_TOKEN`，不得复用其他 Token。
