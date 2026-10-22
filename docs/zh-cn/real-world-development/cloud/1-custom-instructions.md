---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解背景信息，就应确保这些上下文可以访问。[说明文件][instruction-files]可提供此类指导，让 Copilot 不仅了解你希望它做什么，也了解应该如何完成。

本练习将学习：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不比较更新前后的提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。稍后审查云智能体创建的拉取请求时，可以看到说明文件产生的影响。

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
3. 打开 `.github/instructions` 文件夹，查看其中的文件，包括针对 Astro、Drizzle 数据层和测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会应用该说明。
5. 查看项目中针对单元测试创建的说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这让你可以将大型说明拆分为较小、可复用的文件，并提供 Copilot 可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前没有要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习唯一需要编辑的内容：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后本学习路径中，云智能体处理分配的议题时会读取此规则。

编辑前先创建分支。（云智能体会从议题目标分支读取说明，但仍应单独提交本次更改。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中将从 `main` 分支向 Copilot 分配议题，因此必须先把说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体处理议题时会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本研讨会，可以跳过分支操作并直接提交到 `main`。保留分支步骤是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你已了解 Copilot 如何从项目说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，可以在云智能体生成的拉取请求里看到该标准的影响。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
