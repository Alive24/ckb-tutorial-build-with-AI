# 如何快速设计并生成产品原型以验证产品概念

作者：Alive - CKB Dev Rel
日期：2025-06-13

快速设计原型是AI辅助开发的另一高光场景。

在AI辅助开发中，长上下文Agent（如支持MCP协议的智能体）能够持续追踪和理解你在产品原型设计过程中的所有对话、需求变更和上下文信息。这类Agent不仅能记住你提出的每一个想法，还能自动关联相关的产品模型、用户场景和技术实现建议。

## 快速设计产品原型

在实际操作中，建议你在原型探索阶段，充分利用支持长上下文和RAG能力的AI Agent（如Raycast AI、Highlight等），并结合聚合文档和社区知识库，让AI成为你产品创新的得力助手。通过与长上下文Agent对话，你可以：

- **探索产品原型概念**：你可以连续提出不同的产品假设和功能点，Agent会基于历史上下文给出更贴合需求的建议和原型方案。
- **梳理产品模型**：Agent能帮助你快速整理出核心业务流程、数据结构和用户交互模型，甚至自动生成初步的原型文档或流程图。
- **快速验证可行性**：你可以直接让Agent分析某个原型的技术可行性、潜在风险和实现路径，甚至生成代码片段或交互原型，极大提升验证效率。通过使用DeepWiki的MCP，你还可以让Agent快速判断关键依赖是否已经可以满足你的需求。
- **追踪社区动态**：Agent还能通过RAG帮助你追踪社区中的相关讨论和动态，了解同生态中是否有其他人提出过类似的想法或已有相关尝试，从而借鉴经验、避免重复造轮子，并发现潜在的合作机会。

这种方式的意义在于：它显著提升了原型设计的质量，使产品经理、设计师和开发者能够围绕更高质量的原型进行高效、深入的讨论，从而加快想法的迭代、问题的发现与方案的优化，极大促进产品创新的推进和落地。

## 在CKB中快速开发产品原型（前端UI/服务框架/合约Transaction Skeleton）

在CKB开发中生成原型可以大致分为前端UI（dApps）/服务框架（API或其他类型的服务）以及合约设计。实际上这几部分都可以作为原型开发的起始点，通过一定的简略和模拟其他模块推进原型开发速度，然后在原型值得继续开发时补上这些部分。

**如果从前端UI开始开发**，根据我们的经验，你可以直接使用例如v0这样的产品直接生成交互和体验的原型，甚至开始尝试视觉上的呈现风格，然后分享给你的伙伴们快速了解你的思路想法并直接在上面给予你反馈。值得注意的是，在CKB开发中我们除了可以开发Web或Mobile App作为前端UI，也可以在Telegram Mini App上做前端。

**如果从服务框架开始**，根据我们的经验，我们会更为推荐使用Node.js（开发效率高，迭代速度快，生态好，跨栈开发门槛低）或者Python开发原型，还可以考虑使用类似像Nest.js这类模块化的框架，然后在后期产品完成度高且有负载压力时再考虑使用Rust等语言重构。而在实际原型开发中，你可以使用Cursor的Ask模式或其他Agent交互式地按照你的需求逐步生成基础框架，长上下文也会有助于你整体把握开发的走向，帮助你兼顾快速开发和架构合理性。

**如果从合约开始**，根据我们的经验，你可以从根据你的业务流程开始书写基本交易结构（Transaction Skeleton），然后在与Agent和团队讨论中不断推敲其数据验证流程和权限验证流程是否合理，并做出对应调整。你可以使用这样一个模版来描述你设计的交易：

```yaml
Inputs:
  input-cell:
    lock: <lock-script-name>
      args: <args说明>
      rules: <Lock Script通过的要求>
    type: <type-script-name>
      args: <args说明>
      rules: <Type Script通过的要求>
    data: <data说明>
    capacity: <如无特殊说明则为所需的Occupied Capacity>
Outputs:
  output-cell:
    lock: <lock-script-name>
      args: <args说明>
      rules: <Lock Script通过的要求>
    type: <type-script-name>
      args: <args说明>
      rules: <Type Script通过的要求>
    data: <data说明>
HeaderDeps:
  header-dep:
    <依赖的HeaderDep与Cell的关联关系要求>
CellDeps:
  dep-alias:
    <依赖Cell与Cell的关联关系的要求>
Witnesses:
  0: <Witness 0>
```

书写过程中可以进行简单的省略，例如常用的Witness，代码的CellDep，或者找零的Cell，但要记得在实际代码中进行验证。另外要记住交易不会验证Output中的Lock Script是否通过。

同时在在这个过程中，你也会需要开始设计你所定义的Cell结构，例如一个xUDT Cell会是这样的结构:

```yaml
data:
    <amount: <uint128> <xUDT data>
type:
    code_hash: xUDT type script
    args: <owner lock script hash> <xUDT args>
lock:
    <user_defined>
```

除了常用的Cell Data结构（例如UDT直接使用Uint128储存数量）之外 ，你还可以设计自定义的Cell Data结构，例如一个Pausable Data Cell会是这样的结构（请参考Molecule Schema的书写教程）：

```mol
table UDTPausableData {
    pause_list:      Byte32Vec,
    next_type_script:      ScriptOpt,
}
```

总结来说，无论你是从服务框架还是从合约原型入手，AI辅助开发都能极大提升你的开发效率和架构合理性。通过合理利用AI工具（如Cursor、DeepWiki、Context7等）和结构化的交易/Cell设计模版，你可以更快地完成原型搭建、验证业务流程，并持续优化你的设计。建议在开发过程中多与AI进行交互，及时记录和总结有效的Prompt与最佳实践，逐步形成适合自己团队的开发流程和知识库。最后，记得在每个阶段都进行充分的测试和代码审查，确保原型的正确性和可扩展性，为后续产品化打下坚实基础。


