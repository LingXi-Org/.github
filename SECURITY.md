# Security Policy

LingXi 重视运行时、凭据和用户数据安全。请负责任地私下报告潜在漏洞。

## Supported versions

我们优先为各仓库默认分支和最新正式版本提供安全修复。旧版本可能需要先升级才能获得修复。

| Version | Supported |
| --- | --- |
| Default branch / latest release | Yes |
| Older unreleased commits | Best effort |
| End-of-life versions | No |

## Reporting a vulnerability

请勿为未修复漏洞创建公开 Issue、Discussion 或 Pull Request。

1. 打开受影响的 LingXi 仓库。
2. 进入 **Security → Report a vulnerability**，提交私有漏洞报告。
3. 提供受影响版本、攻击前置条件、复现步骤、影响评估和建议缓解措施。
4. 删除或替换真实 Token、个人数据和第三方机密。

维护者会尽快确认报告、协调修复与披露时间。在公开修复前，请避免传播利用细节。若仓库尚未启用私有漏洞报告，请通过 GitHub 联系组织维护者，并只发送建立私密沟通所需的最少信息。

## Scope

通常属于范围：

- 绕过认证、授权、租户或模板安全边界；
- 凭据、checkpoint、审计数据或用户数据泄露；
- 可导致远程代码执行、任意文件访问或持久化篡改的问题；
- 可由不可信输入触发的严重拒绝服务。

通常不属于范围：

- 仅影响已停止支持版本的问题；
- 不包含安全影响的依赖版本建议；
- 社会工程、物理攻击或对第三方服务的测试；
- 未经授权的生产环境扫描或压力测试。
