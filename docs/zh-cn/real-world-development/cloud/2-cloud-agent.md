---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都面临技术债务问题，包括尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期留在待办列表中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会生成一个草稿 PR，供开发人员审查。这样可以委派繁琐任务、加快开发流程，让开发人员专注于更重要的工作。

本练习将探索云智能体的以下方面：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初受聘创建网站的承包商没有留下足够的文档。团队首先希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是同类别中的其他游戏，以便用户继续浏览。团队暂时不需要精致的实现，只需要能够进行 UX 验收测试的版本。目前这项工作被阻塞，但优先级更高的任务还在等待处理。

这两项工作都可能很快被降低优先级，因此适合分配给 Copilot 云智能体。云智能体可以异步处理，让开发人员先完成其他任务，稍后再回来审查其工作。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台完成任务。例如，可以[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看 Copilot 的工作。解决方案准备好供你审查时，它会在拉取请求中提醒你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看起来简单的操作，深入分析后也会变得复杂，因此请注意[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]。将其视为 AI 结对程序员：分阶段开展工作、学习、实验并逐步调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习已介绍自定义说明以及它们如何引导 Copilot。如果之前完成过自定义说明练习，应该已经看到 `.github/copilot-instructions.md` 的作用；否则现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读检查存储库中已有的说明文件，稍后就能识别它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明应用于 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头；这些要求来自说明文件。稍后的练习中还会再次检查这些内容。

## 为 Copilot 云智能体设置开发环境

编写代码通常需要特定环境和设置脚本，确保工作顺利进行。把任务分配给类似软件工程师的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions] 设置工作环境。你可以创建特殊的设置工作流，并配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行。这可以安装所需的开发工具和依赖项。本实验已预先配置此工作流，确保 Copilot 能使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用单独的令牌执行自己的操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流文件相似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会执行此作业。
- 工作流使用 `workflow_dispatch` 触发器，可以从存储库的 **Actions** 选项卡手动运行。这便于先确认工作流运行正常，而不必等待 Copilot 启动它。

## 添加文档

许多项目的文档已经过时或完全缺失，这类技术债务会降低效率，使代码库更难维护，也让新成员更难加入。Copilot 很适合创建文档，因此这是适合分配给云智能体的议题。它会在后台生成所需文档，后续练习中再回来审查。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的拉取请求（PR），标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定解决问题的方案。

13. 查看待办列表及其计划执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并确定合适的方案，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一就是异步处理任务，让我们能专注于其他工作。接下来利用这一点，再给 Copilot 分配另一个任务，同时继续处理应用程序功能。

## 在游戏详情页添加相关游戏部分

Copilot 云智能体的优势之一是可以拆分工作：你处理一组任务时，它可以同时处理另一组。为设计团队添加相关游戏部分可能并不需要很长时间，但这些时间仍可用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论上会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求。云智能体的工作方式类似软件工程师，不需要持续监控；任务完成后再审查即可。现在继续了解和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理技术债务、新功能或框架迁移等任务。

本课探索了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建并使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可以使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
