# LingXi Organization Discussions

Organization Discussions 是 LingXi 的跨仓库社区入口。具体实现任务仍应在目标仓库的 Issue 和 Pull Request 中跟踪。

## Source repository

启用 Organization Discussions 时必须选择：

- **Source repository:** `LingXi-Org/.github`

组织 Discussion 的权限、分类和自动化均由该仓库承载。

## Required structure

请使用以下名称创建 Section 和 Categories。Discussion Form 文件名依赖 Category slug，因此不要随意改变英文名称。

| Level | Name | Format | Purpose |
| --- | --- | --- | --- |
| Category | `Announcements` | Announcement | 官方项目总览、Release、Package、新仓库与开发进展 |
| Category | `Ideas and Requests` | Open-ended | 需求、产品想法和跨仓库建议 |
| Section | `Build Lab` | — | 共建任务入口 |
| Category in Build Lab | `Weekly Missions` | Announcement | 维护者发布每周工程或研究任务 |
| Category in Build Lab | `Submissions` | Open-ended | 参与者提交独立成果 |
| Category | `Engineering Notes` | Open-ended | 架构、工程、研究和实验文章 |

期望 slug：

- `announcements`
- `ideas-and-requests`
- `weekly-missions`
- `submissions`
- `engineering-notes`

若 GitHub 实际生成的 slug 不同，应同步重命名 `.github/DISCUSSION_TEMPLATE/*.yml`，并修改 `.github/workflows/*.yml` 中对应的 slug。

## Ideas and Requests workflow

新需求创建后：

1. 机器人添加 `proposal:review`。
2. 机器人在 Discussion 中 `@leecyang` 请求审批。
3. 维护者回复：
   - `/approve`：按表单中的目标仓库批准。
   - `/approve LingxiSkills`：批准并覆盖目标仓库。
   - `/decline`：拒绝进入工程 Issue。
4. 批准后机器人添加 `proposal:approved`，并为需求提出者生成目标仓库的预填 Issue 链接。
5. Issue 由需求提出者本人创建，因此作者、通知和后续协作关系保持正确。

支持的目标仓库：

- `LingxiLearn`
- `LingxiGraph`
- `LingxiSkills`
- `LingxiIdentity`
- `LingxiNext`

若用户选择 `Organization / Not sure`，审批时必须使用 `/approve <Repository>` 明确目标仓库。

## Build Lab workflow

### Weekly Missions

每个 Mission 应包含：

- 连续任务编号，例如 `W01`
- Objective
- Context
- Deliverables
- Acceptance criteria
- References
- Suggested deadline
- Bonus（可选）

创建后自动添加 `mission:open`。任务结束时维护者回复：

```text
/close-mission
```

机器人将状态切换为 `mission:closed`。

### Submissions

参与者必须为每份成果创建独立 Discussion，并填写：

- 对应 Mission Discussion URL
- 目标仓库
- PR、仓库或 Demo URL
- 完成内容
- 设计与实现
- 验证证据
- 复盘（可选）

创建后自动添加 `submission:received`。

维护者可回复：

```text
/accept
```

或：

```text
/needs-work
```

机器人分别添加 `submission:accepted` 或 `submission:needs-work`。

## Announcements automation

`.github/workflows/community-publisher.yml` 提供三种手动动作：

- `bootstrap`：发布组织技术栈总览、各仓库最新 Release、当前可见 GHCR Package，以及最近 30 天开发快照。
- `sync`：同步最近 48 小时的新仓库、Release 和 GHCR Package。
- `digest`：发布最近 7 天已合并 PR 的开发摘要。

定时任务：

- 每 6 小时检查新仓库、Release 和 GHCR Package。
- 每周日 12:00 UTC 发布 Weekly Development Digest。

为了避免在 Discussions 尚未配置完成时产生失败的定时任务，定时发布只有在仓库 Actions variable 设置为以下值后才会执行：

```text
DISCUSSIONS_AUTOMATION_ENABLED=true
```

手动 `workflow_dispatch` 不受该变量限制，可用于首次验证。

## Bootstrap order

首次启用时按以下顺序操作：

1. 在 Organization Settings 中启用 Discussions。
2. Source repository 选择 `LingXi-Org/.github`。
3. 按本文创建 Section 和 Categories，并确认 slug。
4. 合并包含 Discussion Forms 和 workflows 的变更到 `.github` 默认分支。
5. 在 Actions → `Community Publisher` 手动运行 `bootstrap`。
6. 检查自动生成的组织总览、Release、Package 和 Development Snapshot。
7. 将 `Welcome to LingXi — Architecture, Projects & Current Status` 置顶。
8. 设置 Actions variable `DISCUSSIONS_AUTOMATION_ENABLED=true`，开启定时同步。
9. 创建一个测试 `Ideas and Requests`，验证 `@leecyang`、`/approve` 和 Issue 跳转。
10. 创建一个测试 Mission 和 Submission，验证状态命令。

## Security and permissions

Discussion → Issue 流程不会使用跨仓库写入 Token。机器人只生成预填 Issue 链接，由需求提出者本人提交目标仓库 Issue。

默认 `GITHUB_TOKEN` 仅需要：

- `discussions: write`
- `issues: write`（用于维护 source repository 的共享 labels）
- `contents: read`
- `packages: read`（用于读取可见 GitHub Packages）

不得把 `ORG_INVITE_TOKEN` 用于 Discussion 自动化。组织邀请凭据与社区内容自动化必须保持权限隔离。
