<div align="center">

# LingXi · 灵犀

**Open-source infrastructure and learning experiences for reliable AI products.**

让 AI 不只回答问题，而是理解状态、调用能力、持续执行，并根据结果决定下一步。

**Everything is a Skill. State decides next.**

[灵犀智学 LingxiLearn](https://github.com/LingXi-Org/LingxiLearn) ·
[项目列表](https://github.com/orgs/LingXi-Org/repositories) ·
[Join LingXi / 加入我们](https://github.com/LingXi-Org/.github/issues/new?template=join-lingxi.yml)

</div>

## What we build

LingXi 是一个围绕 **AI 学习产品、Agent 运行时、可复用能力、身份基础设施与受控 Agent 应用** 构建的开源项目组。

我们关注的不只是模型本身，而是如何把 Agent 可靠地交付给真实用户：让目标、状态、Skill、工具、执行过程和可验证结果形成完整闭环。

## Projects

| Project | Role |
| --- | --- |
| **[LingxiLearn](https://github.com/LingXi-Org/LingxiLearn)** | 面向个人学习任务的 AI 学习工作台，通过状态驱动的多 Agent 编排持续调整学习体验。 |
| **[LingxiGraph](https://github.com/LingXi-Org/LingxiGraph)** | 生产级 Agent 图运行时，提供状态图、持久化、恢复、流式事件与服务化执行能力。 |
| **[LingxiSkills](https://github.com/LingXi-Org/LingxiSkills)** | 可组合的 Agent Skills 能力库，提供教学、评测、可视化、学习状态等标准化能力。 |
| **[LingxiIdentity](https://github.com/LingXi-Org/LingxiIdentity)** | LingXi 系列统一身份认证与用户管理服务，提供 OIDC / OAuth2 / JWT 与 BFF 会话边界。 |
| **[LingxiNext](https://github.com/LingXi-Org/LingxiNext)** | 面向 LingxiGraph 的安全、版本化多 Agent 应用层，以 Chainlit 提供交互入口，并负责受控编排、Revision 发布、会话绑定与审计。 |

## The LingXi stack

```text
                    LingxiLearn
                AI learning product
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
   LingxiIdentity  LingxiGraph  LingxiSkills
      Identity       Runtime     Capabilities
                         │
                         ▼
                 Models · Tools · Data

                   LingxiNext
             Versioned Agent App
                         │
                         └──────► LingxiGraph
```

核心思路很简单：

> **Skill 定义系统能够做什么，State 决定此刻应该做什么。**

## Join LingXi

我们欢迎对 **AI Learning、Agent、Runtime、Skills、Frontend、Infrastructure、Design** 感兴趣的开发者参与。

不需要长篇自我介绍。选择一个你感兴趣的方向，在这里开始：

**[Join LingXi / 加入我们 →](https://github.com/LingXi-Org/.github/issues/new?template=join-lingxi.yml)**

也可以直接浏览 [LingXi 项目列表](https://github.com/orgs/LingXi-Org/repositories)，或向任意项目提交 Pull Request。

---

<div align="center">

**Build AI systems that understand, adapt, and keep working.**

</div>
