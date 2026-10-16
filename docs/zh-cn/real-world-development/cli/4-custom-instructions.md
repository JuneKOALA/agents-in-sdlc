---
title: "练习 4 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应该让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅描述希望生成什么代码，也描述代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将：

- 了解存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 遵循一套开发规范和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应记录其 `Props` 约定；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 获取正确的信息，并按照这些规范完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，使其更好地理解你的编码风格和要求。你可以指定编码约定、库，甚至偏好的注释方式，从而引导 Copilot 提供更符合需求的建议和代码片段。可以为整个存储库创建指令，也可以针对特定类型的文件创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，适用于此存储库中的**每个**请求。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。在依赖某种格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。先了解现有内容，再进行修改，观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（不是终端中）打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些指令适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包括 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意开头的 `applyTo` 字段，它设置了决定指令适用范围的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他指令文件及项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供参考示例。

## 更新指令文件以符合团队规范

现有文件已经提供了不错的基础，但还缺少一项要求。我们将修改核心的 `copilot-instructions.md`，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约在第 35 行。
2. 在该部分最后添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令更新后，请退出并恢复筛选对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改过的 TypeScript 文件。留意新增的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot 如何使用指令文件中的上下文，并将新标准应用到了功能中。具体来说，你：

- 了解了存储库指令和路径范围指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 看到了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试得到一致执行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
