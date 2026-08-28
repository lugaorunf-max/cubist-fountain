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
<p align="center"><sub>TODO：工作流总览图 —— 五个阶段，标注每步的输入与产出</sub></p>

我们以先锋主义绘画（蒙德里安、马列维奇等）为起点，借助 Claude / GPT 等多模态模型完成了从图像理解到参数化模型生成、再到 AI 渲染与 3D 打印交付的完整闭环。过程中涉及：

- **视觉推理**：让 AI 分析参考图像的几何构成与构图逻辑
- **设计语法提取**：将图像特征抽象为可参数化的几何规则
- **代码生成**：通过 Prompt 生成可在 Grasshopper 中运行的 GhPython 脚本
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
- [① 视觉推理](#step-1) 
- [② 设计语法提取](#step-2) 
- [③ 代码生成](#step-3) 
- [④ AI 渲染](#step-4) 
- [⑤ 3D 打印](#step-5) 

### Part II: 🧪 案例迭代记录
- [成果一览](#gallery) 
- [Case 01: 蒙德里安《红黄蓝构图》→ 参数化喷泉装置](#case-01)

### Part III: 📦 资产
- [原型参考图片](#assets-images)
- [GhPython 脚本](#assets-scripts)

---

# Part I: 🔄 工作流

> 💡 **使用说明**：以下 Prompt 可直接复制到聊天框与大模型交互使用。每个 Prompt 都经过实战调试，请完整复制以获得最佳效果。各步骤包含实战中用过的全部版本——主线版 + 变体版。
> 模板统一为六段式：**# Role / # Task / # Constraints / # Execution Protocol / # Input / # Output**。其中 # Execution Protocol 是输出前的自查清单（写法借鉴 [awesome-ai-research-writing](https://github.com/Leey21/awesome-ai-research-writing)），实测能显著减少"看似合理实则跑偏"的输出。
> ⚠️ **①②③ 请在同一场对话中依次进行**：上下文连续是生成质量的关键，中途开新对话会让 AI 丢失前面的规则清单。

<a name="step-1"></a>
## ① 🔍 视觉推理

让 AI 读懂一张画的**几何逻辑**，而不是写一段美术赏析。

**主线版**（有参考图像时）：

````markdown
# Role
你是一位精通建筑形式分析的建筑学者，长期参与参数化设计课程的教学与评审，擅长用"形状文法（Shape Grammar）"的语言拆解绘画与平面图的几何生成逻辑。你对"美术赏析式"的空谈零容忍：凡是不能转译为几何操作的形容词，在你这里都不及格。

# Task
分析我上传的参考图像，提取它的几何生成逻辑，输出一份可供参数化建模直接使用的"构成规则分析"。这不是美术赏析，而是一份"生成说明书"——读它的下游（代码生成模型）要能不看原图，就重建出画面的构成逻辑。

# Constraints
1. 分析纪律：
   - 禁止散文式风格描述（如"画面充满张力""构图和谐"），每条结论必须对应一个具体的几何操作（偏移 / 旋转 / 阵列 / 镜像 / 布尔等）。
   - 所有角度、比例、数量给出估计数值，不允许只给定性描述（写"偏转约 15°"，不写"微微倾斜"）。
2. 分析框架（四个维度，缺一不可）：
   - 分割方式：画面被哪些基础几何（矩形 / 圆 / 折线）切分，各占据画面的什么位置；
   - 比例关系：关键元素间的尺寸比例（用相对值，如 1:2），最大与最小元素的尺度跨度；
   - 轴线与网格：是否存在控制轴线、正交网格或局部旋转坐标系（注明偏转角度）；
   - 节奏与递归：哪些母题在重复、嵌套或逐层变异，变异遵循什么规律（如逐层缩放 0.85）。
3. 多系统处理：
   - 图像若包含多个子系统（如主网格 + 局部旋转核），分开描述各自规则，再单独描述它们的交接方式（穿插 / 咬合 / 相切）。
   - 一次只分析一张图；若我上传多张，逐一分析后再总结共同规则，禁止混为一谈。
4. 输出格式：
   - 严格按 # Output 的三部分输出，除此之外不要输出任何多余的对话与客套。

# Execution Protocol
输出前请在后台自查：
1. 每条结论后面能否接上一个几何动词？出现"张力""韵律""和谐"这类无操作指向的词，立即改写。
2. 四个维度是否都给了数值？有没有漏掉某一栏？
3. 这份规则清单交给一个没见过原图的人，能否据此画出大致构成？（不能 → 补充描述后再输出）

# Input
[上传参考图像：绘画 / 平面图 / 照片]
补充说明（可选）：图像背景与提取方向，例如"这是一张斯卡帕的首层平面，我希望提取平面构成用于装置设计"。

# Output
- Part 1 [概述]：不超过 100 字的整体构成概述。
- Part 2 [规则清单]：按四个维度列出，每条一行：规则名 + 几何描述 + 估计数值。
- Part 3 [参数建议]：3 个可参数化变量（变量名 + 控制的特征 + 建议取值范围），供下一步设计语法提取使用。
````

**已知翻车点**：
- 不约束输出结构时，AI 输出散文式赏析，无法进入下一步 → 模板强制四栏结构 + 数值化 + 自查协议。
- 一次上传多张参考图、只说"综合这几张"，AI 会张冠李戴 → 一次一张图，或明确写"综合几张图的共同规则"。
- 分析完直接要代码，一旦 AI 误读图像则整轮报废 → 先把本步规则清单确认无误，再进第②步。

<details>
<summary><b>📎 变体：多原型比选版（只有风格方向、没有具体参考图时）</b></summary>

<br>

````markdown
# Role
你是精通现代建筑平面类型学与参数化设计的建筑师，熟悉从风格派到解构主义的平面构成谱系。

# Task
我暂时没有具体参考图像，只有风格方向。请基于给定风格，提出 3 种可用于参数化生形的平面原型，供我比选。

# Constraints
- 三种原型的空间气质必须显著不同（例如：强引导动线型 / 克制错动咬合型 / 迷宫碎片型）。
- 每种原型只写三件事：几何构成（形与操作）、空间效果（挖空或挤出后的空间气质）、与基地的适配点。
- 不写建筑史背景与大师介绍。

# Execution Protocol
输出前自查：三种原型是否真的可以互相区分？把名称遮掉后，仅凭构成描述能否分出谁是谁？（不能 → 拉大差异再输出）

# Input
风格方向：【如：斯卡帕 / 至上主义 / 立体主义】
基地条件：【一句话，如：3.6m 高差台地、南侧需要厚墙】

# Output
3 个原型对照清单（名称 + 构成 + 空间效果 + 适配点），末尾一句话给出你的推荐与理由。
````

**与主线版的区别**：主线版是"看图提取规则"，变体是"凭空生成候选方向"。实战中只有粗略方向时，先用变体拿到候选原型、选定一种后再进主线版，全程会顺得多。

</details>

<a name="step-2"></a>
## ② 🧩 设计语法提取

整个工作流可靠性的关键一步：先让 AI 输出一份**参数与规则清单**，再由清单生成代码。清单可以直接和原画对照检查，读错的地方在这一层就能改掉，不用等到代码跑崩。

**主线版**：

````markdown
# Role
你是一位参数化设计方法专家，熟悉形状文法（Shape Grammar）与 Grasshopper 建模，经历过无数次"规则没写清、代码跑歪了"的返工。你坚信：能在纸面上用规则说清的设计，才配写成代码。

# Task
基于上一步的图像构成分析，把它转译成一份"可执行的生成规则清单"。这份清单是下一步生成代码的唯一依据——规则清单确认无误后，代码只是它的忠实翻译。

# Constraints
1. 清单结构（两段式）：
   - A. 初始词汇（Initial Vocabulary）：允许使用的基础几何，不超过 5 种，每种注明尺寸模数（如"正方形：边长取 M 的整数倍，M=1.2m"）；
   - B. 生成规则（Rules）：每条写成 "Rule N - 名称：作用对象 + 几何操作 + 参数"，
     示例："Rule 3 - 减法与负形：所有 2D 图形不作为墙体向上拉伸，而是作为切刀从完整体块中向下挖去，挖深为可调参数"。
2. 参数纪律：
   - 每条规则必须标注：哪些是固定值、哪些是可调参数（滑块），可调参数给出建议范围。
   - 规则总数 4–6 条；超过 6 条说明设计意图不聚焦，先收敛再来。
3. 反随机条款：
   - 禁止"随机撒点、随机旋转"类规则。如需生成变体，用 seed 控制初始分配，几何规则本身必须是确定性的。
   - 每次几何叠加必须伴随一次明确的几何操作（取交 / 并 / 差 / 对齐共线边），不允许无操作的简单叠加。
4. 负形条款：
   - 若设计意图涉及挖空，必须包含一条"负形/减法"规则，明确切刀与被切对象。
5. 输出格式：
   - 严格按 # Output 的表格输出，除此之外不要输出任何多余的对话。

# Execution Protocol
输出前请在后台自查：
1. 遮住原图读清单：每条规则能否直接写成代码？出现"等""之类"的开口项，立即补全或删除。
2. 每条规则是否都标注了固定值 / 参数？参数是否都有建议范围？
3. 是否存在任何随机性描述？（有 → 改写为确定性规则 + seed）

# Input
[粘贴第①步的构成规则分析]
设计目标（写死尺度与单位）：例如"0.8–2.4m 家具尺度流水装置，单位：米" / "300–400㎡ 下沉窑洞平面，单位：米"

# Output
一张规则清单表格：| 编号 | 规则名 | 作用对象 | 几何操作 | 固定值 / 参数（含建议范围）| 备注 |
````

**已知翻车点**：
- AI 自编规则时容易偷渡随机性，结果美学失控、也无法向导师解释 → Constraints 里显式禁止随机叠加，seed 只控制初始分配。
- 尺度与单位不写死会整轮返工（实录：作业尺度中途从"百米级平面"更正为"2.4m 装置"，此前全部比例参数作废）→ Input 第一项就写死尺度与单位。
- 规则超过 6 条时，代码生成阶段顾此失彼 → 限制 4–6 条。

<a name="step-3"></a>
## ③ 💻 代码生成

从规则清单生成 GhPython 脚本。输入越结构化，代码越接近可用。

**主线版**：

````markdown
# Role
你是熟悉 Rhino / Grasshopper 二次开发的参数化工程师，维护过 Rhino 7（IronPython）与 Rhino 8（Python 3）双版本的 GhPython 脚本库，踩过布尔运算所有的坑：共面崩溃、列表重载、Guid 类型错配。你的代码第一原则是"能跑"，第二原则是"改参数不崩"。

# Task
根据下方"生成规则清单"，编写一个可直接粘贴进 Grasshopper GhPython 电池运行的脚本，参数化地生成目标几何。代码是规则清单的忠实翻译：清单里的每条 Rule，都要在代码注释中找到对应段落。

# Constraints
1. 环境锁定（写死三件套）：
   - Rhino 版本：【7 / 8】；电池类型：【IronPython / Python 3】；模型单位：米（m）。
   - 两个大版本的 API 差异很大（如 rs.AddRectangle 的 Plane 参数、System.Drawing 的 import 方式、布尔函数的列表重载），禁止混用。
2. 参数接入规范：
   - 所有可调参数集中在电池输入端，采用安全读取模式：x = X if 'X' in globals() else 默认值。
   - 逐一列明每个输入端的 Type Hint（如 boundary 设 Curve、方向设 Vector3d），并在代码注释中重复一遍。
   - 墙厚、踏步、柱径等构件尺寸用绝对数值，不允许随场地比例缩放。
3. 布尔运算稳定性（按序降级）：
   - 首选穿透法：切刀在切割方向上超出被切体 ≥0.1m，避免共面；
   - 多根曲线布尔一律"单根循环"，不把列表直接传入布尔函数；
   - 仍失败则改零布尔策略：直接生成四面墙拼合出中空体，不做任何减法。
4. 变体与随机：
   - 用 seed 参数控制变体，seed 只影响初始分配，不影响几何规则。
5. 注释规范：
   - 代码注释标明每一步对应规则清单中的哪一条（Rule N）。
6. 输出格式：
   - 严格按 # Output 的三部分输出，除此之外不要输出任何多余的对话。

# Execution Protocol
输出前请在后台自查：
1. 每个输入端是否都给了 Type Hint？（漏一个就可能报 'Guid' object has no attribute ...）
2. 代码里是否有把列表直接传进布尔函数的地方？（有 → 改成单根循环）
3. 所有绝对尺寸是否独立于场地比例？单位是否全程为米？
4. 每条 Rule 是否在注释中有对应段落？

# Input
[粘贴第②步的规则清单表格]
输入端清单：参数名 / 类型 / 建议默认值 / 建议滑块范围
输出端清单：输出名 / 内容（如 outlines: List[Curve]、voids: List[Curve]）

# Output
- Part 1 [代码]：完整可运行的 Python 代码（单文件，可直接粘贴）。
- Part 2 [接线说明]：输入端名称 + Type Hint + 推荐默认值；输出端名称与内容。
- Part 3 [预期结果]：一句话描述生成结果的形态特征（用于核对方向是否跑偏）。
````

**已知翻车点**（全部来自真实 Traceback）：
- `TypeError: ... can not be converted to a Plane` —— Rhino 7 的 rhinoscriptsyntax 与 Rhino 8 的 Rhino.Geometry 混用 → 模板强制写死版本与电池类型。
- `'Guid' object has no attribute 'IsClosed'` —— 输入端没设 Type Hint，GH 传进来的是对象 ID 而非几何 → 模板要求逐一列明 Type Hint。
- `TypeError: 'list' value cannot be converted to Curve` —— Rhino 8 Python 3 的布尔函数不接受列表 → 约定"单根循环切割"。
- 布尔运算静默失败 —— 切刀与被切体共面 → 穿透法；再失败降级网格布尔；终极方案是零布尔砌墙法（直接拼合四面墙，天然中空，稳定且快）。
- 脚本能跑但 Rhino 里没有实体 —— 输出端没接出或没 Bake → Output 强制附接线与 Bake 说明。
- 毫米代码跑进米制模型 → Constraints 写死单位。

<details>
<summary><b>📎 变体：报错修复版（实战中修复效率最高的模式）</b></summary>

<br>

````markdown
# Role
你是熟悉 GhPython 与 Rhino 常见报错的参数化工程师，见过 Rhino 7/8 双版本所有的典型崩溃。

# Task
定位下面这段代码的报错原因，给出最小改动修复。

# Constraints
- 只输出需要改动的代码片段，注明替换位置（函数名 / 大致行号），不要重发完整代码。
- 修复后附一句"同类错误还可能出现在哪里"的排雷提示。
- 如果报错信息不足以定位（例如缺代码或缺环境），先向我要，不要猜。

# Execution Protocol
输出前自查：我的诊断是否对应 Traceback 的最后一行？替换片段的语法是否与声明的环境（Rhino 版本 / 电池类型）匹配？

# Input
1. 完整 Traceback 报错原文
2. 报错的完整代码
3. 环境：Rhino 版本 / 电池类型（IronPython 或 Python 3）/ 模型单位

# Output
1. 一句话诊断（错误类型 + 出错位置）
2. 替换代码片段（标注替换位置）
3. 排雷提示（同类错误还可能出现在哪里）
````

**与主线版的区别**：主线版负责"从清单到代码"，变体负责"迭代排错"。关键经验：**报错原文 + 完整代码 + 环境三件套一次给全，基本一次修对**；只贴报错不贴代码，容易修错位置、来回拉扯。

</details>

<a name="step-4"></a>
## ④ 🎨 AI 渲染

把 Rhino / Grasshopper 里的素模变成能上课评图的效果图。本步的核心矛盾不是"画得好不好看"，而是 **AI 会不会顺手改你的设计**——几何保真永远是第一优先级，材质氛围其次。

> 🛠️ **工具建议**：几何保真要求高时，优先用支持"图像编辑"的模型（Gemini 图像 / GPT-4o 图像这类以上传图为基准做编辑的模型）；纯文生图工具（Midjourney 等）对模型截图的几何还原不可控，更适合做没有模型时的前期意向图。实战经验：nano banana 类模型用**英文提示词**效果明显更好，中英文都可以试，取审美最优。

**主线版：图生图渲染**（有模型截图时，推荐）：

````markdown
# Role
You are an architectural visualization specialist who produces competition-grade renderings for design studios. You treat the uploaded model screenshot as the absolute truth of the design: your job is to dress it with materials, light, and atmosphere, never to redesign it.

# Task
我上传的是 Rhino / Grasshopper 参数化模型的截图（素模）。请将它渲染成一张建筑效果图：严格保持模型的几何形态、体块关系与相机视角不变，只赋予材质、光照、环境与氛围。

# Constraints
1. 几何锁定（最高优先级，违反任何一条都必须重来）：
   - Keep the building geometry, massing, openings, and proportions EXACTLY as in the uploaded image. Do not add, remove, or reshape any element.
   - Keep the camera position, view angle, and perspective unchanged. 不切换为正交或其他视角。
2. 材质（严格按我给的材质表，不要自由发挥）：
   - 材质描述必须具体：exposed concrete / corten steel / brushed brass / oiled oak / rammed earth，
     拒绝"高档材质""现代感材质"这类空泛词。
   - 未在材质表里的构件，保持素模的白色/灰色，不擅自赋材质。
3. 光照与氛围：
   - 光照方案从三种里选一种：golden hour（暖光长影）/ overcast soft light（阴天柔光）/ sharp morning light（锐利晨光、强调阴影），默认 golden hour。
   - 阴影方向必须与主光源一致，落在体块正确的一侧。
4. 配景纪律：
   - 人物仅作尺度参照，数量 ≤ 3，不遮挡主体；植物与家具不得遮挡关键的体块交接与细部。
   - 场地背景以 Input 给定为准，不把装置放进与任务无关的场景。
5. 风格与负面约束：
   - 默认 photorealistic architectural photography；需要拼贴风 / 手绘风时在 Input 显式指定。
   - 禁止：过曝、文字、水印、扭曲的人物、模型中不存在的构件、改变体块比例的"艺术加工"。

# Execution Protocol
出图前逐项自查：
1. 体块数量、轮廓、交接是否与截图完全一致？（不一致 → 重渲染）
2. 所有出现的材质是否都在我的材质表里？
3. 人物比例是否正确、且只有尺度参照作用？
4. 视角与透视是否和截图一致？

# Input
1. [上传模型截图：建议干净素模、隐藏地面网格线]
2. 材质表：构件 → 材质（例：主体基座 → exposed concrete；阶梯收边 → brushed brass；水 → still water with subtle reflection）
3. 视角：人视 / 鸟瞰 / 轴测（默认沿用截图视角）
4. 光照：golden hour / blue hour / overcast / sharp morning light（默认 golden hour）
5. 场地：一句话（例：台地南侧、绿植背景）
6. 风格（可选）：写实效果图 / 拼贴风 / 手绘风

# Output
1. 渲染图一张（构图与截图一致）
2. 一段 50 字以内的自查说明：确认了哪些几何未动、实际赋了哪些材质
````

**常见翻车点**（综合公开提示词库经验，实战后请补充你自己的）：
- AI 自作主张加构件、改体块比例 → 几何锁定条款置顶 + "keep ... EXACTLY as in the uploaded image" 句式；Midjourney 场景用 `--style raw` 并把 chaos 压到 `--c 15` 以下（结构一致性优先）。
- 材质写成"高级感""现代风"，渲染结果随机漂移 → 材质必须是具体名词（exposed concrete 而非 high-end concrete）。
- 人视图被渲成鸟瞰 → Input 写死视角 + Constraints 里 "keep the camera angle unchanged"。
- 人物肢体扭曲、比例失真 → 限制人物数量与作用，或干脆要求无人场景。
- 图像编辑类模型对中文提示词响应不稳定 → 关键约束（几何锁定、相机不变）用英文写。

<details>
<summary><b>📎 变体：文生图意向版（还没有模型时，生成前期概念意向图）</b></summary>

<br>

````markdown
# Role
You are an architectural visualization specialist. 你擅长把一句设计描述扩展成结构完整的英文效果图提示词。

# Task
我还没有模型。根据我给出的设计描述，生成一条可直接用于 Midjourney / 即梦等文生图工具的建筑意向效果图提示词。

# Constraints
1. 提示词结构固定为（填空式，不许缺项）：
   Architectural visualization of [建筑/装置类型], [具体材质], [风格或建筑师语汇], [场地环境], [时间与光照], [氛围], [视角], photorealistic
2. 措辞纪律：
   - 材质必须具体：exposed concrete / corten steel / rammed earth / brushed brass / oiled oak；
   - 视角明确：eye-level view（人视）/ bird's-eye view（鸟瞰）/ axonometric（轴测）/ worms-eye view（仰视）；
   - 光照具体化：golden hour / blue hour / sharp morning light / overcast soft light；
   - 风格可引用建筑师作为语汇缩写（如 in the style of Tadao Ando / Carlo Scarpa）。
3. 负面约束必须追加：no text, no watermark, no distorted human figures, no extra structural elements。
4. 参数建议（Midjourney 场景）：--style raw；--c ≤ 15；比例按用途 --ar 16:9（人视场景）/ 1:1（总平）。

# Execution Protocol
输出前自查：每个填空位是否都是具体词而非空泛词？是否包含 photorealistic（防插画化）？负面约束是否追加？

# Input
设计描述（2–3 句：类型 + 形态 + 场地 + 想要的气质）

# Output
- Part 1 [Prompt]：英文提示词一条（可直接复制）。
- Part 2 [Translation]：中文直译对照（用于核对是否忠于设计描述）。
- Part 3 [参数建议]：Midjourney 参数或国产工具的等价设置。
````

**与主线版的区别**：主线版解决"有模型、要保真"，变体解决"没模型、要意向"。工作流走到这一步通常已有模型，所以主线版是默认路径；变体用于①之前的概念探索。

</details>

> 📚 **关键词弹药库**（渲染提示词的可选词表）：
> - 视角 / 相机 / 光照 / 材质关键词速查：[MidJourney Styles & Keywords Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)（Perspective / Camera / Lighting / Materials 四页）
> - 建筑效果图提示词实例与填空模板：[awesome-midjourney-v7-example-prompts · architecture](https://github.com/Pixmind-io/awesome-midjourney-v7-example-prompts/blob/main/prompts/architecture.md)

<a name="step-5"></a>
## ⑤ 🖨️ 3D 打印

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

# Part III: 📦 资产

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
