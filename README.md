# Open Canvas

[点击这里试用](https://opencanvas.langchain.com/)

![应用截图](./public/screenshot.png)

Open Canvas 是一个开源的网页应用，用于与 AI 代理协作以更好地编写文档。它的灵感来自 [OpenAI 的 "Canvas"](https://openai.com/index/introducing-canvas/)，但有几个关键区别。

1. **开源**: 从前端到内容生成代理，再到反思代理的所有代码都是开源的，并采用 MIT 许可证。
2. **内置记忆**: Open Canvas 默认配备了[反思代理](https://langchain-ai.github.io/langgraphjs/tutorials/reflection/reflection/)，它在[共享记忆存储](https://langchain-ai.github.io/langgraphjs/concepts/memory/)中存储风格规则和用户见解。这使得 Open Canvas 能够在不同会话中记住有关您的信息。
3. **从现有文档开始**: Open Canvas 允许用户从空白文本或所选语言的代码编辑器开始，让您可以从现有内容开始会话，而不是被迫从聊天交互开始。我们认为这是理想的用户体验，因为很多时候您已经有一些内容要开始，并希望在此基础上进行迭代。

## 功能特点

- **记忆系统**: Open Canvas 有一个内置的记忆系统，它会自动生成关于您和您的聊天历史的反思和记忆。这些内容会被包含在后续的聊天交互中，以提供更个性化的体验。
- **自定义快速操作**: 自定义快速操作允许您定义自己的提示，这些提示与您的用户绑定，并在会话之间保持。这些操作可以通过单击轻松调用，并应用于您当前查看的内容。
- **预置快速操作**: 还有一系列针对常见写作和编码任务的预置快速操作，这些操作始终可用。
- **内容版本控制**: 所有内容都有一个"版本"与之关联，允许您回溯时间查看内容的先前版本。
- **代码、Markdown 或两者兼具**: 内容视图允许查看和编辑代码和 Markdown。您甚至可以进行生成代码和 Markdown 内容的聊天，并在它们之间切换。
- **实时 Markdown 渲染和编辑**: Open Canvas 的 Markdown 编辑器允许您在编辑时查看渲染后的 Markdown，无需来回切换。

## 使用方法

您可以通过访问 [opencanvas.langchain.com](https://opencanvas.langchain.com/) 免费使用我们的部署版本，或者您可以克隆此仓库并在本地运行/部署到您自己的云端。请参见下一节了解具体步骤。

![Open Canvas 图表示意图](./public/lg_studio_graph_diagram.png)

## 开发

运行或开发 Open Canvas 很简单。首先克隆此仓库并进入目录。

```bash
git clone https://github.com/langchain-ai/open-canvas.git

cd open-canvas
```

接下来，通过 pnpm 安装依赖：

```bash
pnpm install
```

然后[安装 LangGraph Studio](https://studio.langchain.com/)，这是在本地运行图所必需的，或者[创建 LangSmith 账户](https://smith.langchain.com/)以部署到 LangGraph Cloud 生产环境。

之后，将 `.env.example` 文件内容复制到 `.env` 并设置所需的值：

```bash
# LangSmith 追踪
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=

# LLM API 密钥
# Anthropic 用于反思
ANTHROPIC_API_KEY=
# OpenAI 用于内容生成
OPENAI_API_KEY=

# LangGraph 部署，或通过 LangGraph Studio 的本地开发服务器。
# 如果在本地运行，此 URL 应在 `constants.ts` 文件中设置。
# LANGGRAPH_API_URL=

# Supabase 用于身份验证
# 公共密钥
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

最后，启动开发服务器：

```bash
pnpm dev
```

然后，用浏览器打开 [localhost:3000](http://localhost:3000) 开始交互！

您也可以观看一个简短的（2 分钟）视频演示，了解如何在本地设置 Open Canvas [点击这里](https://www.loom.com/share/e2ce559840f14a9abf1b3d5af7686271)。

## LLM 模型

Open Canvas 设计为与任何 LLM 模型兼容。当前部署配置了以下模型：

- **Anthropic Claude 3 Haiku 👤**: Haiku 是 Anthropic 最快的模型，非常适合快速任务，如编辑文档。在[这里](https://console.anthropic.com/)注册 Anthropic 账户。
- **Fireworks Llama 3 70B 🦙**: Llama 3 是 Meta 的最新开源模型，由 [Fireworks AI](https://fireworks.ai/) 提供支持。您可以在[这里](https://fireworks.ai/login)注册账户。
- **OpenAI GPT 4o Mini 💨**: GPT 4o Mini 是 OpenAI 最新的小型模型。您可以在[这里](https://platform.openai.com/signup/)注册 API 密钥。

如果您想添加新模型，请按照以下简单步骤操作：

1. 在 `constants.ts` 中添加或更新模型提供者变量。
2. 安装提供者所需的包（例如 `@langchain/anthropic`）。
3. 更新 `src/agent/utils.ts` 中的 `getModelNameAndProviderFromConfig` 函数，以包含您的新模型名称和提供者。
4. 手动测试以确保您可以：
  > - 4a. 生成新内容
  >
  > - 4b. 生成后续消息（生成内容后自动发生）
  >
  > - 4c. 通过聊天中的消息更新内容
  >
  > - 4d. 通过快速操作更新内容
  >
  > - 4e. 对文本/代码重复上述步骤（确保两者都能工作）

## 路线图

### 功能

以下是我们希望在不久的将来添加到 Open Canvas 的功能列表：

- **在编辑器中渲染 React**: 理想情况下，如果您让 Open Canvas 生成 React（或 HTML）代码，我们应该能够在编辑器中实时渲染它。**编辑**: 这现在正在规划阶段！
- **多个助手**: 用户应该能够创建多个助手，每个助手都有自己的记忆存储。
- **为助手提供自定义'工具'**: 一旦我们在 LangGraph.js 中实现了 `RemoteGraph`，用户应该能够让助手访问调用他们自己的图作为工具。这意味着您可以自定义您的助手，使其能够访问当前事件、您自己的个人知识图等。

您有功能请求吗？请[提出问题](https://github.com/langchain-ai/open-canvas/issues/new)！

### 贡献

我们希望继续开发和改进 Open Canvas，并希望得到您的帮助！

首先，GitHub 上有一些带有功能请求的问题，概述了改进和添加内容以使应用的用户体验更好。
主要有三个标签：

- `frontend`: 这个标签添加到以 UI 为重点的问题上，不需要太多或任何代理工作。
- `ai`: 这个标签添加到专注于改进 LLM 代理的问题上。
- `fullstack`: 这个标签添加到需要同时涉及前端和代理代码的问题上。

如果您对贡献有任何问题，请通过电子邮件联系我：`brace(at)langchain(dot)dev`。对于一般的错误/代码问题，请[在 GitHub 上提出问题](https://github.com/langchain-ai/open-canvas/issues/new)。
