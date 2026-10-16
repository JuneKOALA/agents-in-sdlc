---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解某些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可提供这类指导，让 Copilot 不仅了解要完成**什么**，也了解**如何**完成。

本练习将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令和路径范围指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工具不同，这里不会使用前后对比提示词。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示效果。后续在此环境中审查 cloud agent 创建的拉取请求时，你会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 遵循一套开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码中应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应有一段注释，说明文件的作用。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些规范完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，使其更好地理解你的编码风格和要求。你可以指定编码约定、库，甚至偏好的注释方式，从而引导 Copilot 提供更符合需求的建议和代码片段。可以为整个存储库创建指令，也可以针对特定类型的文件创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，适用于此存储库中的**每个**请求。它应包含项目级信息，即与发给 Copilot 的大多数聊天或 CLI 请求相关的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件。可以提供 TypeScript 或 Astro 等特定语言的指南，也可以为创建 UI 组件或单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中使用时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 中的指令集中在项目级指导上，例如项目构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体指令。
- 使用自然语言，让指导清晰易懂，并提供代码示例说明应采用和避免的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会逐渐了解最适合项目和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目的文件时，你可能会发现针对多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的入口各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关界面的课程会在适当位置介绍这些入口。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含许多指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
## 探索项目中的自定义指令文件

花些时间阅读本存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。请特别留意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些指令适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包括 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意开头的 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式，用于决定指令适用的文件。此处，所有 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意项目中有关单元测试的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供代码生成示例。（这些路径是相对于指令文件的，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来将添加 TSDoc 文档注释和文件注释头的规则。
## 添加新的存储库标准

接下来是本课唯一需要编辑的内容：添加一项全项目规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续本环境中的练习会展示 cloud agent 如何在处理议题时使用这项规则。

编辑前，先创建分支（cloud agent 会读取议题目标分支上的指令，但仍然建议先提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约在第 27 行。留意其中记录了项目编码标准，但尚未要求在代码中添加文档，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。
## 提交、推送并合并指令更新

Cloud agent 会从议题目标分支读取指令。在下一课中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此指令更新必须先合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并将其合并。这样，cloud agent 在下一个练习中处理议题时就能读取这些指令。

> [!TIP]
> 如果你希望在本工作坊中直接使用 `main` 分支，可以跳过创建分支的步骤，直接提交到 `main`。此处的分支流程是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何获取项目中的指令文件上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来的练习中，你会看到 cloud agent 创建的拉取请求如何遵循该标准。

接下来，将[第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
