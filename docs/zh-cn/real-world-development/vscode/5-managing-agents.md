---
title: "练习 5 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在 agent mode 中使用 GitHub Copilot 时，它会自主工作——探索代码库、提出更改建议、编辑文件并运行命令。由于 Copilot 会在本机实时工作，VS Code 中的 Copilot Chat 会持续显示每个工具调用和文件编辑。你可以内联审查每项差异、接受或拒绝单个更改，还可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监视无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中引导智能体，在高对比度模式旁添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了另一个请求——他们也希望为觉得默认主题过暗的用户添加浅色模式切换。与其从头开始新的对话，不如引导现有会话扩展工作范围。

## 监视 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤——探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录查看每项工具调用。你会看到文件读取、文件编辑和它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 还原更改，让智能体再次尝试。
4. 还可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认为安全的命令。

5. 等待智能体完成高对比度模式的实现。完成后应该会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

高对比度模式实现后，接下来在同一对话中添加浅色模式切换。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括涉及的组件、采用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续工作，检查已有更改并提出浅色模式的补充实现。
3. 像审查高对比度模式一样，逐项审查新编辑。对每个差异选择 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不合意，例如它尝试以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止工作，再发送澄清性的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以通过后续提示要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个切换控件，将浅色模式整合到现有设置面板中”），或扩展范围（“还要为两个切换控件添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看更改文件的完整列表。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并检查差异。确认代码遵循自定义智能体强调的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储持久化设置。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课中会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监视并引导正在进行的会话——逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留会话上下文。

本课中，你了解了：

- 实时监视智能体的工作。
- 接受和拒绝单个文件编辑。
- 使用后续提示在执行过程中引导会话。

在[下一课][next-lesson]中，你将发布无障碍功能并创建拉取请求，同时审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
