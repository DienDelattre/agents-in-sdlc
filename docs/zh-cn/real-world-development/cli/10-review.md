---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。本次你：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，再通过 Interactive 模式审查。
- 使用自定义说明引导智能体，自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付成果

研讨会包含两个 PR 阶段，每个阶段都从更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能，更新说明并将其应用到功能；自定义 `quality-checks` 报告；创建 QA 配置文件；并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化研讨会，我们将这部分工作合并到同一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并使用新的报告格式汇报结果。QA 汇总这些结果，并在 PR 前审查需求和测试覆盖率。

新添加的测试应弥补真实缺口；如果 QA 运行后无需添加测试，也可能是合理结果。授权合并前，请审查代码和证据。更改后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义功能，并根据团队需要进行调整。说明用于设定标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在开始构建前梳理方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可选用速度较快的模型，复杂工作则选择能力更强的模型。

上下文仍然与基础设施同样重要。清楚描述要构建的内容、原因和方式，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以了解以下 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：协调多个相互独立的子任务并行执行。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方式就是持续使用。可以用它编写生产代码、兴趣项目，或实现一直想做却还没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果要从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。如需比较其他环境，请查看 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
