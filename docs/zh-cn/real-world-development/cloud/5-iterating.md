---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 一起完成了多个任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建相关游戏功能以供迭代，并实现了包括高对比度和浅色模式切换在内的无障碍功能。现在来检查代码更改；如有需要，还可以向 Copilot 提供反馈以改进其工作。

### 场景

正如多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在允许团队其他成员审查前，先检查 GitHub Copilot 为文档、相关游戏功能和无障碍功能生成的建议。
## GitHub Copilot cloud agent 与安全性

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对其用于编写代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在其中创建独立的临时环境来处理任务。
- 任何 GitHub Actions 工作流都必须先由人工批准，才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。
## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中为代码添加了文档。我们将通过 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，具体标题可能有所不同。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息，或回顾迄今为止的学习内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些都来自练习 2 开始时查看过的自定义指令文件。

6. 审查完更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到某些工作流正等待批准的指示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该能看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动另一个会话来处理请求。由于结果并非确定性的，我们无法给出必须使用的具体文字。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建一份介绍 Astro 应用结构的 README。

1. 添加评论，要求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述建议，也可以提出其他希望 Copilot 添加到代码库中的文档要求。
2. 选择 **View Session**，观察 Copilot 如何执行工作。注意它会启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认结果符合预期后，可以将 PR 从草稿状态改为准备审查，然后合并到主分支。

![将 PR 改为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，具体标题可能有所不同。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到某些工作流正等待批准的指示。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 你应该能看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 中的终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以通过迭代向 Copilot cloud agent 请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或者补充文件注释头和 TSDoc 文档注释（请记住，这项任务是在自定义指令更新前分配的！）。与之前一样，可以在 **Conversation** 选项卡中添加新评论提出请求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，查看无障碍自定义智能体实现的功能。这个 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，具体标题可能有所不同。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件
   - 使用本地存储保存用户偏好
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 审查完更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到某些工作流正等待批准的指示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该能看到工作流在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 中的终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能正常，并且页面重新加载后设置仍然有效。

请注意，无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进机会，可以像对其他 PR 一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委托任务

Cloud agent 最适合处理真正的积压工作，让你可以同时专注于其他任务。可以在自己的存储库中提交一些新议题并分配给 Copilot，培养这种协作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的练习。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，稍后可以观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、规划并异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作任务控制中心**，监视并引导正在进行的无障碍会话。
- **审查并迭代了 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 环境的练习。若想继续探索，其他环境的工作坊可以补充本课的实践内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接，从终端完成两项经过审查的更改。

也可以在自己的存储库中尝试以下后续练习：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
