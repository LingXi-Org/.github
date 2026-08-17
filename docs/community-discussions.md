# LingXi 组织 Discussions 使用说明

LingXi 使用 Organization Discussions 作为跨仓库社区入口。需求讨论、每周共建、成果提交、技术文章和官方公告在这里进行；具体实现任务仍应在目标仓库的 Issue 与 Pull Request 中跟踪。

> 命令、标签名、仓库名、文件名和分类 slug 属于技术标识，继续保留英文；其余面向社区的内容统一使用中文。

## 来源仓库

启用 Organization Discussions 时使用：

- **来源仓库：** `LingXi-Org/.github`

组织 Discussions 的分类、模板和自动化均由该仓库承载。

## 分类结构

推荐的中文显示名称如下。底层 slug 保持现有英文值，不应随意修改。

| 层级 | 中文名称 | 格式 | 用途 | slug |
| --- | --- | --- | --- | --- |
| 分类 | `公告` | Announcement | 官方项目总览、版本、软件包、新仓库与开发进展 | `announcements` |
| 分类 | `想法与需求` | Open-ended | 产品、Agent、Skill 和生态需求 | `ideas-and-requests` |
| Section | `共建实验室` | — | 每周共建与成果提交入口 | — |
| 共建实验室内分类 | `每周任务` | Announcement | 维护者发布周期性工程或研究任务 | `weekly-missions` |
| 共建实验室内分类 | `任务提交` | Open-ended | 参与者提交独立成果 | `submissions` |
| 分类 | `技术札记` | Open-ended | 架构、工程、研究和实验文章 | `engineering-notes` |

对应的 Discussion Form 文件：

- `.github/DISCUSSION_TEMPLATE/announcements.yml`
- `.github/DISCUSSION_TEMPLATE/ideas-and-requests.yml`
- `.github/DISCUSSION_TEMPLATE/weekly-missions.yml`
- `.github/DISCUSSION_TEMPLATE/submissions.yml`
- `.github/DISCUSSION_TEMPLATE/engineering-notes.yml`

如果 GitHub 在修改分类显示名称时改变了 slug，必须同步修改对应模板文件名和工作流中的 slug 判断；不要让分类名称调整破坏已经运行的自动化。

## 想法与需求

社区成员通过“想法与需求”提交真实使用场景、当前问题、期望结果和参与意愿。

新需求创建后：

1. 机器人添加 `proposal:review`。
2. 机器人在 Discussion 中 `@leecyang` 请求审批。
3. 维护者根据需求边界和目标仓库进行审核。
4. 批准后机器人添加 `proposal:approved`，并为需求提出者生成目标仓库的预填 Issue 创建入口。
5. Issue 由需求提出者本人提交，后续实现和 Pull Request 跟踪转移到目标仓库。

维护者命令：

```text
/approve
/approve LingxiSkills
/decline
```

其中：

- `/approve`：使用表单中选择的目标仓库。
- `/approve <Repository>`：批准并覆盖目标仓库，例如 `/approve LingxiGraph`。
- `/decline`：当前不进入工程实现。

支持的目标仓库：

- `LingxiLearn`
- `LingxiGraph`
- `LingxiSkills`
- `LingxiIdentity`
- `LingxiNext`

用户选择“组织级 / 暂不确定”时，维护者必须使用 `/approve <Repository>` 明确目标仓库。

使用的状态标签：

```text
proposal:review
proposal:approved
proposal:declined
```

## 共建实验室

### 每周任务

维护者使用“每周任务”发布具有明确交付物和验收标准的周期性任务。

每个任务应包含：

- 连续任务编号，例如 `W01`
- 目标
- 背景
- 交付物
- 验收标准
- 参考资料
- 建议截止时间
- 进阶挑战（可选）

创建后自动添加：

```text
mission:open
```

任务结束时，维护者回复：

```text
/close-mission
```

机器人将状态切换为：

```text
mission:closed
```

### 任务提交

参与者应为每份成果创建独立 Discussion，不要将完整成果堆叠在每周任务的评论区。

提交时必须填写：

- 对应每周任务的 Discussion 链接
- 目标仓库
- Pull Request、仓库或演示链接
- 完成内容
- 设计与实现
- 验证证据
- 复盘（可选）

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

“技术札记”用于沉淀可长期检索的工程经验、架构设计、实验结论和研究思考。

支持的主题包括：

- 架构设计
- 智能体运行时
- 技能体系
- 前端工程
- 研究与实验
- 基础设施
- 其他

文章应至少包含摘要、正文和必要参考资料。发布后机器人添加：

```text
engineering-note
```

## 公告

“公告”用于组织级官方信息，适合发布：

- 项目总览
- 版本发布
- 软件包发布
- 开发进展
- 新仓库
- 重要变更

人工发布时应说明发生了什么、为什么开发者需要关注，以及必要的迁移、兼容性或后续操作。

机器人创建的官方公告使用：

```text
announcement
```

## 社区内容发布工作流

`.github/workflows/community-publisher.yml` 提供以下手动动作：

```text
bootstrap
sync
digest
```

- `bootstrap`：发布或初始化组织总览、各仓库最新版本和当前开发快照。
- `sync`：同步最近 48 小时的新仓库、版本和可访问的软件包。
- `digest`：发布最近 7 天的开发摘要。

定时任务只有在 Actions variable 设置为以下值后才会执行：

```text
DISCUSSIONS_AUTOMATION_ENABLED=true
```

当前如果不希望自动发布，不要设置该变量即可。手动运行不受该变量限制。

组织级 GHCR 软件包读取是可选能力。默认 `GITHUB_TOKEN` 无法读取时，工作流只跳过软件包公告，不影响其他内容。确需启用时使用独立 Secret：

```text
ORG_PACKAGE_TOKEN
```

不得复用 `ORG_INVITE_TOKEN`。

## 安全与权限

Discussion → Issue 流程不会使用跨仓库写入 Token。机器人只生成预填 Issue 链接，由需求提出者本人提交目标仓库 Issue。

默认工作流权限保持最小化：

- `contents: read`
- `discussions: write`
- `issues: write`
- `packages: read`（仅社区内容发布工作流）

组织邀请凭据、软件包读取凭据和社区内容自动化必须保持权限隔离。
