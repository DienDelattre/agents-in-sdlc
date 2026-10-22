---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要准备好所有内容。你将创建 Tailspin Toys 存储库副本，并启动一个 [codespace][codespaces]，用于编辑说明文件和审查云智能体生成的工作。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的全部文件，后续练习都将在其中完成。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在拥有 GitHub Copilot 访问权限的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下刚创建的存储库路径（**organization-or-user-name/repository-name**），后续练习会用到。

## 创建 codespace

接下来，使用 codespace 完成本实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建过程需要几分钟，但仍比手动安装所有服务快得多。你可以利用这段时间探索 GitHub Copilot 的其他功能，后续会介绍这些内容。

> [!CAUTION]
> 后续练习还会用到这个 codespace。现在先让它保持打开状态，并留在浏览器标签页中。

> [!NOTE]
> 本研讨会可在 codespace 或本地[开发容器][dev-containers]中运行。两者都确保环境中已安装所有必需项。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**，VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜你创建了实验存储库副本！你也已开始创建 codespace，后续将与 Copilot 云智能体协作时使用它。

## 下一步

接下来添加云智能体会遵循的自定义说明。继续学习[练习 1：自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
