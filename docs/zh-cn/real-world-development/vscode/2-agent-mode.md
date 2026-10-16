---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你的工作流程通常包括查找所有必要文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……工作还不止于此。

这正是 Copilot Agent Mode 的用武之地。

Copilot Agent Mode 在 IDE 中可以更自主地工作。它的行为与开发者类似：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但 agent mode 和 cloud agent 面向两种不同体验。Agent mode 在 IDE 中执行任务，可以快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot Agent Mode 如何探索项目、确定相关文件并协调多文件更改。
- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望用户能够按类别和发行商筛选游戏。上一练习中已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成数据层、UI 和测试方面的剩余工作。

## 运行 Tailspin Toys 网站

进行任何更改之前，先浏览 Tailspin Toys 网站，了解其当前功能。

这是一个带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看游戏详情。该网站是单个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则通过使用标准 DOM API 的小型、范围明确的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务来启动单个 Astro 开发服务器，方便运行网站。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，应该会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，提示你打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端中选择 localhost URL 链接会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，你的网站服务器现在正通过该隧道运行！

### 浏览网站

网站启动后，可以查看其功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表及其标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、描述、发行商和类别等信息。

## 使用 Copilot 探索积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新的对话。各练习对模式和模型的要求可能不同；每个步骤都会在相关位置说明。

网站的初始实现已能正常运行，不过我们还想添加新功能。先查看积压工作。根据模板设置项目时，系统已自动为你创建一组议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 会自主规划并跨多个文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。构建本工作坊时使用了 Claude Sonnet 4.5，因此在说明中列出了该模型。这样做是希望代码建议相对一致，以提供良好体验。不过，由于 LLM 具有概率性，收到的建议可能与说明不同。这完全正常且符合预期。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应该能够完成任务。

3. 向 Copilot 发送以下提示，询问议题积压工作：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. 选择 **Continue** 运行命令，列出所有议题。
5. 查看生成的议题列表。

请注意，Copilot 甚至会根据它认为对用户最有用的内容为议题排序。

## 查看指令文件

开始让智能体生成代码之前，先查看将用于向 Copilot 提供工作上下文的指令文件。我们将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以重复使用、拆分复杂指令，并提供参考示例。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善其余筛选逻辑
- 添加或更新筛选行为测试
- 更新游戏列表页，添加筛选 UI

此外，合并前必须运行并通过测试。Copilot Agent Mode 可以帮你完成这些任务！现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 作为模型。
5. 使用以下提示，根据积压工作中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，找到与目标功能相关的文件。你应该会看到它定位到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行中选择它们。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框截图，其中要求用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至有些测试会失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成并且未检测到错误。

   ![Copilot Agent Mode 完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意可能存在的问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加功能。我们了解了：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、确定相关文件并协调多文件更改。
- 合并到代码库前，如何审查 Copilot Agent Mode 生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 附加探索练习 — 实现分页

随着游戏列表不断扩大，就需要添加分页功能。运用本练习学到的技能，提示 Copilot 更新网站并实现分页。可以考虑以下几点：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页的实现方式：是否允许用户选择每页数量，还是采用固定数量。
- 编写提示时提供必要的指导，确保 Copilot 按照你的要求实现功能。
- 你可能需要与 GitHub Copilot 迭代，提出更改并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：作用及适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
