# LingXi 组织 Discussions 使用说明

LingXi 使用 Organization Discussions 作为跨仓库社区入口。需求讨论、每周共建、成果提交、技术文章和官方公告在这里进行；具体实现任务仍在目标仓库的 Issue 与 Pull Request 中跟踪。

## 语言与技术标识约定

社区界面、表单字段、机器人回复和文档统一使用中文。

以下内容保持英文：

- 分类 slug
- Discussion Form 文件名
- 维护命令
- 状态标签
- 仓库名、环境变量等技术标识

**重要：分类显示名可以是中文，但 slug 必须保持下表中的英文值。** 已通过实际 A/B 测试确认，中文 slug 会导致 GitHub Discussion Category Form 无法正常渲染结构化表单。

## 来源仓库

- 来源仓库：`LingXi-Org/.github`

## 分类结构

| 层级 | 中文显示名称 | 格式 | slug |
| --- | --- | --- | --- |
| 分类 | `公告` | Announcement | `announcements` |
| 分类 | `想法与需求` | Open-ended | `ideas-and-requests` |
| Section | `共建实验室` | — | — |
| 共建实验室内分类 | `每周任务` | Announcement | `weekly-missions` |
| 共建实验室内分类 | `任务提交` | Open-ended | `submissions` |
| 分类 | `技术札记` | Open-ended | `engineering-notes` |

对应的 Discussion Form 文件：

- `.github/DISCUSSION_TEMPLATE/announcements.yml`
- `.github/DISCUSSION_TEMPLATE/ideas-and-requests.yml`
- `.github/DISCUSSION_TEMPLATE/weekly-missions.yml`
- `.github/DISCUSSION_TEMPLATE/submissions.yml`
- `.github/DISCUSSION_TEMPLATE/engineering-notes.yml`

不要把这些文件名改成中文，也不要把分类 slug 改成中文。

## 想法与需求

社区成员通过“想法与需求”提交真实使用场景、当前问题、期望结果和参与意愿。

创建后：

1. 机器人添加 `proposal:review`。
2. 机器人 `@leecyang` 请求审批。
3. 维护者审核目标仓库与需求边界。
4. 批准后添加 `proposal:approved`，并生成对应仓库的预填 Issue 创建入口。
5. Issue 由需求提出者本人提交。

维护者命令：

```text
/approve
/approve LingxiSkills
/decline
```

状态标签：

```text
proposal:review
proposal:approved
proposal:declined
```

## 共建实验室

### 每周任务

维护者使用“每周任务”发布具有明确交付物和验收标准的周期性任务。

创建后自动添加：

```text
mission:open
```

任务结束时回复：

```text
/close-mission
```

状态切换为：

```text
mission:closed
```

### 任务提交

参与者应为每份成果创建独立 Discussion，并关联对应每周任务。

创建后自动添加：

```text
submission:received
```

维护者评审命令：

```text
/accept
/needs-work
```

对应状态标签：

```text
submission:accepted
submission:needs-work
```

## 技术札记

“技术札记”用于沉淀架构、工程、研究与实验文章。发布后机器人添加：

```text
engineering-note
```

## 公告

“公告”用于官方项目总览、版本、软件包、新仓库、开发进展和重要变更。人工公告和自动公告均使用中文内容。

机器人使用标签：

```text
announcement
```

## 社区内容发布

`.github/workflows/community-publisher.yml` 提供：

```text
bootstrap
sync
digest
```

定时任务只有在以下 Actions variable 为 `true` 时才启用：

```text
DISCUSSIONS_AUTOMATION_ENABLED=true
```

当前若不希望自动发布，不设置该变量即可。手动运行不受影响。

组织级软件包读取为可选能力，需要时使用独立 Secret：

```text
ORG_PACKAGE_TOKEN
```

不得复用 `ORG_INVITE_TOKEN`。

## 安全与权限

Discussion → Issue 流程不使用跨仓库写入 Token。机器人只生成预填 Issue 链接，由需求提出者本人提交目标仓库 Issue。

默认权限保持最小化：

- `contents: read`
- `discussions: write`
- `issues: write`
- `packages: read`（仅社区内容发布工作流）
