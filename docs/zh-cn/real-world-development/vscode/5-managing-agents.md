---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在 Copilot 智能体模式下，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在本地实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中查看每项差异、接受或拒绝单项更改，并通过后续提示指导对话，在不离开编辑器的情况下改进或扩展工作。

本练习将学习：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中指导智能体，在高对比度模式之外添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一个请求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换。你无需从头创建新对话，而是指导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每一步，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会在聊天中以内嵌差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时滚动聊天，查看工具调用。你会看到文件读取、文件编辑和请求执行的终端命令。
3. 智能体建议编辑文件时，差异会以内嵌方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 还可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式的实现。它应会发送一条最终消息，总结所做的更改。

## 指导智能体添加浅色模式

高对比度模式已就绪，现在在同一对话中添加浅色模式切换。由于继续使用同一聊天会话，智能体会保留之前工作的所有上下文，包括涉及的组件、样式方案和 local storage 模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会从之前的工作继续，探索已有更改并提出浅色模式的新增内容。
3. 新编辑出现时逐项审查，并像处理高对比度模式那样选择 **Keep** 或 **Undo**。
4. 如果智能体采取了不符合预期的方向（例如以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止它，再发送澄清提示。

> [!TIP]
> 指导进行中的会话比重新开始更快。可以通过后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板中”）或扩展范围（“为两个切换开关都添加键盘快捷键”）。

## 审查工作集

提交更改前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改文件。应该能看到 Astro 组件、样式和相关测试等内容。
3. 打开几个已更改文件并检查差异，确认自定义智能体中的无障碍模式已得到遵循，包括 ARIA 属性、键盘导航、语义 HTML 和 local storage 持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并指导了正在进行的会话：审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课介绍了：

- 实时监控智能体工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示指导正在进行的会话。

接下来，[将无障碍工作发布为拉取请求][next-lesson]，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
