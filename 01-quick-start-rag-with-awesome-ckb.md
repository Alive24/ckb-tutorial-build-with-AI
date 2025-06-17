# 01 - 快速上手：如何用 Awesome CKB 的整合功能提升 AI 的 RAG 效果

作者：Alive - CKB Dev Rel
日期：2025-06-13

## 什么是 RAG（Retrieval-Augmented Generation）？

RAG，即检索增强生成，指的是在 LLMs 生成结果前增加一个信息检索环节从而提升回答质量。
你可以提前准备好一个领域相关的知识库，里面可以包含你精心整理的文档、代码片段、笔记，甚至是实时更新的在线资源。
当你向 AI 提问时，它不会盲目生成答案，而是先在这个知识库中快速定位最相关的信息，然后基于这些精准的上下文，生成高质量、可信赖的回答。

简单来说，RAG 就是你先给 AI 提供一些相关资料做准备，AI 再结合这些资料生成回答。

## Awesome CKB 整合功能为何能提升 AI 的 RAG 效果

Awesome CKB 页面不仅收录了 Nervos CKB 生态的优质资源，还基于DeepWiki和Context7提供了AI友好的信息获取方式，并且还内置了整合（Aggregation）语料的功能，适合为 AI 编辑器（如 Cursor）等工具提供高质量的上下文，省去了用户自行收集整理语料的环节，方便用户在多个相关文档的指导下进行 AI 助力开发。另外，每次用户触发文档更新时，整合语料接口都会自动获取最新版本的内容，这方便了用户一键跟上各个模块最新的变化。用户还可以将各个库的 Issue 加入语料中，这样 AI 就能获取到社区讨论中的实际问题和解决方案，包括常见错误处理、性能优化建议、最佳实践等宝贵经验。这些来自一线的讨论内容往往比官方文档更贴近实际开发场景，能帮助 AI 生成更实用、更接地气的代码和解释，以及文档。

## 操作教程

- 你需要一款可以使用外部语料辅助生成的 MCP Client，本文使用 Cursor 为例。
- 访问 Awesome CKB，了解 CKB 生态圈内有哪些资源可以查阅学习。
- 在 Awesome CKB 页面顶部，通过标签（Tag）筛选你关心的资源类型（如 dApp、Script、Recommended 等）。
- 通常情况下，你可以直接使用“Copy Aggregate All (No Outdated) Preset Link”或“Copy Aggregate Recommended Only Preset Link”快速获取常用聚合 API URL。
  - 你也可以自行勾选你想要聚合的条目，然后在表格下方点击“Copy aggregate link”复制包含所有已选资源的聚合 API URL。
  - 你还可以勾选“Include Issues for LLMs Context”选项，将 GitHub Issue 讨论也纳入聚合，获取一手社区经验（但聚合时间会稍稍变长）。
  - 这里你可以将这个 URL 理解为指向一个常见的`llms.txt`；如果你使用的编辑器只支持直接导入内容，你也可以选择下载`llms.txt`然后复制粘贴导入。

### 在 MCP Client中使用

- 每款MCP Client导入的方式都不太一样；以Cursor 1.0为例，你需要依次选择Cursor Settings -> Indexing & Docs -> Docs 的右侧找到Add Doc按钮，然后将URL复制进去，确认后等待期完成索引。
- 完成索引后，在各处可以生成内容的地方你都可以使用`@Docs`找到你刚刚添加的文档鼓励AI使用你提供的聚合文档优化生成结果的质量。
- 由于聚合器每次访问都会自动获取最新的结果，因此你只需要时不时回到设置页面点击刷新即可，无须另行配置。

## 小贴士

- 推荐优先选择“Recommended”标签资源，确保你一直在用最新的内容。
- 如果你喜欢更为独立的索引方式，你可以选择直接使用Context7提供的LLMs.txt。如果你还需要获取其他库的文档，你还可以直接使用Context7的MCP服务动态获取最新的文档。
