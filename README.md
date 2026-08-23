# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 辅助的参数化设计工作流 🏛️✨

## 为什么做这个项目

在参数化设计（Rhino/Grasshopper）中，将二维图像（如抽象绘画、城市肌理、自然形态等）转化为三维几何形体是一个常见的设计起点。但在实际工作流中，设计师通常面临两个瓶颈：

- **冷启动困难**：从图像到几何规则缺乏清晰的转译路径，每一次面对新的参考图像都需要从头推演构图逻辑。
- **脚本编写门槛**：在 Grasshopper 中写 GhPython 脚本打断设计思维，调试语法的时间常常超过推敲形态的时间。

多模态大模型已经能够“看懂”图像，但**可靠地**将视觉理解转化为可用的参数化代码，仍然需要反复调试 Prompt。我们希望通过这套工作流，把这种“调试”沉淀为可复用的经验。

## 我们做了什么

这是一个基于真实设计项目的**实验记录与工具集**，而非一套自动化 pipeline。

我们以先锋主义绘画（蒙德里安、马列维奇等）为起点，借助 Claude/GPT 等多模态模型完成了从图像理解到参数化模型生成、再到 AI 渲染与 3D 打印交付的完整闭环。过程中涉及：

- **视觉推理**：让 AI 分析参考图像的几何构成与构图逻辑
- **设计语法提取**：将图像特征抽象为可参数化的几何规则
- **代码生成**：通过 Prompt 生成可在 Grasshopper 中运行的 GhPython 脚本
- **人工微调**：设计师介入调整关键参数（体块高度、间距、旋转角度等）
- **AI 渲染**：将模型转化为风格化效果图
- **实体交付**：通过 3D 打印输出物理模型

本仓库记录了这套工作流中的**关键方法、Prompt 模板、迭代过程与可复用经验**。

## ✨ 特点

- **实战来源**：所有内容均来自实际设计项目中的真实尝试，记录了哪些方法有效、哪些无效。
- **开箱即用**：Prompt 模板可直接复制到 ChatGPT/Claude/DeepSeek 中使用。
- **可复现**：每个环节都有清晰的输入、输出与操作说明。
- **持续更新**：后续会随新工具和新模型的迭代持续补充。

> **目标：减少重复调试，把精力留给设计本身。**

---

## 目录 (Table of Contents)

### Part I: 📌 项目介绍
- [项目背景与目标](#为什么做这个项目)
- [工作流全景总览](./workflow/overview-diagram.md)
- [从图像到实体的 6 个阶段拆解](./workflow/stages-breakdown.md)
- [工具链与模型选型说明](./workflow/toolchain.md)

### Part II: 🎯 案例展示
- [Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置](./cases/mondrian-fountain/README.md)
  - [参考图像与 AI 解析对比](./cases/mondrian-fountain/01-reference-analysis.md)
  - [参数化模型生成过程](./cases/mondrian-fountain/02-model-generation.md)
  - [3D 打印实体成果](./cases/mondrian-fountain/03-physical-prototype.md)
- [Case 02: 风格派构成 → 建筑立面转译](./cases/cubist-pavilion/README.md)
- [更多案例进行中...](./cases/README.md)

### Part III: 🧩 提示词模板
- [图像分析 Prompt 模板](./prompt-library/01-image-analysis/)
- [代码生成 Prompt 模板](./prompt-library/02-code-generation/)
- [渲染 Prompt 模板](./prompt-library/03-rendering/)

### Part IV: 💡 复用经验 / Skill

- [视觉推理与设计语法提取](./visual-reasoning/README.md)
- [参数化建模与代码生成](./parametric-modeling/README.md)
- [人机协作迭代日志](./iteration-logs/README.md)
- [AI 渲染与可视化](./rendering/README.md)
- [数字建造与实体交付](./fabrication/README.md)
