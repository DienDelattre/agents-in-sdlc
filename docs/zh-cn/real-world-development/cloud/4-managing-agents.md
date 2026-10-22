---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你已请 Copilot 云智能体处理三个改善用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看拥有未关闭拉取请求（PR）的智能体任务、探索已执行的操作，还可以指导正在进行的会话。

本课将学习：

- 使用智能体页面监控云智能体任务。
- 指导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到现在也是添加浅色模式的好时机。既然正在更新网站样式并添加切换功能，顺便加入这一功能也很合理。你将指导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前工作状态和采用的方法而异。

4. 注意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 指导云智能体

了解活动任务后，在智能体处理高对比度模式时，请它同时加入浅色模式切换。

1. 选择用于添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。看到类似下方的消息时，说明已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务。它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

这需要几分钟。你可以趁此机会暂停一下，回顾迄今为止学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和指导 GitHub Copilot 云智能体任务的中心。通过这个任务控制中心，可以跟踪所有活动和已完成的任务、审查正在执行的工作，还能重定向进行中的任务以调整范围或提供额外指导。

本课探索了：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
