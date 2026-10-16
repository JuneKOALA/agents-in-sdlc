---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题。技术债务可能是尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。智能体完成工作后，会生成一个草稿 PR，供人工开发者审查。这样既能将繁琐任务交给智能体处理、加快开发进度，也能让开发者专注于更重要的工作。

你将了解如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下足够的文档，因此代码几乎完全没有说明。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望在每款游戏的详情页上显示同类别的其他游戏，方便访客继续浏览。目前不需要完善的实现，只需要能用于验收测试用户体验的版本。这项工作目前是一个阻碍，但优先级低于其他议题。

这两类任务都很容易被排到较低优先级，因此非常适合分配给 Copilot cloud agent。它可以在后台异步处理任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认符合预期。
## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。与人类开发者协作类似，可以通过多种方式委派工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。会话执行期间或完成后，都可以查看 Copilot 的工作。方案准备好供你审查时，它会在拉取请求中提及你！
## 编写范围明确的指令的重要性

GitHub Copilot 并没有什么魔法——不能因此跳过思考过程。即使看起来简单的操作，深入分析后也可能很复杂，因此[请留意如何为 Copilot cloud agent 界定任务范围][cloud-agent-best-practices]。应将它视为 AI 结对编程伙伴：分阶段工作、学习、尝试并根据结果调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习已介绍自定义指令以及它们如何引导 Copilot。如果你已完成自定义指令的实践练习，应该已经看到 `.github/copilot-instructions.md` 的作用；如果还没有，可以在此时快速查看。在将任务分配给 Copilot cloud agent 前，先只读检查此存储库现有的指令文件，以便稍后观察它们的影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别留意 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令范围限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，留意生成的 PR 中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot cloud agent 设置开发环境

无论由谁参与，编写代码通常都需要特定环境，并且需要运行一些设置脚本，以确保一切就绪。将任务分配给 Copilot 时也一样，因为它会像软件工程师一样工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境执行任务。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，在 cloud agent 开始工作前运行，从而让它使用所需的开发工具和依赖项。此工作流已预先配置好，以便顺利完成实验并提供学习机会。它确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流相同，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 工作流使用 `workflow_dispatch` 触发器，因此可以在存储库的 **Actions** 选项卡中手动运行。这样可以测试工作流是否正常运行，而无需等待 Copilot 触发它。

## 添加文档

大家都知道文档很重要，但大多数项目要么文档已经过时，要么根本没有文档。这类技术债务经常得不到处理，拖慢开发效率，也使维护代码库或引入新开发者变得更困难。Copilot 很擅长创建文档，因此这是适合分配给 Copilot cloud agent 的任务。它会在后台生成所需文档。后续练习中将回来审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题可能类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻后刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 生成的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定合适的处理方案。

13. 审查待办事项列表及 Copilot 将要执行的任务。
14. 向下滚动拉取请求时间线，应该可以看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 留意实时会话内容，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适方案，以及编写代码。

此过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能够专注于其他工作。我们将利用这一点：再分配一项任务给 Copilot cloud agent，然后开始编写应用代码以添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的优势之一是可以拆分工作，让你处理一组任务的同时，它处理另一组任务。即使为设计团队添加相关游戏部分不一定需要很久，这段时间也可以用来完成其他工作。现在就将这项任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论上会出现 👀，表示 Copilot 已开始工作！

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求！Copilot cloud agent 的工作方式与软件工程师类似，因此你无需全程监视，可以等它完成后再审查结果。接下来，让我们创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent 可以将议题分配给 Copilot，让它异步处理任务。可以让 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台执行任务，现在可以开始创建并使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且能够获取前面模块介绍过的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

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
