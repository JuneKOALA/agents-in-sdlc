---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解某些背景信息，就应确保它能够获取这些上下文。有多种方式可以向 Copilot 提供特定上下文，其中重要的一种是[指令文件][instruction-files]，它们可以为代码生成提供指导。

本练习将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义指令和路径范围指令文件传递给 Copilot。
- 在现有指令生效的情况下，生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的某些标准。Copilot 具有非确定性。本练习旨在展示更新指令后行为变化的趋势，而不是逐字匹配生成结果。

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
> Copilot 也可以帮助生成指令文件。不同界面提供的入口各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前界面的课程会在适当位置介绍这些入口。
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
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式，用于决定指令适用的文件。此处，所有 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意项目中有关单元测试的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供代码生成示例。（这些路径是相对于指令文件的，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来将添加 TSDoc 文档注释和文件注释头的规则。
## 为更改创建分支

按照最佳实践，为更改创建分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新的对话。各练习对模式和模型的要求可能不同；每个步骤都会在相关位置说明。

要了解自定义指令的影响，先在当前指令文件生效的情况下发送提示。之后更新指令，再次发送相同提示，以观察差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，这样 Copilot 就可以修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件和测试文件。
6. 留意建议的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这是 `.github/instructions/drizzle.instructions.md` 中的数据层约定起作用的结果（该文件适用于 `src/lib/*.ts`）。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使未明确要求，也有可能添加文档注释。如果发生这种情况也没关系；本练习的重点是观察更新指令后行为的一致性变化。

## 添加新的存储库标准

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
## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空对话。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在生成的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将它应用到了后续练习会用到的实际代码中。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，需要使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已包含：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础实现修改的每个文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到积压工作中的文档标准议题。也可以手动操作，不过 Copilot 已连接到项目的 GitHub 工具，因此可以让它代劳。提示 Copilot 查找议题、创建 PR 来关闭议题，然后合并 PR。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已在工作区中配置好 GitHub 工具，无需额外设置。Copilot 首次使用相关工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习中会介绍这种连接如何通过模型上下文协议实现。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 提示你允许它代表你执行 GitHub 操作时，请检查操作内容，并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请检查操作内容，并酌情选择 **Allow**。

现在你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何获取项目中的指令文件上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，并观察 Copilot 如何根据**现有**指令生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和辅助函数基础实现提交到 `main`，以便下一课继续使用。

接下来进入 [agent mode][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
