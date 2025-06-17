# 00 - 在CKB开发中使用AI助力编程

作者：Alive - CKB Dev Rel
日期：2025-06-13

## 前言

考虑到每个人对于AI辅助编程的涉猎程度、接受程度、熟练程度都会有所不同，本篇文章不可避免具有一定的个人经验色彩；再加上AI辅助编程更新速度极快带来的时效性问题，本文的局限性是显然的，敬请谅解，后续也会尽量地更新内容以优化。

## 简介

以往在CKB开发中，Rust语言和UTXO模型的学习曲线较陡峭，许多开发者在入门阶段会遇到理解难、开发效率低的问题。
AI辅助编程技术的引入，极大地降低了这些门槛。通过智能代码生成、自动补全和上下文理解，AI能够帮助开发者快速掌握复杂概念，减少重复性劳动，避免常见错误，从而专注于创新和业务逻辑的实现。本系列教程将分享如何利用AI工具加速CKB应用开发，提升代码质量和开发效率。

## 准备

### [CKB Getting Started](https://docs.nervos.org/docs/getting-started/how-ckb-works)

在这里你可以快速地了解CKB的基础概念和尝试5分钟快速上手。

### AI辅助工具选择

随着AI技术的快速发展，选择合适的AI辅助工具变得越来越重要，但选择适合的工具需要经验和时间。

基于CKB Dev Rel的经验，在选择工具时，我们需要特别关注以下三点：MCP（Model Context Protocol）、RAG（检索增强生成）以及多模型接入能力。

MCP（Model Context Protocol，模型语境协议）是一个关键的技术标准，它让AI模型能够不局限于训练数据，从外部更好地获取相关上下文和资源数据以辅助生成，而且还增加了其生成过程（比如增加Sequential Thinking或Deep Research能力）和结果形态（比如直接执行代码，进行外部操作）的多样性，从而提供更精准的代码建议和操作指导。

RAG（Retrieval-augmented generation, 检索增强生成）则像是给AI配备了一个强大的自定义知识库。在CKB开发中，这意味着AI能够快速检索和理解相关的文档、指引、代码示例和最佳实践，以及社区讨论中的知识产出，从而生成更准确、更符合生态特点的代码。

多模型接入能力则让开发者总能用上当下表现最好的模型而不受订阅限制，甚至使用本地模型；同时因为模型与工具/界面解耦，使用体验不会因为某些模型提供商产品的不完善而打折。

具体到产品，本文只会基于作者经验推荐选择和资源指引，不会详细到具体使用方式，而这些产品大致可以分为编辑器和Agent（智能体）。

#### 编辑器（包括IDE/插件）

