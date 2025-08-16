---
title: 深入了解mcp协议
date: 2025-08-14 15:18:11
tags:
  - golang
  - mcp
typora-root-url: ./深入了解mcp协议
---

[TOC]

# 1. MCP技术

## 1.1 MCP是什么

MCP （Model Context Protocol，模型上下文协议）是一个标准化协议，定义了应用程序和 AI 模型之间交换上下文信息的方式。这使得开发者能够以一致的方式将各种数据源、工具和功能连接到 AI 模型（一个中间协议层），就像 USB-C 让不同设备能够通过相同的接口连接一样。MCP 的目标是**创建一个通用标准，使 AI 应用程序的开发和集成变得更加简单和统一**。

MCP是由人工智能公司 **Anthropic** 于 **2024 年 11 月 24 日**正式发布并开源的协议标准。Anthropic 公司是由前 OpenAI 核心人员成立的人工智能公司，其发布的 Claude 系列模型是为数较少的可以和 GPT 系列抗衡的模型。

![图片](640.gif)

## 1.2 为什么需要 MCP

MCP 协议旨在解决大型语言模型（LLM）与外部数据源、工具间的集成难题，被比喻为“AI应用的USB-C接口“。通过标准化通信协议，将传统的“M×N集成问题”（即多个模型与多个数据源的点对点连接）转化为“M+N模式”，大幅降低开发成本。

![图片](640-1755156942268-60.webp)

在 MCP 协议没有推出之前：

1. 智能体开发平台需要单独的插件配置和插件执行模型，以屏蔽不通工具之间的协议差异，提供统一的接口给 Agent 使用；
2. 开发者如果要增加自定义的工具，需要按照平台规定的 http 协议实现工具。并且不同的平台之间的协议可能不同；
3. “M×N 问题”：每新增一个工具或模型，需重新开发全套接口，导致开发成本激增、系统脆弱；
4. 功能割裂：AI 模型无法跨工具协作（如同时操作 Excel 和数据库），用户需手动切换平台。

没有标准，整个行业生态很难有大的发展，所以 MCP 作为一种标准的出现，是 AI 发展的必然需求。

总结：MCP 如何重塑 AI 范式：

![图片](640-1755156942268-61.webp)

## 1.3 MPC核心组件

如上图所示，在MCP架构体系中，包含MCP Server、MCP Client和MCP Host几个核心组件，及组件间交互协议。

### 1.3.1 MCP Server

MCP Server是一个通过MCP协议实现对外提供服务的轻量级应用，它对外提供工具执行、资源访问、预定义Prompt等一系列资源。

- 资源（Resources）：类似文件的数据，可以被客户端读取，如 API 响应或文件内容
- 工具（Tools）：可以被 LLM 调用的函数 
- 提示（Prompts）：预先编写的模板，帮助用户完成特定任务

下面是一个[文件系统操作](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)的MCP Server示例，它提供了对文件、目录的读写操作。

![图片](640-1755156279646-3.webp)

### 1.3.2 MCP Client

在Host应用内部，MCP Client 与 MCP server 保持 1:1 的连接。MCP Client 充当 LLM 和 MCP Server 之间的桥梁。简单理解就是MCP Client SDK，用于实现Host应用使其能够与MCP Server交互。

### 1.3.3 MCP Host

发起请求的 LLM 应用程序（例如 Claude Desktop、IDE 或 AI 工具）。通常像Claude Desktop 和Cursor这类智能助手应用和IDE，它们都是MCP Host应用，通过MCP client来连接某个MCP Server，并调用其提供的工具。

另外值得注意的是，**运行MCP Host应用所在的主机也属于MCP Host的一部分，主机的文件系统、浏览器等都可能成为MCP Server在运行时操作的对象。**

## 1.4 通信协议

MCP采用JSON-RPC来编码消息，在Sever和Client之间通过标准输入输出和Streamable HTTP进行消息交换。

### 1.4.1 Standard Input/Output (stdio)

采用标准输入/输出流进行传输，适用于在同一台机器上运行的客户端和服务器之间的通信。

**Server**

![图片](640-1755156534813-6.webp)

