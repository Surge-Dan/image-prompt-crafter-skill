# AI 生图模型如何"看"你的 Prompt

这篇文档解释扩散模型在底层究竟如何处理文本 prompt。理解这些原理后，你就能从"背菜谱"升级为"理解烹饪原理"——任何场景下都能推导出正确的 prompt 策略。

---

## 1. 完整处理管线

```
用户输入的 Prompt 文本
        │
        ▼
┌──────────────────────────┐
│  [1] Text Encoder        │  ← CLIP / T5 / 混合编码器
│  文本 → Token Embeddings │     每个词变成一个高维向量
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│  [2] Cross-Attention     │  ← 将 token 向量"注入"到图像空间
│  Token ↔ 图像区域绑定     │     每个 token 关注图像的某些区域
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│  [3] Denoising Loop      │  ← 在文本引导下一步步从噪声中"浮现"图像
│  随机噪声 → 去噪 → 图像   │     CFG scale 控制文本的引导强度
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│  [4] VAE Decoder         │  ← 潜空间表示 → 可见像素
│  潜空间 → 像素图像         │
└──────────────────────────┘
```

**关键理解**：模型不是一次性"画"出图像——它是从纯噪声开始，每一步都在问自己："给定这段文字，我应该往哪个方向'去噪'？" prompt 越好，这个方向就指得越准。

---

## 2. Text Encoder：词语如何变成数字信号

### CLIP 编码器（SD 1.5 / SDXL 部分）

- SD 1.5 使用 OpenAI 的 CLIP ViT-L/14，有严格的 **77 token 限制**
- SDXL 使用 CLIP ViT-L + OpenCLIP ViT-bigG 双编码器，对长 prompt 的容忍度更高
- CLIP 是在 **互联网图文对（alt-text）** 上训练的——这意味着它更熟悉"照片描述"式的语言，而不是文学化的长篇描述

**对写 prompt 的影响：**
- SD 1.5：prompt 超过 77 token 会被截断！信息密度比长度更重要
- CLIP 的 embedding 空间中，**具体名词的距离比抽象形容词更明确**。"Golden Retriever" 在向量空间中有明确的位置；"beautiful" 的位置非常模糊
- CLIP 不擅长理解语法关系（介词、从句），它更依赖单个 token 和 token 之间的共现统计

### T5 编码器（DALL-E 3 / Flux / SD3 / Imagen）

- T5-XXL 是一个纯文本 LLM 的编码器，在大量自然语言文本上训练
- 对**长文本、复杂句子、逻辑关系**的理解远超 CLIP
- 没有 77 token 的限制（实际限制大得多）

**对写 prompt 的影响：**
- 用 T5 编码器的模型（Flux, DALL-E 3）可以用**完整的自然语言句子**
- 但仍然遵循"具体 > 模糊"的规律——T5 只是更擅长处理自然语言形式，不是魔法

### 为什么"Prompt 用英文写"更好

大多数 text encoder 的训练数据以英文为主。中文 token 在 embedding 空间中的位置相对稀疏、训练不够充分。部分模型（T5 系）也处理中文，但英文的 embedding 质量更稳定。FLUX 和 SD3 对中文的支持较好，但 MJ/SD 系列英文效果明显更优。

---

## 3. Cross-Attention：词语与画面的"婚姻介绍所"

这是整个管线中最关键的环节。在每个去噪步骤中，cross-attention 层会计算**每个文本 token 应该"关注"图像的哪些空间区域**。

### Content Token 的空间绑定

```
Prompt: "A golden retriever sitting in a sunny garden"

Token "golden" + "retriever" → 注意力集中在画面中"狗"所在的区域
Token "garden"              → 注意力分散到背景区域
Token "sunny"               → 注意力全局分布（光照类 token 通常是全局的）
```

### 属性绑定问题（为什么多主体很难）

这是 AI 生图最核心的挑战之一：

```
Prompt: "A red ball and a blue cube"

模型可能生成：
- 红色球 + 蓝色立方体 ✓（正确）
- 红色立方体 + 蓝色球 ✗（属性互换）
- 紫色球 + 紫色立方体 ✗（属性混合）
```

**原因**：cross-attention 是"软"绑定——"red"这个 token 同时关注了球和立方体两个区域，而没有强制只关注球。模型的注意力机制在区分"哪个形容词修饰哪个名词"方面存在天然缺陷。

**应对策略：**
- 用空间语言明确分离：`A red ball on the left side, a blue cube on the right side`
- 减少同一画面中的独立主体数量（≤3 个为宜）
- 如果主体之间有强关联（拥抱、手持），成功率高于独立并置

### Style Token 的特殊性

风格类 token（`oil painting`, `anime`, `cinematic`, `watercolor`）往往是**全局注意力**——它们影响整个画面的美学倾向，而不是某个特定区域。这就是为什么：

- 风格词放在 prompt 的**后部**效果更好（先确定画什么，再确定怎么画）
- 多个冲突的风格词（`photorealistic anime oil painting`）会让模型困惑
- 一个明确指向的风格 > 三个模糊的风格

---

## 4. Denoising 过程：文本如何引导图像"浮现"

### CFG（Classifier-Free Guidance）

CFG 是控制 prompt 跟随强度的核心参数：

