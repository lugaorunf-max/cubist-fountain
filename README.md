# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 驱动的参数化设计工作流 🏛️✨

## 为什么做这个项目

当设计师面对一幅抽象画（如蒙德里安、马列维奇的作品），试图将其转化为三维建筑形体时，通常会遇到两个瓶颈：

- **冷启动困难**：从二维图像的视觉构图到三维参数化模型，中间缺乏清晰的转译规则。
- **脚本编写门槛**：在 Rhino/Grasshopper 中编写 GhPython 脚本打断了设计思维，让创意流变得支离破碎。

与此同时，多模态大模型（GPT-4o、Claude 3.5 等）已经具备了强大的图像理解能力，但如何将这种理解**可靠地**转化为可用的参数化代码，仍然是一个需要反复调试的过程。

我们不想让这种“调试鸿沟”继续存在。

## 我们做了什么

我们基于蒙德里安《红黄蓝构图》等先锋主义画作，探索并跑通了一条 **“图像语义拆解 → 结构化 Prompt → 代码生成 → 人工微调 → 参数化建模 → AI 渲染 → 3D 打印”** 的完整工作流，并将过程中的所有 Prompt 设计、迭代记录和关键经验开源出来：

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

### Part I: 图像理解与解析 Prompts

- [图像语义分析 —— 基础版](./prompts/01-image-analysis/v1-baseline.md)
- [图像语义分析 —— 加入几何关系约束](./prompts/01-image-analysis/v2-geometry.md)
- [图像语义分析 —— 结构化 JSON 输出](./prompts/01-image-analysis/v3-structured.md)
- [示例输出 —— 蒙德里安《红黄蓝构图》](./prompts/01-image-analysis/examples/mondrian-output.json)

### Part II: 代码生成 Prompts

- [GhPython 脚本生成 —— 初版（几何错误）](./prompts/02-code-generation/v1-broken.md)
- [GhPython 脚本生成 —— 加入坐标系与尺寸约束](./prompts/02-code-generation/v2-fixed.md)
- [GhPython 脚本生成 —— 加入参数化调节接口](./prompts/02-code-generation/v3-parametric.md)
- [生成的 GhPython 脚本示例](./prompts/02-code-generation/examples/generated-script.py)

### Part III: 迭代与优化记录

- [第一轮：AI 无法理解“叠涩”与“斜线切割”](./iteration-logs/round-1.md)
- [第二轮：几何正确但缺乏设计感](./iteration-logs/round-2.md)
- [第三轮：加入风格约束后的突破](./iteration-logs/round-3.md)
- [关键经验总结](./iteration-logs/lessons-learned.md)

### Part IV: AI 渲染与可视化 Prompts

- [AI 渲染图生成 —— 风格描述模板](./prompts/04-rendering/style-prompt.md)
- [AI 渲染图生成 —— 材质与光照控制](./prompts/04-rendering/material-lighting.md)

### Part V: 完整工作流总览

- [工作流全景图](./workflow/overview-diagram.md)
- [端到端示例：从蒙德里安到实体模型](./workflow/end-to-end-example.md)

---

## Part I: 图像理解与解析 Prompts

> **使用说明**：以下 Prompt 可直接复制到 ChatGPT/Claude/DeepSeek 等支持多模态输入的模型中使用。每个 Prompt 都经过多轮迭代，请完整复制以获得最佳效果。

### 图像语义分析 —— v3（最终版）