**Client**

![图片](640-1755156534814-7.webp)

### 1.4.2 Server-Sent Events (SSE)

远程通信**：**利用 SSE 与 HTTP 结合，实现跨网络的实时数据传输，适用于需要访问远程资源或分布式部署的场景。

**Server**

![图片](640-1755156534814-8.webp)

**Client**

![图片](640-1755156534814-9.webp)

###  1.4.3 Streamable HTTP

2025 年 3 月 26 日，MCP 引入了一项关键更新：用 Streamable HTTP 替代原先的 HTTP + SSE 作为默认传输方式。
这一变更在解决原有方案中连接不可恢复、服务端长连接压力大等问题的同时，依然保留了 SSE 带来的流式响应优势。

#### **HTTP + SSE 的缺陷**

远程 `MCP` 通过 `HTTP + SSE` 的传输方式工作，存在以下问题，这也是它所被替换的根本原因：

- 不支持恢复连接

- 要求服务器保持高可用的长连接

- 服务器只能通过 `SSE` 发送消息


#### **不支持恢复连接**

如果客户端和服务器之间的 `SSE` 连接中断了，就无法 “从端点继续”，只能重新开始新的连接，之前的上下文可能会丢失。

#### **要求服务器保持高可用的长连接**

服务器必须一直保持一个稳定、不中断的 `SSE` 长连接，否则通信就中断。

#### **服务器只能通过 `SSE` 发送消息**

服务器无法在已有的请求之外，主动地发送消息给客户端，除了通过专门的 /sse 通道。换句话说，它是“单向被动响应”，而不是“任意时机推送”。

#### Streamable HTTP

`Streamable HTTP` 并不是传统意义上的 **流式 HTTP**（`Streaming HTTP`），它指的是一种 **兼具以下特性的传输机制**：

- 以普通 `HTTP` 请求为基础，客户端用 `POST/GET` 发请求；
- 服务器可选地将响应升级为 `SSE` 流，实现 **流式传输** 的能力（当需要时）；
- 去中心化、无强制要求持续连接，支持 `stateless` 模式；
- 客户端和服务端之间的消息传输更加灵活，比如同一个 `/message` 端点可用于发起请求和接收 `SSE` 流；
- 不再需要单独的 `/sse` 端点，一切通过统一的 `/message` 协议层处理。

#### Streamable HTTP 的优势

- 支持无状态服务器：无需维持高可用的长连接
- 纯 `HTTP` 实现：`MCP` 可在纯 `HTTP` 服务中实现，无需 `SSE` 支持
- 兼容基础设施：因为 “只是 HTTP”，可以与中间件和现有基础设施良好集成
- 向后兼容：是当前 `HTTP+SSE` 传输方式的渐进式改进
- 灵活的传输方式：服务器可选择是否使用 `SSE` 进行流式响应

#### 从 HTTP+SSE 到 Streamable HTTP 的变化

- 移除了 `/sse` 端点
- 所有客户端 → 服务端的消息都通过 `/message`（或类似端点）发送
- 所有客户端 → 服务端的请求都可以被服务器升级为 `SSE`，以发送通知或请求
- 服务器可以选择建立会话 `ID` 以维护状态
- 客户端可以通过对 `/message` 发送一个空的 `GET` 请求启动 `SSE` 流
- 该方法兼容旧版本的实现，并允许服务器保持无状态（如果有需要）

#### 为什么不用 WebSocket？

官方团队曾认真探讨过是否应该将 `WebSocket` 作为远程通信的主要方式，并尝试在其基础上实现断线重连等功能。但最终决定暂时不采用 `WebSocket`，主要原因如下：

