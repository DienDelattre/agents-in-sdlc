---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，就应让 Copilot 获得相应指导。[说明文件][instruction-files]不仅描述需要编写什么代码，也描述代码应如何组织。现在筛选功能已完成，你将探索 Copilot 使用的说明，添加文档标准，并将其应用于代码。

本课将学习：

- 探索存储库说明和路径范围说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 团队有一套开发实践指南和要求：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应记录其 `Props` 契约；相关代码变更时，注释也应保持最新。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 按照这些实践完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，使其更好地理解你的编码风格和要求。你可以说明代码规范、库以及注释偏好，也可以创建适用于整个存储库或特定文件类型的说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次处理此存储库的请求时，都会发送给 Copilot 的单个说明文件。内容应与大多数请求相关，描述项目级要求。
- `.github/instructions/*.instructions.md`：适用于特定语言、文件类型或任务的指南。

> [!NOTE]
> 不同环境对其他说明格式的支持可能不同。依赖特定格式之前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

起始项目已包含一组说明文件。先了解现有内容，再进行更改并查看效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指南。这些说明适用于与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。这些说明覆盖 Astro 文件、Drizzle 数据层、测试等内容。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它通过 glob 确定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为小型可复用文件，并向 Copilot 提供可参考的示例。

## 更新说明文件以符合团队指南

现有文件已经提供了良好起点，但仍有不足。现在修改核心的 `copilot-instructions.md`，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。本课中，你：

- 探索了存储库说明和路径范围说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，以便一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
