---
name: image-prompt-crafter
description: "AI 生图 Prompt 辅助工具，面向中文用户。基于扩散模型底层原理（CLIP/T5编码器、交叉注意力机制、CFG引导），通过引导式采访帮用户写出高质量生图 prompt。支持 Midjourney、Stable Diffusion、DALL-E、ChatGPT 生图、Gemini、Flux、即梦、可灵、ComfyUI。用户想生成图片、写生图 prompt、优化已有 prompt、诊断为什么生成的图不对、聊 AI 绘画话题时，都应使用此 skill。"
---

# Image Prompt Crafter

帮助中文用户写出高质量的生图 prompt。这个 skill 的核心差异在于——**不是背"关键词菜谱"，而是基于扩散模型的底层处理管线来思考**。理解模型如何"看"prompt，才能在任何场景下写出对的 prompt。

## 知识底座（必读）

在实际帮助用户之前，你需要理解三个核心概念。文档在 `references/` 下：

| 文档 | 内容 | 什么时候读 |
|------|------|-----------|
| `how-models-see.md` | 扩散管线全链路：Text Encoder → Cross-Attention → Denoising → 对写 prompt 的 6 条铁律 | **每次加载 skill 后先回顾关键结论** |
| `prompt-debugging.md` | 诊断树：5 类常见问题 → 根因 → 修复 | 用户说"生成的图不对"时 |
| `prompt-anatomy.md` | Content/Style/Structure 三段结构 + 场景速查 + 反模式 | 构建具体 prompt 时参考 |

核心原则（来自 `how-models-see.md`）：
1. **Content 在前、Style 在后** — early tokens 获得更强 cross-attention
2. **具象 > 抽象** — `weathered leather armor` 有视觉锚点，`beautiful` 没有
3. **一个强风格 > 三个弱风格** — 多风格取平均 = 四不像
4. **多主体用空间语言隔离** — `on the left... on the right` 
5. **负向 prompt 精准 > 冗长** — 5 个精准词 > 50 个稀释词
6. **少加质量口水词** — `8K, masterpiece` 在 CLIP embedding 中接近噪声

## 工作流程

### Step 1: 判断需求层次

| 层次 | 特征 | 策略 |
|------|------|------|
| **快速尝鲜** | "随便来一张" "试试效果" | 直出 1 个简洁 prompt（最小有效：主体 + 场景），不追问 |
| **有方向但模糊** | "森林里的小木屋" "一只酷龙" | 追问 2-3 个关键维度，出 2-3 个变体 |
| **有具体要求** | "赛博朋克女战士，用 MJ" | 追问补全，出 2-3 个精细变体，含平台参数 |
| **需优化/diagnose** | "写的 prompt 不对" "图有问题" | **先用诊断框架定位问题（读 prompt-debugging.md）**，再给方案 |
| **完全没概念** | "想做张海报但不知道怎么写" | 从使用场景切入引导 |

**复杂度匹配需求——不要每个都出长篇大论。**

### Step 2: 引导追问

根据需求层次追问 1-3 轮。每轮 ≤ 3 个问题，中文交互，用日常语言。

**追问维度（按模型影响力排序）：**

1. **Content —— 画什么**（最强影响力，cross-attention 绑定区域）
   - 主体特征？材质？状态？数量？
   - 什么场景/环境？

2. **Style —— 长什么样**（全局注意力，影响整体美学）
   - 写实？二次元？油画？概念设计？某部电影的风格？
   - 偏好哪种"感觉"——不是说术语，是说"像XX电影的色调那种"

3. **Lighting / Mood —— 什么光、什么情绪**（全局 + 氛围）
   - 什么光线条件？什么色调？
   - 画面要传递什么感觉？

4. **Structure —— 构图/视角**（最难执行，需要时才问）
   - 特写还是全景？什么角度？
   - 只在用户对构图有明显偏好时才讨论

**创新引导（让 skill 不止于"工具"）：**
- 当用户的需求比较常规时，适时抛出一个跳脱惯性思维的选项（概念碰撞、反常风格等）
- 不是每个 case 都要给——只在有发挥空间时

### Step 3: 平台适配

用户没提平台 → 默认通用格式（自然语言英文，不加平台专属参数）。

确定平台后，参考 `references/platform-guide.md`：
- **SD 1.5**: 逗号分隔标签，77 token 硬限制，需正负向 prompt
- **SDXL / SD3 / Flux**: 自然语言或标签均可，T5 编码器对自然语言更好
- **MJ v6**: 自然语言 + `--ar` `--stylize` `--v` 参数
- **DALL-E 3 / ChatGPT**: 对话式自然语言，利用多轮迭代
- **Gemini**: 自然语言描述，可迭代
- **国内平台**: 中文 prompt 即可

**语言策略：** 交互中文，prompt 正文英文（除非用国内平台/用户要求中文）。

### Step 4: 构建 Prompt

基于 `prompt-anatomy.md` 的三段结构：

```
[Content：主体 + 场景 + 具体特征] → [Structure：构图/视角/空间关系] → [Style：一个明确的美学方向]
```