- 想要以 `RPC` 风格使用 `MCP`（例如构建一个无状态、只暴露基础工具的服务）时，如果每次调用都依赖 `WebSocket`，将引入不必要的维护和网络开销。
- 在浏览器环境中，`WebSocket` 连接无法像普通 `HTTP` 请求那样附加请求头（比如 `Authorization`），而且不同于 `SSE`，`WebSocket` 在浏览器中也无法由第三方库完全“模拟”实现。
- 只有 `GET` 请求可以自动升级为 `WebSocket`，而 `POST` 等其他 `HTTP` 方法并不支持直接升级。这就意味着如果要让 `POST` 请求使用 `WebSocket`，需要一个额外的 **两步升级** 过程，增加了实现的复杂度和延迟。
- 也避免在 `MCP` 规范中增加 `WebSocket` 的可选支持，以减少客户端与服务器间可能的兼容性组合问题（但不阻止社区自己扩展非官方的 `WebSocket` 实现）
- 官方也有意避免在 `MCP` 协议中引入 `WebSocket` 作为官方选项，以避免客户端和服务器之间因传输方式组合过多而导致的兼容性问题（当然，这并不妨碍用户基于 `WebSocket` 自行实现非官方版本）。

当然，如果将来实践中发现 `SSE` 并不理想，官方仍会考虑重新评估 `WebSocket` 的可能性。

#### MCP Server 示例

#### 无状态服务器（Stateless Server）

`Streamable HTTP` 支持构建完全无状态、无需保持长连接的服务器架构。

以一个仅提供大语言模型（`LLM`）工具的服务为例，不依赖其他高级功能，可以按以下方式实现：

- 始终响应初始化请求，但无需持久化任何状态；
- 对所有传入的 `ToolListRequest`，直接返回一个标准的 `JSON-RPC` 响应；
- 对 `CallToolRequest`，执行对应工具，等待其完成后，将结果通过 `HTTP` 响应体以 `CallToolResponse` 的形式返回。

#### 支持流式输出的无状态服务器（Stateless Server with Streaming）

即使服务器完全无状态、且不支持长连接，也仍然可以利用此设计进行流式响应。

以工具调用时的进度反馈为例：

- 当收到 `POST` 的 `CallToolRequest` 时，服务器通过响应头声明该响应将以 `SSE`（`Server-Sent Events`）格式返回；
- 启动工具执行逻辑；
- 在执行过程中，服务器可以通过 `SSE` 向客户端持续发送多个 `ProgressNotification`（进度通知）；
- 执行完成后，服务器通过 `SSE` 发送最终的 `CallToolResponse`；
- 最后，服务器关闭 `SSE` 流，整个交互完成。

#### 有状态服务器（Stateful Server）

对于需要维护客户端会话的服务器，整体架构可以保持与 `http+SSE` 的实现类似。

主要区别在于：服务器需要为客户端生成唯一的会话 `ID`，并要求客户端在后续所有请求中携带该 `ID`。

服务器可以利用会话 `ID` 实现 **粘性路由** 或消息总线中的会话定位。例如，在水平扩展的部署中（部署多台相同的 `mcp server`），某个 `POST` 请求可能被路由到任意一个节点，此时可以通过 `Redis` 等中间件将请求路由到关联的会话上下文，确保状态一致性。

#### 小结

`Streamable HTTP` 是对 `MCP` 协议传输层的一次重要优化。它在保留原有 `HTTP + SSE` 模式优势的基础上，解决了连接不可恢复、长连接负担重、传输不灵活等问题，带来了更高的可用性与灵活性。

###  1.4.4 模式总结

MCP支持了三种通信方式：**Stdio**、**SSE**、**Streamable HTTP**

| 特性           | Stdio                        | SSE                            | **Streamable HTTP**            |
| -------------- | ---------------------------- | ------------------------------ | ------------------------------ |
| 通信方向       | 双向（仅限本地）             | 单向（服务器到客户端）         | 双向（支持复杂交互）           |
| 使用场景       | 本地进程间通信               | 实时数据推送                   | 跨服务、分布式系统、高并发场景 |
| 支持并发连接数 | 低                           | 中等                           | 高（适合大规模并发）           |
| 适应性         | 局限于本地环境               | 支持浏览器，但单向通信         | 高灵活性，支持流式数据与批处理 |
| 协议基础       | 标准输入输出（stdin/stdout） | HTTP协议（Server-Sent Events） | HTTP协议（双向流式传输）       |



## 1.5 MCP协议带来的改变

