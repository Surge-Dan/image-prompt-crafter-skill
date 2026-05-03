# Prompt 结构解析 — 模型管线视角

这篇文档基于 `how-models-see.md` 中的模型原理，展示如何构建高质量 prompt。**这不是一个填空模板——而是帮你理解"为什么这样写效果好"。**

---

## 高效 Prompt 的三段结构

从模型的交叉注意力机制来看，一个 prompt 天然分成三个功能区：

```
[Content 段] → [Structure 段] → [Style 段]
─────────────────────────────────────────
 画什么         怎么组织         长什么样
 强attention    中attention      弱全局attention
 绑定到区域     引导空间关系      影响整体美学
```

### 为什么这个顺序有效

1. **Content 在最前**：early tokens 获得最强的 cross-attention 权重——模型第一个要回答的问题是"画面里有什么"
2. **Structure 在中间**：空间关系 token 在主体确定后，引导"怎么放"
3. **Style 在最末**：风格 token 是全局注意力——它不绑定到哪个区域，而是影响整个画面的处理方式

### 反例：为什么大部分人的 prompt 顺序是反的

```
"masterpiece, best quality, 8K, highly detailed, a girl in a forest, cinematic lighting"
 ↑────── 无效噪声 ──────↑                                    ↑── 风格放中间──↑
```

正确的应该是：
```
"a young woman in a misty pine forest, soft morning light filtering through trees, cinematic photorealistic photography"
 ↑──────────────── Content ────────────────────↑ ↑─────────── Style ────────────↑
```

---

## 各维度的写作策略

### Content 段——主体描述

**原则：具象压倒抽象。每个 token 都是给模型的一个"视觉锚点"。**

```
弱：a warrior                             → 模型有无限可能，结果随机
中：a female warrior in armor             → 方向有了，但还不够
强：a female warrior in worn leather armor → 具体化了材质，模型更有方向
最强：a female warrior in scarred leather armor, gripping a weathered iron sword → 每个名词都有修饰，每个修饰都是视觉信息
```

**如果你只知道用户想要什么主体，不知道如何描述：**

1. 问自己：这个主体是什么材料/材质构成的？
2. 这个主体有什么最显眼的特征？
3. 这个主体处于什么状态（新的/旧的/动的/静的）？

### Structure 段——空间与构图

**这是最难执行的部分。** 扩散模型没有显式的 3D 几何理解，空间 token 只能提供软约束。

```
已确定能较好执行的：close-up, full body shot, wide angle, shallow depth of field, centered
中等可执行的：bird's eye view, low angle, symmetrical, negative space
难执行的：rule of thirds, golden ratio, 精确的空间关系
```

**对于空间关系，用以下策略：**
- 不是 `a dog and a woman`，而是 `a woman sitting on a bench with a dog resting at her feet`
- 加入参照物：`a tiny figure silhouetted against a massive setting sun`
- 用前景/背景：`a stone fountain in the foreground, rolling hills stretching to the horizon behind`

### Style 段——美学处理

**单一、明确的方向 > 多个模糊的方向。**

```
效果最好：一个具体的文化/美学锚点
很好：一个艺术流派 + 一个光线描述
还行：一个宽泛的流派（"digital art"）
差：三个互相冲突的流派拼在一起
```

**例外：** `photorealistic` + 具体的光线和镜头描述，这两个方向不冲突，可以组合。

---

## 最小有效 Prompt

不是每个 prompt 都要很复杂。最小有效 prompt = **一个具体的主体 + 一个场景/环境**。

```
快速尝鲜/简单需求的最小配方：

"A [具体主体] in a [具体场景/环境]"

例子：
"A fluffy orange cat in a sunlit garden"           → 两段式的有效 prompt
"A glass of iced coffee on a cafe table"            → 也够了
"A single cherry blossom tree on a misty hillside"  → 简洁但有画面感
```

只有用户需要特定风格/氛围时，才追加 Style 段。

---

## 常见 Prompt 场景速查（模型优化版）

### 人物肖像

```
[主体（年龄/性别/特征）], [表情/姿态], [服装/配饰], [环境], [光线], [风格锚点]

"A young woman with freckles and windswept auburn hair, soft genuine smile, wearing a cream linen dress, standing in a golden wheat field at sunset, warm backlighting creating a halo effect, editorial portrait photography"
```

### 风景/场景

```
[地点/环境], [时间/天气], [光线], [氛围], [风格锚点]

"A mist-covered valley at dawn, layers of rolling green hills fading into the distance, golden first light breaking over the mountain ridge, ethereal atmosphere, wide landscape photography"
```

### 奇幻场景

```
[地点 + 奇幻元素], [主体（可选）], [光线/色彩], [氛围], [美学锚点]

"A floating crystal citadel suspended among storm clouds, lightning arcing between floating islands, bioluminescent blue glow emanating from the crystal structures, epic fantasy atmosphere, concept art style"
```

### 极简/设计感

```
[主体], [构图/空间], [背景], [光线]

"A single red paper boat, centered on a vast calm dark water surface, misty empty background, dramatic overhead spotlight, minimalist composition"
```

---

## 不要做的事（Prompt 反模式）

| 反模式 | 为什么不好 | 改法 |
|--------|-----------|------|
| `a beautiful and amazing and wonderful scene` | 全是无信息量的抽象词 | 改用一个具象的环境描述 |
| `photorealistic, oil painting, anime, 3D render` | 四个互相抵消的风格信号 | 只保留一个 |
| `8K, masterpiece, highly detailed, best quality, ultra HD` 开头 | 浪费 token 预算，无视觉信息 | 删除，或在末尾只留 1 个（如果需要） |
| prompt 超过 80 词但主体在第 15 个词才出现 | 主体被前面的噪声稀释 | 主体移到第一个词 |
| `((((highly detailed))))` | 括号过深产生 artifacts | 用更轻的权重或不用括号 |
