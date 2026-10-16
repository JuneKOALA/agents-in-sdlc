---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 执行了三个独立任务，以改进用户体验和添加功能。Cloud agent 虽然设计为异步自主运行，但监视这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务。你可以了解已执行的操作，也可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监视 cloud agent 任务。
- 引导正在运行的会话，要求添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到现在也是添加浅色模式的好时机。既然已经要更新网站样式并添加切换功能，将浅色模式也纳入工作似乎很合理。你将引导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，包括：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和所采用的方案而异。

4. 还可以留意右侧显示的拉取请求 (PR) 面板。它可以让你查看 PR 和已更改的文件，以便进一步监视任务。

## 引导 cloud agent

查看正在执行的任务后，让 Copilot 在处理高对比度模式时也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，等待它完成设置并开始执行任务。看到类似下方的消息时，就说明已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents 页面中的 cloud agent 任务，其中突出显示了在 Copilot 工作时引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 开始工作

与之前一样，Copilot 会开始处理更新后的任务！完成你发送消息时正在处理的步骤后，它会将新请求纳入工作流程。

和之前一样，这需要几分钟。现在可以稍作休息，回顾迄今为止的学习内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监视和引导 GitHub Copilot cloud agent 任务的中心入口。在这个任务控制中心，你可以跟踪所有活动和已完成任务、查看正在执行的工作，并重定向正在运行的任务以调整范围或提供更多指导。

你了解了以下概念：

- 使用任务控制中心和智能体页面监视 cloud agent 任务。
- 重定向正在进行的会话以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，我们将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