```
最终输出 = 无条件生成 + CFG_scale × (有条件生成 - 无条件生成)
```

- **CFG 低（1-4）**：模型更自由，创意性更高，但可能忽略 prompt 细节
- **CFG 中（5-9）**：平衡点，大多数场景推荐
- **CFG 高（10+）**：强行跟随 prompt，但可能产生过度锐化、不自然的画面

**这就是为什么负向 prompt 有效**：在 CFG 计算中，模型同时做正向（跟 prompt）和负向（远离负向 prompt）两个方向，然后取差值放大。负向 prompt 本质上是在"推开"不想要的视觉特征。

### 负向 Prompt 的实际机制

```
负向 prompt "blurry, low quality, distorted"
→ 在每一步去噪中，模型计算"blurry/低质量"的视觉方向
→ 最终的 CFG 公式会**远离**这个方向
```

**有效策略：**
- 负向 prompt 应描述你**不想要的视觉特征**，不是你不想要的**语义内容**
- `blurry, distorted anatomy`（视觉特征）✅
- `no dogs, no people`（语义排除）❌ —— 效果很差！模型不擅长"排除概念"
- 负向 prompt 过长（>50 tokens）会稀释效果

---

## 5. Content vs Style vs Structure — 三维分词法

从模型视角，prompt 中的所有 token 可以归入三个轴：

### Content Token（内容轴）— 决定了"画面里有什么"

```
- 名词：cat, castle, forest, woman, spaceship
- 修饰性形容词：golden, ancient, fluffy, metallic
- 数量/大小：giant, tiny, three, lone
```

**特性**：content token 通过 cross-attention 绑定到具体空间区域。越靠前、越具体的 content token 影响力越强。

### Style Token（风格轴）— 决定了"画面长什么样"

```
- 艺术流派：oil painting, impressionism, art nouveau, baroque
- 媒介/技术：watercolor, pencil sketch, 3D render, pixel art
- 摄影风格：cinematic, editorial, snapshot, macro photography
- 文化风格：anime, Ghibli, ukiyo-e, art deco
- 光线/色彩（也是全局的）：golden hour, neon lighting, monochrome
```

**特性**：style token 产生全局注意力，影响整体美学方向。放在 prompt 后部效果更好。多个风格词之间会融合（不是选最强的，是取平均）。

### Structure Token（结构轴）— 决定了"画面怎么组织"

```
- 构图：close-up, wide shot, bird's eye view, centered
- 空间关系：on the left, in the background, above, beside
- 镜头：85mm, wide angle lens, macro, tilt-shift
- 景深：shallow depth of field, bokeh, deep focus
```

**特性**：structure token 是最难执行的——它们要求模型精确控制空间布局。扩散模型天然不擅长精确布局（因为没有显式的几何推理）。对 layout 有严格要求时，优先考虑 ControlNet/IP-Adapter 等辅助工具，而不是纯靠 prompt。

### Quality Token（质量轴）— 大部分是"安慰剂"

```
"8K, highly detailed, masterpiece, best quality, award-winning, trending on ArtStation"
```

**真相**：
- 这些 token 在 CLIP 的 embedding 空间中，**不携带可区分的视觉信息**
- 对 SD 1.5 有微弱影响（因为训练数据中有这类标签）
- 对 SDXL、Flux、DALL-E 3、MJ v6 等新一代模型**几乎无效**
- 用户看到很多 prompt 加这些词不是因为它们有用，而是因为"所有人都这么做"

**该怎么做：**
- 如果用户要的是"高品质、精细"的画面，用**描述画面质量的具象词**代替："fine grain leather texture", "visible wood grain on the table", "individual strands of hair"
- 真实的细节描述 → 模型知道该在哪里放细节
- "highly detailed" → 模型不知道你指的"细节"到底是什么

---

## 6. 从模型原理推导的 6 条铁律

### 铁律 1：Content 在前，Style 在后

early tokens → 更强的 cross-attention → 主导"画什么"
later tokens → 较弱的全局影响 → 主导"怎么画"

### 铁律 2：具象 > 抽象，永远如此

模型没有"美感"，只有 embedding 距离。
- `an extremely beautiful scene` → embedding 接近噪声
- `autumn maple forest, golden hour light, mist rolling between trees` → 每个词都有明确的视觉对应

### 铁律 3：一个强大的风格 > 三个模糊的风格

`photorealistic, anime, oil painting` → 三个方向互相抵消
`Ghibli-inspired hand-drawn animation style` → 一个明确的方向

### 铁律 4：多主体用空间语言隔离

`a dog and a cat` → attention 冲突
`a golden retriever on the left sofa, a black cat curled up on the right armchair` → 空间分离减轻冲突

### 铁律 5：负向 Prompt 长不如准

`lowres, bad anatomy, text, watermark, blurry`（5 个精准词）效果 > 50 个词的负向列表
每个额外的负向词都在稀释其他词的"推力"

### 铁律 6：Token 预算有限——花在刀刃上

SD 1.5 只有 77 个 token 的预算。每个 `very, extremely, really` 都在浪费预算。
Flux/DALL-E 3 预算更充裕，但不意味着该浪费——高质量 prompt 的信息密度才是关键。