相比Agent，编辑器中的行内自动补全、生成、对话能从用户体验层面直接提高开发效率，也是最标配的工具，因此选择更为成熟和扩展性更好的选项更重要。根据个人喜好，用户可以选择像[Cursor](https://www.cursor.com/downloads)（本文作者的选择）这样的整体解决方案，也可以选择像[Kilo Code](https://kilocode.ai/)这种基于插件拓展的更为非侵入式的方案。这样的编辑器就像雇了一群敏锐高效的助手，解放你宝贵的时间用于真正重要的部分。

#### Agent

虽然例如Cursor也提供了Agent Mode的选项，但除了自主性，Agent维持长上下文的能力也一样重要。这样的Agent能够帮助你更全面深入且连贯地探索概念与设计层面的创新，在MCP的使用上也更为成熟，更像一个能与你一同成长且与你互补的伙伴。Agent的选择上其实更受限于操作系统（本文作者日常会同时使用Mac Mini，Windows台式机，以及Ubuntu笔记本）：Mac OS用户最推荐[Raycast AI](https://www.raycast.com/core-features/ai)（本文作者的选择），在各方面都是最好的选项，尤其是因为Raycast本身的积累使得它在成熟度和使用体验上一时无两；在Windows上，[Highlight](https://highlightai.com/)是一个更为成熟的选项，尤其是在MCP使用体验上使它成为了Windows上的最好选择。[Claude Desktop](https://claude.ai/download)则是一个更为老派的选择，而且可能也是Linux生态中满足需求的唯一选择（然而需要使用[AstroSteveo/claude-desktop-linux-installer](https://github.com/AstroSteveo/claude-desktop-linux-installer)自行编译），也适合其他系统作为一个备用选项。

### MCP使用

简单来讲，你可以将MCP理解为AI语言模型的通用插件系统。有些MCP服务可以让你使用云端服务，有些MCP服务则可以让你在本地运行。然而MCP的安装体验到现在都不是特别理想，因此这里提供了一个简单易懂的安装指南：

#### 自动安装（推荐）

在安装方式上，如果你使用的是Raycast AI和Highlight这两个MCP Client，其内置的环境大大简化了安装配置的流程。
Raycast AI还支持直接从Smithery.ai安装，更进一步简化。
[Smithery.ai](https://smithery.ai/)是目前市面上体量最大的MCP聚合器，在使用上也是最方便的：其界面支持一键安装到各大MCP Client上，因此建议如果你希望使用其他的MCP Client，请尽量在其中选择。

#### 命令行安装

一般各类MCP服务默认都会在其GitHub Repo中提供npx安装命令，直接在你的Node.js环境下运行即可，但命令中往往包含了一些参数需要你去替换。
除了Smithery.ai外，还有一些MCP服务目录聚合器也提供了类似的服务，但一般最多也只会提供npx命令。

- [MCP.so](https://mcp.so/)
- [Cursor Directory](https://cursor.directory/)
  - 这个站点还提供了很多Cursor Rules
- [Pulse](https://www.pulsemcp.com/)
- [Claude MCP Community](https://www.claudemcp.com/)

#### JSON安装（其他资源站）与手动安装

JSON安装则是最通用且最常见，而又看起来最复杂的安装方式。它一般会提供这样的一个JSON字段，然后你将“mcpServer”这个对象里的内容插入到你原有的mcp。json文件中相应的位置，再重启你的Client即可。每个Client配置mcp.json的方式不一样，搜索一下即可。

> 这个KEY只是占位的 你需要换成自己的key

```json
{
    "mcpServers": {
      "openmemory": {
        "command": "npx",
        "args": [
          "-y",
          "openmemory"
        ],
        "env": {
          "OPENMEMORY_API_KEY": "om-767qq3caqfzbi3v0hhlvsz0wmb6hdro6",
          "CLIENT_NAME": "claude"
        }
      }
    }
  }
```

> Tip: Raycast用户可以直接复制这段JSON并打开Install Server，就会自行填入对应的内容。

还有一种比较非主流的方式是使用通用工具，达到一个MCP服务调用任意MCP服务的效果，可以尝试[@smithery/toolbox](https://smithery.ai/server/@smithery/toolbox)。

### RAG

RAG的定义其实相当模糊，实现方式也多样。简单来说，检索增强生成（RAG）技术通过结合多种数据源，提升AI回答的准确性和丰富度。

首先，RAG依赖大量外部数据源，如文档库、代码仓库、社区讨论和在线知识库，这些资源为AI提供了广泛且权威的背景知识，确保回答的专业性和全面性。

其次，Memory（记忆）系统作为RAG中不可或缺的核心组成部分，负责保存和管理用户的对话历史、偏好以及个性化的上下文信息，
为AI提供实时、个性化且连贯的检索内容。

RAG通过同时调用外部数据和Memory，实现“先检索再生成”的智能问答流程，使AI能够基于最新、最相关的多源信息，给出更精准、更全面的回答。
因此，外部数据源和Memory共同构成了RAG的知识基础，二者相辅相成，共同提升AI的智能水平和用户体验。

#### 外部数据源

在CKB生态中， Awesome CKB页面不仅收录了 Nervos CKB 生态的优质资源，还内置了整合（Aggregation）语料的功能，适合为 AI 编辑器（如 Cursor）等工具提供高质量的上下文，省去了用户自行收集整理语料的环节。 请看下一篇教程：[01 - 快速上手：如何用 Awesome CKB 的整合功能提升 AI 编辑器的 RAG 效果](./01-quick-start-rag-with-awesome-ckb.md)

#### Memory（记忆）

Memory（记忆）不但对RAG重要，对Agent概念也是至关重要的：它允许AI记住与用户的对话历史和上下文信息，保证整个对话的整体性。

在MCP生态中，Memory服务绝大部分都是使用Graph Database（图数据库）的方式，将记忆储存为Node和Relation的关系。而记忆服务又分为本地Memory服务和云端Memory服务两大类：

本地Memory服务将对话历史以JSON格式存储在本地，用户对数据拥有完全的控制权，同时还可以通过云盘等方式进行备份，Raycast的Memory是这类服务中体验最好的一个，而其他平台也可使用MCP官方推出的[memory服务](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/memory)。

相比之下，云端Memory服务则将记忆存储在云端数据库中，但目前可用的选项并不多，我推荐的只有OpenMemory，而且目前它缺少记录Relation的功能。如果真的希望云端储存，建议一步到位选择自行部署的Neo4j服务器（也可以在本地通过Docker部署），并使用[mcp-neo4j-memory](https://github.com/neo4j-contrib/mcp-neo4j/tree/main/servers/mcp-neo4j-memory) 连接。

无论是本地还是云端Memory服务，它们都服务于几个共同的核心目标：首先是保持对话的连贯性，让AI能够记住之前的对话内容；其次是存储用户偏好和常用设置；再次是积累知识库，方便后续查询和参考；最后是提供个性化的对话体验。这些功能共同构成了一个完整的Memory服务系统。
