# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 辅助的参数化设计工作流 🏛️✨

![License](https://img.shields.io/badge/License-MIT-blue)
![Rhino](https://img.shields.io/badge/Rhino-8-darkgray)
![Grasshopper](https://img.shields.io/badge/Grasshopper-GhPython-green)
![LLM](https://img.shields.io/badge/LLM-Claude%20%7C%20GPT%20%7C%20Gemini-8A2BE2)
![Output](https://img.shields.io/badge/Output-3D%20Printed-orange)

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


# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 辅助的参数化设计工作流 🏛️✨

![License](https://img.shields.io/badge/License-MIT-blue)
![Rhino](https://img.shields.io/badge/Rhino-8-darkgray)
![Grasshopper](https://img.shields.io/badge/Grasshopper-GhPython-green)
![LLM](https://img.shields.io/badge/LLM-Claude%20%7C%20GPT%20%7C%20Gemini-8A2BE2)
![Output](https://img.shields.io/badge/Output-3D%20Printed-orange)

<p align="center">
  <img src="images/hero.png" alt="hero" width="820">
</p>

<p align="center"><sub>TODO：四联 hero 图 —— 原画 / AI 构图解析标注 / Grasshopper 模型 / 3D 打印成品，从左到右排开</sub></p>

## 📖 为什么做这个项目

当你第五次删掉 AI 生成的 GhPython 脚本时，问题可能不在模型，而在你和它之间的「翻译方式」。

在参数化设计（Rhino / Grasshopper）中，把二维图像（抽象绘画、城市肌理、自然形态）转化为三维几何是常见的设计起点，但通常卡在两处：

- **冷启动困难**：从图像到几何规则缺乏清晰的转译路径，每换一张参考图都要从头推演构图逻辑；
- **脚本编写门槛**：在 Grasshopper 中写 GhPython 会打断设计思维，调试语法的时间常常超过推敲形态的时间。

多模态大模型已经能"看懂"图像，但**可靠地**把视觉理解转化为可用的参数化代码，需要一套被验证过的对话方式。本仓库做的，就是把这套方式——连同它踩过的每一个坑——完整开源出来。

## 🎯 我们做了什么

<p align="center">
  <img src="images/workflow.png" alt="workflow" width="760">
</p>

<p align="center"><sub>TODO：工作流总览图，六阶段，标注每步输入输出</sub></p>

以先锋主义绘画（蒙德里安、马列维奇等）为起点，用多模态大模型跑通了从图像到实体的完整闭环：

| 阶段 | 做什么 | 工具 |
|------|--------|------|
| 🔍 视觉推理 | AI 读取参考图像的几何构成与构图逻辑 | Claude / GPT / Gemini |
| 🧩 设计语法提取 | 图像特征 → 可参数化的规则清单 | 同上 |
| 💻 代码生成 | 规则清单 → 可运行的 GhPython 脚本 | 同上 |
| 🎛️ 人工微调 | 设计师调整关键参数、修正几何 | Rhino / Grasshopper |
| 🎨 AI 渲染 | 模型 → 风格化效果图 | TODO（即梦 / Seedream …） |
| 🖨️ 实体交付 | 3D 打印输出物理模型 | TODO（打印机型号） |

> **这不是一套自动化 pipeline**，而是一份实验记录：哪些问法有效、哪些会翻车、翻车了怎么救。

## 🏆 成果一览

<!-- 表格内嵌缩略图，GitHub 会直接渲染；图片放 images/ 目录，每列宽 140 左右 -->

| 作品 | 原画 | AI 解析 | 参数化模型 | 渲染 | 3D 打印 |
|------|------|---------|-----------|------|---------|
| TODO 斜构水庭 | <img src="images/c1-art.png" width="130"> | <img src="images/c1-analysis.png" width="130"> | <img src="images/c1-model.png" width="130"> | <img src="images/c1-render.png" width="130"> | <img src="images/c1-print.png" width="130"> |
| TODO 岩层星舰 | <img src="images/c2-art.png" width="130"> | <img src="images/c2-analysis.png" width="130"> | <img src="images/c2-model.png" width="130"> | <img src="images/c2-render.png" width="130"> | <img src="images/c2-print.png" width="130"> |
| TODO 方圆织境 | <img src="images/c3-art.png" width="130"> | <img src="images/c3-analysis.png" width="130"> | <img src="images/c3-model.png" width="130"> | <img src="images/c3-render.png" width="130"> | <img src="images/c3-print.png" width="130"> |

<p align="center">
  <img src="images/showcase.gif" alt="showcase" width="700"><br>
  <sub>TODO：打印延时摄影或模型旋转动画 —— 全仓库最抓眼球的位置，值得专门剪一段</sub>
</p>

## ✨ 特性

- 🖼️ **有实物**：不是概念演示，每个案例都走到 3D 打印成品
- 📋 **开箱即用**：提示词完整贴出，复制进 Claude / GPT / DeepSeek 即可复现
- 🧪 **保留失败**：每轮翻车记录都在——坑比模板更值钱
- 🧱 **三段式转译**：图像 → 规则 → 代码，比"图像直接生成代码"可靠得多，这是本仓库最想传达的方法

## 📑 目录

<!-- TODO：上线后在 GitHub 上核对锚点链接是否跳转正确 -->

- **[Part I: 工作流六步拆解](#part-i-工作流六步拆解)** —— 每步的做法、完整提示词、已知翻车点
- **[Part II: 案例走查](#part-ii-案例走查)** —— 从原画到打印成品的完整迭代记录（含失败轮）
- **[Part III: 脚本](#part-iii-脚本)** —— 全部 GhPython 脚本
- **[来源与致谢](#-来源与致谢)**

---

# Part I: 工作流六步拆解

> 💡 **使用说明**：以下 Prompt 可直接复制到聊天框与大模型交互使用。每个 Prompt 都经过实战调试，请完整复制以获得最佳效果。

## ① 🔍 视觉推理

让 AI 读懂一张画的**几何逻辑**，而不是写一段美术赏析。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO（例：不约束输出结构时，AI 会输出散文式赏析，无法进入下一步 → 强制要求按「分割方式 / 比例 / 轴线 / 节奏」四栏输出）

## ② 🧩 设计语法提取

整个工作流可靠性的关键一步：先让 AI 输出一份**参数与规则清单**，再由清单生成代码。清单可以直接和原画对照检查，读错的地方在这一层就能改掉，不用等到代码跑崩。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO

## ③ 💻 代码生成

从规则清单生成 GhPython 脚本。输入越结构化，代码越接近可用。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO（例：脚本能跑但体块互相重叠 → 在 prompt 里显式给定行列数与间距约束）

## ④ 🎛️ 人工微调

AI 负责转译，人负责判断。体块高度、间距、旋转角度、水景落点——这类决策仍然属于设计师。

TODO：一张参数表（参数 / 含义 / 实际调过的取值范围）

## ⑤ 🎨 AI 渲染

````markdown
TODO：渲染提示词，含风格与光照关键词
````

**已知翻车点**：TODO（例：哪类材质描述基本不生效、视角怎么控制才稳定）

## ⑥ 🖨️ 3D 打印

TODO：打印前检查清单（最小壁厚 / 悬挑角度 / 底座接地面积 / 整体缩比）

---

# Part II: 案例走查

## Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置

> TODO：一句话——这个案例最值得看的一点

<p align="center">
  <img src="images/case01-start.png" width="360">
  <img src="images/case01-final.png" width="360">
</p>
<p align="center"><sub>TODO：左原画、右打印成品，首尾对照</sub></p>

<details>
<summary><b>🌀 完整迭代过程（TODO 轮，含 TODO 次失败）—— 点开看怎么救回来的</b></summary>

<br>

### 第 1 轮：失败

TODO：用了什么提示词 → AI 给了什么 → 几何哪里错了、怎么发现的（配图）

### 第 2 轮：修正

TODO：只写改动点（diff 式）→ 新结果（配图）

### 第 3 轮：定稿

TODO：最终模型 + 渲染 + 打印（配图）

</details>

## Case 02: TODO

TODO：同上结构

---

# Part III: 脚本

| 脚本 | 所属案例 | 说明 |
|------|---------|------|
| [`TODO.py`](scripts/TODO.py) | Case 01 | TODO 一句话说明输入输出 |

> 所有脚本的可调参数集中在文件头部，微调阶段直接改数值，不需要读代码。

---

## 🙏 来源与致谢

<!-- TODO: 公开前必须补全本节，并确认已获得指导教师与组员同意 -->

- 工作流来源：TODO（学校 / 学院 / 课程全称与学期）
- 指导教师：TODO
- 团队成员与分工：TODO（本人负责：TODO）
- 案例与课程作业的对应关系：TODO

## 📄 License

[MIT](LICENSE)

# Cubist Fountain: From Abstract Art to 3D-Printable Architecture

> 从蒙德里安的画布到可触摸的实体模型 —— 一套 AI 辅助的参数化设计工作流 🏛️✨

![License](https://img.shields.io/badge/License-MIT-blue)
![Rhino](https://img.shields.io/badge/Rhino-8-darkgray)
![Grasshopper](https://img.shields.io/badge/Grasshopper-GhPython-green)
![LLM](https://img.shields.io/badge/LLM-Claude%20%7C%20GPT%20%7C%20Gemini-8A2BE2)
![Output](https://img.shields.io/badge/Output-3D%20Printed-orange)

<p align="center">
  <img src="images/hero.png" alt="hero" width="820">
</p>
<p align="center"><sub>TODO：四联 hero 图 —— 原画 / AI 构图解析标注 / Grasshopper 模型 / 3D 打印成品</sub></p>

## 📖 为什么做这个项目

当你第五次删掉 AI 生成的 GhPython 脚本时，问题可能不在模型，而在你和它之间的「翻译方式」。

在参数化设计（Rhino / Grasshopper）中，将二维图像（抽象绘画、城市肌理、自然形态等）转化为三维几何形体是一个常见的设计起点。但在实际工作流中，设计师通常面临两个瓶颈：

- **冷启动困难**：从图像到几何规则缺乏清晰的转译路径，每一次面对新的参考图像都需要从头推演构图逻辑。
- **脚本编写门槛**：在 Grasshopper 中写 GhPython 脚本打断设计思维，调试语法的时间常常超过推敲形态的时间。

多模态大模型已经能够"看懂"图像，但**可靠地**将视觉理解转化为可用的参数化代码，仍然需要反复调试 Prompt。我们希望通过这套工作流，把这种"调试"沉淀为可复用的经验。

## 🎯 我们做了什么

<p align="center">
  <img src="images/workflow.png" alt="workflow" width="760">
</p>
<p align="center"><sub>TODO：工作流总览图 —— 六个阶段，标注每步的输入与产出</sub></p>

我们以先锋主义绘画（蒙德里安、马列维奇等）为起点，借助 Claude / GPT 等多模态模型完成了从图像理解到参数化模型生成、再到 AI 渲染与 3D 打印交付的完整闭环。过程中涉及：

- **视觉推理**：让 AI 分析参考图像的几何构成与构图逻辑
- **设计语法提取**：将图像特征抽象为可参数化的几何规则
- **代码生成**：通过 Prompt 生成可在 Grasshopper 中运行的 GhPython 脚本
- **人工微调**：设计师介入调整关键参数（体块高度、间距、旋转角度等）
- **AI 渲染**：将模型转化为风格化效果图
- **实体交付**：通过 3D 打印输出物理模型

本仓库记录了这套工作流中的**关键方法、Prompt 模板、迭代过程与可复用经验**——是一份实验记录与工具集，而非一套自动化 pipeline。

## ✨ 特点

- 🖼️ **有实物**：不是概念演示，每个案例都走到 3D 打印成品
- 📋 **开箱即用**：提示词完整贴出，复制进 Claude / GPT / DeepSeek 即可复现
- 🧪 **保留失败**：每轮翻车记录都在——坑比模板更值钱
- 🧱 **三段式转译**：图像 → 规则 → 代码，比「图像直接生成代码」可靠得多

## 📑 目录 (Table of Contents)

### Part I: 🔄 工作流
- [① 视觉推理](#step-1) —— 让 AI 读懂一张画的几何逻辑
- [② 设计语法提取](#step-2) —— 全流程可靠性的关键一步
- [③ 代码生成](#step-3) —— 规则清单 → GhPython 脚本
- [④ 人工微调](#step-4) —— AI 负责转译，人负责判断
- [⑤ AI 渲染](#step-5) —— 模型 → 效果图
- [⑥ 3D 打印](#step-6) —— 数字模型 → 物理模型

### Part II: 🧪 案例迭代记录
- [成果一览](#gallery) —— 三个作品的五阶段画廊
- [Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置](#case-01)

### Part III: 📋 提示词
- [速查表](#prompts-index) —— 六步主提示词索引
- [变体与辅助提示词](#prompts-variants) —— 不在主线上、但实战用过的

### Part IV: 📦 资产
- [原型参考图片](#assets-images)
- [GhPython 脚本](#assets-scripts)

---

# Part I: 🔄 工作流

> 💡 **使用说明**：以下 Prompt 可直接复制到聊天框与大模型交互使用。每个 Prompt 都经过实战调试，请完整复制以获得最佳效果。

<a name="step-1"></a>
## ① 🔍 视觉推理

让 AI 读懂一张画的**几何逻辑**，而不是写一段美术赏析。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO（例：不约束输出结构时，AI 会输出散文式赏析，无法进入下一步 → 强制要求按「分割方式 / 比例 / 轴线 / 节奏」四栏输出）

<a name="step-2"></a>
## ② 🧩 设计语法提取

整个工作流可靠性的关键一步：先让 AI 输出一份**参数与规则清单**，再由清单生成代码。清单可以直接和原画对照检查，读错的地方在这一层就能改掉，不用等到代码跑崩。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO

<a name="step-3"></a>
## ③ 💻 代码生成

从规则清单生成 GhPython 脚本。输入越结构化，代码越接近可用。

````markdown
# Role
TODO

# Task
TODO

# Constraints
TODO

# Input
TODO
````

**已知翻车点**：TODO（例：脚本能跑但体块互相重叠 → 在 prompt 里显式给定行列数与间距约束）

<a name="step-4"></a>
## ④ 🎛️ 人工微调

AI 负责转译，人负责判断。体块高度、间距、旋转角度、水景落点——这类决策仍然属于设计师。

TODO：一张参数表（参数 / 含义 / 实际调过的取值范围）

<a name="step-5"></a>
## ⑤ 🎨 AI 渲染

````markdown
TODO：渲染提示词，含风格与光照关键词
````

**已知翻车点**：TODO（例：哪类材质描述基本不生效、视角怎么控制才稳定）

<a name="step-6"></a>
## ⑥ 🖨️ 3D 打印

TODO：打印前检查清单（最小壁厚 / 悬挑角度 / 底座接地面积 / 整体缩比）

---

# Part II: 🧪 案例迭代记录

<a name="gallery"></a>
## 成果一览

<!-- 表格内嵌缩略图，GitHub 会直接渲染；图片放 images/ 目录 -->

| 作品 | 原画 | AI 解析 | 参数化模型 | 渲染 | 3D 打印 |
|------|------|---------|-----------|------|---------|
| TODO 斜构水庭 | <img src="images/c1-art.png" width="120"> | <img src="images/c1-analysis.png" width="120"> | <img src="images/c1-model.png" width="120"> | <img src="images/c1-render.png" width="120"> | <img src="images/c1-print.png" width="120"> |
| TODO 岩层星舰 | <img src="images/c2-art.png" width="120"> | <img src="images/c2-analysis.png" width="120"> | <img src="images/c2-model.png" width="120"> | <img src="images/c2-render.png" width="120"> | <img src="images/c2-print.png" width="120"> |
| TODO 方圆织境 | <img src="images/c3-art.png" width="120"> | <img src="images/c3-analysis.png" width="120"> | <img src="images/c3-model.png" width="120"> | <img src="images/c3-render.png" width="120"> | <img src="images/c3-print.png" width="120"> |

<p align="center">
  <img src="images/showcase.gif" alt="showcase" width="700"><br>
  <sub>TODO：打印延时摄影或模型旋转动画 —— 全仓库最抓眼球的位置，值得专门剪一段</sub>
</p>

<a name="case-01"></a>
## Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置

> TODO：一句话——这个案例最值得看的一点

<p align="center">
  <img src="images/case01-start.png" width="360">
  <img src="images/case01-final.png" width="360">
</p>
<p align="center"><sub>TODO：左原画、右打印成品，首尾对照</sub></p>

<details>
<summary><b>🌀 完整迭代过程（TODO 轮，含 TODO 次失败）—— 点开看怎么救回来的</b></summary>

<br>

### 第 1 轮：失败

TODO：用了什么提示词 → AI 给了什么 → 几何哪里错了、怎么发现的（配图）

### 第 2 轮：修正

TODO：只写改动点（diff 式）→ 新结果（配图）

### 第 3 轮：定稿

TODO：最终模型 + 渲染 + 打印（配图）

</details>

<!-- TODO：Case 02、Case 03 按同样结构追加 -->

---

# Part III: 📋 提示词

> 本 Part 只放**索引**和**变体**。六步主线提示词的全文在 Part I，同一个提示词全文只出现一次，不重复粘贴。

<a name="prompts-index"></a>
## 速查表

| 场景 | 提示词 | 适用模型 | 一句话说明 |
|------|--------|---------|-----------|
| 图像构图分析 | [① 视觉推理](#step-1) | Claude / GPT / Gemini | TODO |
| 分析 → 参数清单 | [② 设计语法提取](#step-2) | TODO | TODO |
| 参数清单 → 脚本 | [③ 代码生成](#step-3) | TODO | TODO |
| 模型 → 效果图 | [⑤ AI 渲染](#step-5) | TODO | TODO |

<a name="prompts-variants"></a>
## 变体与辅助提示词

不在六步主线上、但实战中用过的提示词放这里：不同模型的适配版、不同输入类型的改造版（城市肌理 / 自然形态）、调试用辅助 prompt。

### TODO：变体名（如「城市肌理输入的视觉推理改造版」）

````markdown
TODO
````

**与主线版的区别**：TODO

---

# Part IV: 📦 资产

<a name="assets-images"></a>
## 原型参考图片

| 参考图 | 用在哪个案例 | 原作出处 |
|--------|-------------|---------|
| <img src="images/ref-01.png" width="140"> | Case 01 | TODO（如：Piet Mondrian, 1927，公有领域；图片来源 URL） |

<a name="assets-scripts"></a>
## GhPython 脚本

| 脚本 | 所属案例 | 输入 | 输出 |
|------|---------|------|------|
| [`TODO.py`](scripts/TODO.py) | Case 01 | TODO | TODO |

> 所有脚本的可调参数集中在文件头部，微调阶段直接改数值，不需要读代码。

---

## 🙏 来源与致谢

<!-- TODO: 公开前必须补全本节，并确认已获得指导教师与组员同意 -->

- 工作流来源：TODO（学校 / 学院 / 课程全称与学期）
- 指导教师：TODO
- 团队成员与分工：TODO（本人负责：TODO）
- 案例与课程作业的对应关系：TODO

## 📄 License

[MIT](LICENSE)