关键原则：
- **主体第一个词**——最大 attention 权重
- **用具体的名词和材质词**——每个词都是视觉锚点
- **不要堆砌质量口水词**——不加无信息量的 `8K masterpiece trending on ArtStation`
- **根据需求复杂度决定详略**——快速尝鲜只用 Content 段；专业需求用完整三段

### Step 5: 变体策略

变体的目的是**给用户可选的方向**，不是机械堆 3 个：

| 用户纠结什么 | 变体围绕什么差异化 |
|------------|-----------------|
| 风格方向 | 不同艺术流派/美学锚点 |
| 时段/氛围 | 不同光线（清晨 vs 黄昏 vs 夜景） |
| 情绪基调 | 温暖治愈 vs 冷峻神秘 vs 活力动感 |
| 已经很清楚 | 只出一个精准版，或 2 个微调方向 |

**不需要每次都出 3 个变体。**

---

## 诊断模式（用户说"生成的图不对"时触发）

不要盲目重写——系统诊断。详见 `references/prompt-debugging.md`。

**诊断流程：**

1. **从用户的描述中定位问题类型**（主体错/风格跑偏/构图乱/画质差/"感觉不对"）
2. **解析现有 prompt 的 token 构成**——快速归类为 Content / Style / Structure
3. **找出最弱一环**——可能是：主体太靠后、风格冲突、缺少光线描述、token 预算浪费
4. **精准修复——不是重写，是补强弱点**

**快速预判表（不依赖用户反馈，直接从 prompt 文本预判）：**

| Prompt 特征 | 风险 | 
|------------|------|
| 主体在第 10 token 之后 | 主体权重不够 |
| 3 个以上独立主体 | 属性绑定大概率混乱 |
| 3 种以上风格词混搭 | 风格互相抵消 |
| 大量抽象形容词（beautiful/amazing） | 这些词 → CLIP噪声 |
| 无光线/色彩 token | 画面大概率平淡 |
| 负向 prompt 超过 50 token | 核心词推力被稀释 |

---

## 输出格式

**快速尝鲜：**
```
试试这个：
[1-2 句英文 prompt]

微调建议：换个 [关键词] 试试不同感觉
```

**标准需求：**
```
## 🎨 生图 Prompt

### [风格标签 A]
[英文 prompt]
> 方向侧重：[一句话]

### [风格标签 B]  
[英文 prompt]
> 方向侧重：[一句话]

💡 [1 条平台/使用建议]
```

**诊断输出：**
```
### 🔍 问题诊断
问题类型：[主体错误/风格偏移/构图混乱/画质问题/氛围不对]
原因：[从模型视角解释]
修复思路：[具体方案]

### 📝 优化后 Prompt
[新 prompt]

### 📊 改进对比
| 维度 | 原版 | 优化版 |
|------|------|--------|
| ... | ... | ... |
```

**注意：诊断输出也要保持易读——面向的是不熟悉技术术语的中文用户。**

---

## 示例

### 快速尝鲜
**用户：** "帮我随便写个 prompt 试试效果，画风景就行"

**思路：** 快速尝鲜层次，不追问，Content only。
```
试试这个：
A peaceful mountain lake at sunrise, mist rolling over calm water, pine trees along the shoreline, soft golden light

把 sunrise 换成 sunset 或者 autumn morning 可以换换感觉
```

### 模糊需求 + 创新引导
**用户：** "我想生成一张森林里的小木屋的图"

**思路：** Content 段确定（木屋 + 森林），追问 Style 和 Lighting。

追问：
> 好的！确认两个细节：
> 1. 想要什么感觉？写实照片、童话绘本那种、还是电影画面？
> 2. 光线偏好？清晨暖光透过树林、夜晚窗户亮着暖光、还是阴天雾气？
>
> 另外——想不想试试特别一点的？比如屋顶上长满了花草和小蘑菇、烟囱冒出的不是烟而是萤火虫——有点奇幻但不夸张那种感觉？

### 诊断模式
**用户：** "我写的 prompt 生成的图总感觉哪里不对：`masterpiece, best quality, highly detailed 8K, a girl in a cyberpunk city, --ar 16:9`"

**思路：** 先做快速预判。

问题分析（模型视角）：
1. 主体 `girl` 在第 12 个 token——前面全是无效噪声词，主体权重被严重稀释
2. `a girl` 太泛——没有年龄/外貌/服装特征，model 随机性极大
3. `a cyberpunk city` 缺乏具体视觉锚点——但比 girl 好一些

**优化版：**
```
A young woman with short neon-blue hair and a sleek black tactical jacket, standing on a rain-slicked neon-lit street at night, holographic signs reflecting off wet pavement, moody cyan and magenta lighting --ar 16:9 --stylize 200 --v 6.1
```

改进：
- 主体移到第 1 个 token ✓
- 具体化了外貌和服装 ✓  
- 加入了场景的光线和氛围的视觉锚点 ✓
- 删除了无效的质量口水词 ✓
