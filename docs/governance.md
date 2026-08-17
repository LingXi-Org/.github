# LingXi 三人治理说明

LingXi 使用一名组织管理员与两名导师组成治理小组。面向社区的内容使用中文；命令、标签、仓库名和 Discussion slug 保留英文。

## 治理角色

- 管理员：`@leecyang`
- 导师一：组织 Actions Variable `MENTOR_ONE`
- 导师二：组织 Actions Variable `MENTOR_TWO`
- 导师 Team：建议 slug `mentors`
- 共建成员 Team：建议 slug `builders`

组织 Actions Variables：

```text
MENTOR_ONE=<mentor-one-login>
MENTOR_TWO=<mentor-two-login>
MENTOR_TEAM=mentors
MISSION_AUDIENCE_TEAM=builders
```

## 每周任务

每周任务由管理员或两位导师发布。任务可以是：

- 文档 / 报告
- 外部链接 / 作品
- 技术札记（Engineering Notes）
- 代码 / Pull Request
- 实验 / Demo
- 直接参与指定 Issue
- 无需独立提交
- 不限形式

任务发布后会自动 `@LingXi-Org/builders`（或 `MISSION_AUDIENCE_TEAM` 指定的 Team）。

如果任务选择“需要”独立提交，参与者在“任务提交”分类提交成果。成果可以直接填写正文，也可以只提供链接。三名治理者中的任意一人可以：

```text
/accept
/needs-work
```

任务结束时三人任意一人可以：

```text
/close-mission
```

## 开发需求与 Issue

“想法与需求”中的开发需求由三名治理者中的任意一人批准：

```text
/approve
/approve LingxiSkills
/decline
```

批准后生成的预填 Issue 带有治理标记，创建后自动获得 `issue:approved`，不需要再次审批。

直接在代码仓库新建的开发 Issue 会获得 `issue:review`。三人任意一人可以：

```text
/approve-issue
/decline-issue
```

## Pull Request 审批规则

组织共享 PR 门禁按作者身份动态计算审批人：

| PR 作者 | 必需审批 |
| --- | --- |
| `leecyang` | 无，治理门禁自动通过 |
| 导师一 | `leecyang` + 导师二 |
| 导师二 | `leecyang` + 导师一 |
| 其他成员 | `leecyang` + 导师一 + 导师二 |

门禁只统计当前 PR `head SHA` 上的 `APPROVED` Review。PR 新增提交后，旧提交上的批准不会计入当前门禁。

每个代码仓库通过 `.github/workflows/lingxi-governance.yml` 调用组织共享 Action。管理员需要在仓库 `main` 的 Ruleset 或 Branch protection 中把 `PR 审批门禁` 设置为 required status check。

## 推荐 Team 权限

- `Mentors` (`mentors`)：
  - `.github`：Maintain，用于发布 Announcement 格式的每周任务并管理 Discussions。
  - 代码仓库：Write，用于正式 `APPROVE` / `REQUEST_CHANGES` PR Review。
- `Builders` (`builders`)：按实际开发方式分配 Read/Write；Team 主要用于每周任务通知。

不要把 `Mentors` 加入 `main` 保护规则的 bypass list。
