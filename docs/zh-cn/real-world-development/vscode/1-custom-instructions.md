---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解背景信息，就应确保它可以访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，用于指导代码生成。

本练习将学习：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 使用当前说明生成第一个筛选数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察生成代码如何遵循新标准。
- 将说明更新和筛选功能切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不完全符合设定的标准。Copilot 的结果具有非确定性。本练习旨在观察更新说明后行为的**整体趋势**，而不是逐字符匹配输出。

## 说明文件

### 场景

Tailspin Toys 团队有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释记录代码。
- 每个文件开头都应有注释块，说明文件用途。

通过说明文件，可以确保 Copilot 了解这些做法并遵循它们。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，使它更好地理解你的编码风格和要求。你可以指定代码规范、库以及注释偏好，也可以创建适用于整个存储库或特定文件类型的说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次处理此存储库的请求时都会发送给 Copilot 的单个说明文件。内容应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建，例如 TypeScript 或 Astro，也可以用于创建 UI 组件或一组单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会范围，但示例项目展示了常见做法：

- 让 `copilot-instructions.md` 专注于项目级指南，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持说明清晰，并提供代码应当和不应当采用的示例。

说明文件没有唯一的创建方法，AI 工具的使用方式也不止一种。可以通过实验找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。查看本项目中的说明文件时，可能会看到针对[界面更新][ui-instructions]和 [Astro][astro-instructions] 等多种任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应学习路径会在适当位置说明具体操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读存储库自带的说明文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。特别注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置相对于存储库根目录的 glob。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会应用该说明。
5. 查看项目中针对单元测试创建的说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这让你可以将大型说明拆分为较小、可复用的文件，并提供 Copilot 可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前没有要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个全新对话。模式和模型选择因练习而异，需要时会在具体步骤中说明。

要观察自定义说明的影响，先在使用当前说明文件的情况下发送提示。之后更新说明，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以直接修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有非确定性，因此即使没有明确要求，它也可能添加文档注释。如果出现这种情况也没关系，本练习仍然用于观察更新说明后的行为一致性提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准写入存储库，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。注意其中记录了项目编码标准，但尚未涉及代码文档，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中提供全局指南，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务添加说明。刚添加的文档注释规则属于项目级标准，因此应放在存储库级文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

现在已引导 Copilot 遵循新的项目标准，并将标准应用于真实代码，下一练习会继续使用这些代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础代码修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联待办事项中的文档标准议题。Copilot 已连接项目的 GitHub 工具，可以替你查找议题并创建关联 PR，再合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍此连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，描述说明更新和新的发行商辅助函数基础代码：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 请求你 **Allow** 其代表你执行 GitHub 操作时，先审查命令，再酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求你 **Allow** 其代表你执行 GitHub 操作和 shell 命令时，先审查命令，再酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你已了解 Copilot 如何从项目说明文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据**现有**说明生成内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和发行商辅助函数基础代码提交到 `main`，为下一练习做好准备。

接下来，在[智能体模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