在使用MCP协议之前，开发智能应用的开发者（如Cursor）如果要使用外部服务的能力，每个应用开发需要各自开发对接外部服务的程序来实现能力集成。使用MCP后，服务提供者（如Slack、Google Drive）会以MCP标准协议对外提供MCP Server，服务的工具和资源将统一汇聚到市场上，应用以标准化的接口对接资源和工具，从而提升资源和工具的标准化和复用率。

![图片](640-1755156534814-10.gif)



## 1.6 **MCP 的发展情况**

MCP 自 **2024 年 11 月 24 日 发布以来，**OpenAI、Google、微软、腾讯、阿里、百度等头部企业纷纷接入 MCP，推动其成为**事实性行业标准。**并且相继出现了 [mcp.so](https://mcp.so/) 、[mcpmarket](https://mcpmarket.cn/) 等超大体量的 MCP 服务提供商。国内的头部企业也相继加入 MCP 服务商的竞争中，百度发布了[千帆 MCP 广场](https://sai.baidu.com/mcp?ref=openi.cn)**，**阿里发布了[魔搭 MCP 广场](https://www.modelscope.cn/mcp?category=research-and-data&page=1)。在如此庞大的 MCP 市场下，开发者基本不需要开发自己的插件，直接使用 MCP 服务商的插件就可以直接开发大量 Agent。

同时很多头部企业，开始把自身原有的 API 业务开发成封装成 MCP 服务对外提供。比如

1. GitHub Copilot 提供 MCP 的方式生成代码；
2. AWS 2025 年 6月推出开源工具 Amazon Serverless MCP Server**，**支持 Agent 直接操作云上资源，进行服务编排。
3. 腾讯地图、高德地图、百度地图均发布 MCP Server，支持在 Agent 中使用丰富的地图资源。
4. 腾讯云COS、阿里云OSS、百度网盘均已支持 MCP 协议的接入。

**未来趋势**：

- **与 AIOS 融合**：MCP 正成为 AI 操作系统（如华为鸿蒙 HMAF）的核心组件，实现跨设备智能调度；
- **生态挑战**：大厂通过 MCP 构建“闭环生态”（如阿里集成高德地图），可能引发协议割裂，需推动跨平台协作标准。

MCP 不仅是技术协议，更是 **AI 生产力革命的基石**——它让模型真正融入现实世界，成为人类工作的无缝延伸。



# 2. MCP执行细节

当用户在Host应用中输入一个Query后，Host应用是如何在LLM, MCP Client和MCP Server之间交互的呢？下图描述了Query请求处理的完整流程：

![图片](640-1755156534814-11.webp)

1. MCP Client首先从MCP Server获取可用的工具列表；
2. 将用户的Query连同工具描述通过Function Calling一起发送给 LLM；
3. LLM 决定是否需要使用工具以及使用哪些工具；
4. 如果需要使用工具，MCP Client会通过 MCP Server执行相应的工具调用；
5. 工具调用的结果会被发送回LLM；
6. LLM基于所有信息生成自然语言响应；
7. 最后将响应展示给用户。

## 2.1 Host调用LLM

Host将用户Query和Tools一起发给LLM。

![图片](640-1755156534814-12.webp)

## 2.2 Client调用MCP Server

大模型通过Query和提供的工具进行推理，确定完成用户任务需要执行的工具，如果LLM返回结果中如果需要执行tool_calls，则在会话中调用MCP Server。

![图片](640-1755156534814-13.webp)

## 2.3 返回最终处理结果

Host将MCP Server执行的结果和Message一并发给LLM，如果不需要再调用其他工具，那么调用LLM做最后润色后返回最终结果。

![图片](640-1755156534814-14.webp)

# 3. Agent, Function Calling与MCP

## 3.1 Agent

LLM Agents是专为生成需要推理和执行任务的高级人工智能系统。它们能够通过推理思考、回忆过往对话，并通过调用外部工具，根据上下文和用户要求动态规划和执行任务，最终自动完成用户问题。

![图片](640-1755156534814-15.webp)

形象来讲，LLM是一个强大的文本处理大脑，但其本身不具备执行任务的能力，因此需要借助工具，使其能够调用外部工具和服务，从而代理人工完成复杂任务的规划和执行。

## 3.2 Function Calling

**Function Calling（函数调用）** 是大型语言模型的关键技术。前面有提到过 **RAG技术** 是为了解决模型无法和外接数据交互的问题，但是 **RAG** 的局限在于只赋予了模型检索数据的能力，而 **Function Calling** 允许模型理解用户请求中的潜在意图，并自动生成结构化参数来调用外部**任何**函数/工具，从而突破纯文本生成的限制，实现与真实世界的交互，比如可以调用查天气、发邮件、数学计算等工具。

Function Call 模型最早由 OpenAI 在 2023 年 6 月 13 正式提出并发布，首次在 GPT-4 模型上实现了 Function Calling 能力。OpenAI 作为大语言模型的领路人，其发布的模型的 API 协议都会行业标准，后面国内外新发布模型都会按照 OpenAI 的协议作为标准实现。截止目前，支持 Fucntion Calling 能力的主流模型如下表：

![图片](640-1755156766459-57.webp)

除了上面的知名度高的模型，还有一些其他开源或闭源模型也支持了 Fucntion Calling 能力，但是截止目前为止，GPT-4 仍然是公认的 Fucntion Calling 能力最强的模型。

[OpenAI最初通过Function Calling](https://platform.openai.com/docs/guides/function-calling)来给大模型装上“手脚”，让其能够调用外部工具和服务，从而代理人工完成复杂任务的规划和执行。

通过精妙的Prompt设计，可以让模型根据用户Query和可用的Function描述来进行任务分解和推理，从而选择合适的工具进行执行，并通过多次迭代完成用户任务。

![图片](640-1755156534815-16.webp)

## 3.3 MCP

Function Calling的问题显而易见，对于Function的描述各厂商存在差异，模型迁移成本高；另外Fucntion Call存在效果不稳定、容错性低等问题。而MCP标准化了Agent与外部资源和工具交互的接口、协议及实现。从而为构建更为健壮的Agent应用和推动生态的协作起到了关键作用。

## 3.4 对比

![图片](640-1755156534815-17.png)



# 4. MCP技术生态

随着[Manus](https://manus.im/)的发布，MCP协议得以快速普及，MCP应用、MCP Server服务以及MCP应用市场的蓬勃发展，使得MCP技术生态迅速繁荣起来。

![图片](640-1755156534815-18.webp)

## 4.1 MCP Server Marketplace

几个主要的MCP Server Marketplace上Host的MCP Server数量在短短20天内（2025/3/28 - 2025/4/18）有了大幅的增长。这进一步说明了其技术生态的繁荣程度。

| **MCP Marketplace**                                          | **发布MCP Server数量\**\**3/28 - 4/18** | **类型**                          |
| :----------------------------------------------------------- | :-------------------------------------- | :-------------------------------- |
| [Smithery](https://smithery.ai/)                             | 2978 - 4792                             | 综合MCP资源                       |
| [mcp.so](https://mcp.so/)                                    | 4831 - 8990                             | 综合MCP资源                       |
| [PulseMCP](https://www.pulsemcp.com/)                        | 3189 - 3864                             | 综合MCP资源                       |
| [Cursor Directory](https://cursor.directory/)                | 1800+                                   | Cursor生态，专注代码开发场景      |
| [Glama](https://glama.ai/mcp/servers)                        | 3390 - 3640                             | 跨模型协作的场景                  |
| [modelcontextprotocol](https://github.com/modelcontextprotocol/servers) | 331                                     | Anthropic官方热门 MCP Server 源码 |
| [Cline](https://cline.bot/mcp-marketplace)                   | 90                                      | VSCode插件市场                    |

## 4.2 MCP应用程序

我们可以在[PulseMCP](https://www.pulsemcp.com/clients)上看到200多个应用支持了MCP，你可以在上面找到适合自己的MCP应用。

![图片](640-1755156534815-19.png)

## 4.3 MCP Server

以[mcp.so](https://mcp.so/categories)上的MCP Server目录为例，我们看到MCP Server的范围已经涵盖数据、研究、云平台、数据库、Chatbot、文件系统、自动化等多个领域。

![图片](640-1755156534815-20.webp)

除此之外，在开发者支持方面，除了官方的SDK以外，[FastMCP](https://github.com/jlowin/fastmcp)、[Spring AI](https://docs.spring.io/spring-ai/reference/api/mcp/mcp-overview.html)、[mcp-agent](https://github.com/lastmile-ai/mcp-agent)等技术框架也进一步降低了MCP的开发成本。

# 5. 对MCP发展的思考

作为一个新兴的技术规范，我们应该以审慎而积极的态度来看待，以下是个人对MCP可能带来的变化和其局限的分析判断。

## 5.1 MCP带来的变化

正如WebService和WSDL定义了系统间服务调用的基本协议，推动了SOA架构的广泛发展，从此单应用系统逐渐发展成为多应用系统，企业内部的各个系统实现了数据交换和服务集成。REST轻量级服务风格进一步降低了数据传输中的Schema复杂度，而之后的微服务又使得服务进一步解耦和原子化，结合容器编排技术提升了服务的治理能力。任何一种技术和协议都是在不断迭代中进行发展、升级及迭代的。

在此，我们不妨大胆预测MCP带来的变化：

![图片](640-1755156534815-21.webp)

**MCP OpenAPI化**

MCP将成为软件对外开放和集成的新协议，SaaS、应用服务将会以MCP方式提供对外服务，MCP将成为与OpenAPI类似的对外服务协议。

**应用交互重构**

重构应用形态，智能助手将以“搜索框”的形式成为各个应用的交互入口，支持语音及自然语言交换形式，完成复杂任务的执行。

**业务E2E自动化**

端到端理解用户意图、执行任务、容错处理将大大简化用户业务流程，一句话自动完成下单、售后、流程审批等操作将变得更为常见。

**生态开放协作**

支付、生活服务、内容服务等服务作为基础能力，实现你中有我、我中有你，从而更大限度拓展服务生态的协作开放。例如，一个个人助手可以完成调用获取好友列表、差旅申请、支付流程等操作，实现在一个应用中通过集成多个服务的MCP Server来完成复杂业务，前提是MCP授权及安全有保证，且MCP具有广泛的应用场景。

## 5.2 技术限制

当然，MCP还处于发展初期，不可避免存在很多问题及缺陷，具体表现在：

**应用范围受限**

MCP Server大部分以本地库方式执行，很大比例的MCP Server依赖于浏览器等外部工具，通常是以Desktop App + MCP方式执行，更适合个人Assistant。企业应用一般在云端执行，这在一定程度限制了MCP的应用。

**行业标准支持**

当前在数据连接和交互领域有众多标准在竞争，MCP协议只是其中之一，要想成为行业通用标准面临很大挑战。目前MCP主要依托Anthropic的Claude生态，缺少核心厂商的支持，但也看到像OpenAI，以及阿里、腾讯也在积极拥抱MCP。

**对协议本质的质疑**

MCP本质上更像是 FunctionCall与Proxy 的组合，在这个角度上来看，MCP只是对于Agent执行外部工具和访问外部资源做了规范，并未在更大范围内实现标准化。许多人质疑其是否能够称之为一个真正的协议。

**安全风险**

MCP Server本地部署，被Host使用后将直接获得Host OS的访问权限，不安全的MCP Server将可能成为漏洞跳板。此外，不可信的MCP Server通过各种[攻击手段](https://km.woa.com/articles/show/625688)也带来了诸多安全挑战。

![图片](640-1755156534815-22.webp)

例如：开发一个 MCP Sever 用来实现加法操作，同时在其中放入恶意的 Prompt，要求 LLM 读取用户当前的工作路径。那么在其执行时就会读取用户工作目录和文件。



参考文章：

> https://www.cnblogs.com/xiao987334176/p/18845151
>
> https://mp.weixin.qq.com/s/BPfVDSaCNFqe39NmtMHc3g
>
> https://mp.weixin.qq.com/s/jTx_ktWeu5Ck8RDZ3b6iRw?click_id=20
