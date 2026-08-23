# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 驱动的参数化设计工作流 🏛️✨

## 为什么做这个项目

当设计师面对一幅抽象画（如蒙德里安、马列维奇的作品），试图将其转化为三维建筑形体时，通常会遇到两个瓶颈：

- **冷启动困难**：从二维图像的视觉构图到三维参数化模型，中间缺乏清晰的转译规则。
- **脚本编写门槛**：在 Rhino/Grasshopper 中编写 GhPython 脚本打断了设计思维，让创意流变得支离破碎。

与此同时，多模态大模型（GPT-4o、Claude 3.5 等）已经具备了强大的图像理解能力，但如何将这种理解**可靠地**转化为可用的参数化代码，仍然是一个需要反复调试的过程。

我们不想让这种“调试鸿沟”继续存在。

## 我们做了什么

我们基于蒙德里安《红黄蓝构图》等先锋主义画作，探索并跑通了一条 **“图像语义拆解 → 结构化 Prompt → 代码生成 → 人工微调 → 参数化建模 → AI 渲染 → 3D 打印”** 的完整工作流，并将其中的视觉解析方法、设计语法转换规则、参数化建模流程以及关键迭代经验开源：

- **Prompt 模板库**：图像分析、几何提取、代码生成等场景的实战 Prompt，附带迭代版本和修改原因。
- **工作流拆解**：将“从画到建筑”的复杂任务拆解为 6 个可执行的阶段，每个阶段都有清晰的输入、输出和工具链。
- **失败记录与经验**：记录了哪些 Prompt 设计会导致代码跑不通、几何错误或风格偏离，以及如何修复。

## ✨ 特点

- **实战打磨**：所有 Prompt 均来自真实的设计项目，经历了多轮迭代验证。
- **开箱即用**：复制 Prompt 到 ChatGPT/Claude/DeepSeek，上传你的参考图像，即可获得结构化输出。
- **可复现的工作流**：完整的代码、Prompt、输入输出示例，让你可以按图索骥。
- **持续更新**：随着新模型和新技术（如 Agent、MCP）的演进，我们会不断迭代和补充。

> **不要在 Prompt 调试上浪费时间，把精力留给真正的设计。**

---

## 目录 (Table of Contents)

### Part I: 设计案例成果集

- [Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置](./cases/mondrian-fountain/README.md)
  - [参考图像与 AI 解析对比](./cases/mondrian-fountain/01-reference-analysis.md)
  - [参数化模型生成过程](./cases/mondrian-fountain/02-model-generation.md)
  - [3D 打印实体成果](./cases/mondrian-fountain/03-physical-prototype.md)
- [Case 02: 风格派构成 → 建筑立面转译](./cases/cubist-pavilion/README.md)
- [更多案例进行中...](./cases/README.md)

### Part II: 工作流全景总览

- [端到端完整流程图](./workflow/overview-diagram.md)
- [从图像到实体的 6 个阶段拆解](./workflow/stages-breakdown.md)
- [工具链与模型选型说明](./workflow/toolchain.md)

### Part III: 视觉推理与设计语法提取

- [图像语义分析 —— 从基础到结构化](./visual-reasoning/01-image-analysis.md)
- [几何关系与构图约束提取](./visual-reasoning/02-geometry-extraction.md)
- [设计语法（Design Grammar）的形式化表达](./visual-reasoning/03-design-grammar.md)

### Part IV: 参数化建模与代码生成

- [GhPython 脚本生成流程](./parametric-modeling/01-generation-workflow.md)
- [坐标系、尺寸与几何约束](./parametric-modeling/02-constraints.md)
- [参数化接口设计：让设计师可调节](./parametric-modeling/03-parameter-control.md)

### Part V: 人机协作迭代日志

> 真实的设计过程不是“一次生成”，而是 AI 与设计师的多轮对话。

- [Round 1: AI 无法理解“叠涩”与“斜线切割”](./iteration-logs/round-1.md)
- [Round 2: 几何正确但缺乏“建筑感”](./iteration-logs/round-2.md)
- [Round 3: 加入风格约束后的突破](./iteration-logs/round-3.md)
- [关键经验：什么时候该信 AI，什么时候该介入](./iteration-logs/lessons-learned.md)

### Part VI: AI 渲染与可视化

- [渲染风格描述模板](./rendering/style-prompt.md)
- [材质与光照控制](./rendering/material-lighting.md)

### Part VII: 数字建造与实体交付

- [从 Rhino 模型到 STL 导出](./fabrication/01-stl-export.md)
- [3D 打印参数与后处理](./fabrication/02-printing-settings.md)
- [实物模型展示](./fabrication/03-physical-gallery.md)

### Part VIII: Prompt 与模板库（附录）

> 前面各 Part 中使用的所有 Prompt 在此集中归档，供直接复制使用。

- [图像分析 Prompt 模板](./prompt-library/01-image-analysis/)
- [代码生成 Prompt 模板](./prompt-library/02-code-generation/)
- [渲染 Prompt 模板](./prompt-library/03-rendering/)


