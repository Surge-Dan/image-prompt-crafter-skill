# 各平台 Prompt 语法指南

不同平台的语法差异不是随机的——根源在于它们使用的 **Text Encoder 架构不同**。理解这一点后，你不需要死记硬背每个平台的规则。

## 为什么不同平台的 Prompt 策略完全不同

| 平台 | Text Encoder | Prompt 风格 | 原因 |
|------|-------------|------------|------|
| **SD 1.5** | CLIP ViT-L/14 | 逗号分隔关键词 | CLIP 在 alt-text 图文对上训练，习惯"标签式"描述；77 token 硬限制 |
| **SDXL** | CLIP-L + OpenCLIP bigG | 逗号分隔为主，可略带自然语言 | 双编码器互补，上限更高但仍偏标签风格 |
| **SD 3 / 3.5** | 双 T5 + CLIP | 自然语言或标签均可 | T5 编码器天然处理自然语言 |
| **Flux** | T5-XXL + CLIP-L | 自然语言描述 | T5 提供强大的句子理解，CLIP 提供视觉对齐 |
| **DALL-E 3** | 自研 T5-XXL 变体 | 自然语言句子 | 专为图像生成微调的自然语言编码器 |
| **ChatGPT 生图** | GPT-4o 原生多模态 | 对话式自然语言 | 端到端多模态模型，不是"编码→扩散"的两段式 |
| **Midjourney v6** | 自研（推测 T5 混合） | 自然语言 + 参数 | 自研编码器，对氛围/情绪词极度敏感 |
| **Gemini** | Gemini 原生多模态 | 对话式自然语言 | 同 ChatGPT，端到端多模态 |
| **即梦/可灵/通义** | 自研/混合 | 自然语言（中英文均可） | 国内团队自研，中文优化 |

**关键推论：** 对于 T5 系模型（Flux、SD3、DALL-E 3），**不需要**把 prompt 写成逗号分隔的标签——写完整、清晰的句子效果更好。对于 CLIP 系模型（SD 1.5、SDXL），标签式写法贴合其训练分布。

---

## Midjourney (MJ)

### 基本特点
- 接受**自然语言描述**，不需要逗号分隔
- 对形容词和氛围词的理解较好
- 长度建议 40-80 词，太长的 prompt 权重稀释

### 常用参数
| 参数 | 用途 | 示例 |
|------|------|------|
| `--ar [W]:[H]` | 画面比例 | `--ar 16:9`, `--ar 3:4`, `--ar 1:1` |
| `--stylize [0-1000]` | 风格化程度，默认100 | `--stylize 250`（高风格化） |
| `--chaos [0-100]` | 创意/随机度 | `--chaos 30`（四张差异更大） |
| `--no [element]` | 排除元素 | `--no text, watermark` |
| `--v [version]` | 模型版本 | `--v 6.1` |
| `--q [0.25/0.5/1]` | 生成质量 | `--q 1`（最高质量） |
| `--style raw` | 减少MJ自动美化 | 用于写实摄影 |

### Prompt 风格建议
- 主语在前、氛围词在后
- 可以加入摄影师/艺术家名字作为风格参考
- 末尾加参数
- **示例：** `a cozy wooden cabin nestled in a misty pine forest at dawn, warm golden light spilling from the windows, cinematic atmosphere, soft morning fog --ar 16:9 --stylize 200 --v 6.1`

---

## Stable Diffusion (SD) / SDXL

### 基本特点
- 对**逗号分隔的关键词标签**反应最好（Danbooru tagging 风格）
- 需要正向 prompt + 负向 prompt
- 可以用 `(keyword)` 加括号提高权重，`[keyword]` 降低权重
- SDXL 可以直接用自然语言，但仍然推荐逗号分隔

### 权重语法
| 写法 | 效果 |
|------|------|
| `mountain` | 正常权重 |
| `(mountain)` | 权重 × 1.1 |
| `(mountain:1.3)` | 权重 × 1.3 |
| `[mountain]` | 权重 × 0.9 |
| `((mountain))` | 等效 `(mountain:1.21)` |

### 标准负向 Prompt（通用模板）
```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, deformed, disfigured, poorly drawn, mutation, ugly
```

### Prompt 风格建议
- 关键词用逗号分隔
- 质量词开头：`masterpiece, best quality,`
- 主体详述放在靠前位置
- 风格词放在靠后位置
- **示例：**
  - **正向：** `masterpiece, best quality, highly detailed, a cozy wooden cabin in a misty pine forest, warm golden light from window, dawn atmosphere, soft morning fog, cinematic composition, 8K`
  - **负向：** `lowres, bad anatomy, text, error, blurry, deformed, watermark`

---

## DALL-E 3 (OpenAI)

### 基本特点
- 接受**完整的自然语言句子**
- 不需要特殊参数标签
- prompt 会经过内部改写，所以关键元素要明确
- 支持的风格范围较窄，偏向安全和写实

### Prompt 风格建议
- 用完整句子，像在和摄影师描述需求
- 清晰说明主体、环境、光线
- 避免复杂的主体交互（DALL-E 在处理精确的动作时有局限）
- 如果生成不理想，追加更多细节
- **示例：** `A cozy wooden cabin nestled deep in a misty pine forest at dawn. Warm golden light spills from the cabin windows, creating a contrast with the cool blue morning fog. Soft volumetric light rays filter through the trees. Photorealistic, highly detailed.`

### 注意事项
- 对真实人物生成限制严格
- 对暴力/恐怖内容拒绝生成
- 文字渲染能力弱，不要期望在图片中加入可读文字

---

## ChatGPT 生图 (GPT-4o)

### 基本特点
- 基于 GPT-4o 的原生图像生成能力，通过对话直接生成图片
- 接受**自然语言对话式描述**，像和人说话一样描述即可
- 支持多轮对话迭代——可以"把猫换成狗""背景改成海边"
- 对中文 prompt 的理解也很好
- 有较强的风格跟随能力

### Prompt 风格建议
- 自然语言描述，不用特殊标签
- 可以在一个请求里包含多个画面要素
- 利用多轮对话来逐步调整画面，不追求一次到位
- **示例：** `帮我画一张早晨的森林小木屋，木屋被雾气包围，阳光透过松树洒下来，窗户里透着暖黄色的光。画面要有电影感。`

### 注意事项
- 有内容安全限制，和 DALL-E 类似的审核机制
- 对真实公众人物和版权角色的生成有限制
- 可以通过"再改改 XX 部分"来迭代优化

---

## Gemini 生图

### 基本特点
- Gemini 2.0 Flash 及以上版本支持原生图像生成
- 支持**自然语言描述**，中文和英文都行
- 可以通过对话连续生成多张相关图片
- prompt 跟随力较强，尤其在细节执行上
- 目前仍在持续更新，能力边界逐渐扩展

### Prompt 风格建议
- 自然语言描述，不需要特殊参数
- 可以给出清晰的主体-环境-风格框架
- 利用对话能力先描述再迭代
- **示例：** `Generate an image of a wooden cabin in a misty pine forest at dawn, warm golden light streaming through the windows, soft fog rolling between the trees, cinematic atmosphere.`

### 注意事项
- 图像生成功能和可用区域可能因政策而异
- 对敏感内容的生成有限制
- 建议给出清晰明确的描述，模糊的指令结果稳定性较差

---

## ComfyUI / 工作流类

### 基本特点
- 底层通常跑 SD/SDXL/Flux 模型
- prompt 语法与 SD 类似
- 用户可能已经有自己的工作流和节点设置

### Prompt 建议
- 按照 SD 的逗号分隔风格
- 如果用户有特定 LoRA 或模型需求，确认后适配
- 不需要额外参数——ComfyUI 的参数通过节点设置

---

## 国内平台

### 即梦 (Jimeng)
- 字节跳动出品，支持中文 prompt
- 自然语言描述为主
- 对写实人物和风景有较好表现
- 风格接近 DALL-E / Midjourney 的混合体

### 可灵 (Kling)
- 快手出品，视觉风格偏向短视频/社交媒体美学
- 支持中文和英文 prompt
- 自然语言描述即可

### 通义万相 (Tongyi Wanxiang)
- 阿里云出品，支持中文 prompt
- 自然语言描述
- 对国风、东方美学类内容理解较好

---

## 其他平台

### Flux (Black Forest Labs)
- 支持自然语言描述
- prompt 跟随力强，不需要特殊的 tagging 风格
- 推荐使用自然语言，但也可以在末尾追加逗号分隔的质量词
- **示例：** `A cozy wooden cabin in a misty pine forest at dawn, warm light from windows, soft morning fog, cinematic atmosphere`

---

## 通用版 Prompt（用户未指定平台时）

当用户不知道或不关心平台时，输出一个通用格式：

1. **主 prompt**：自然语言写法（英文），兼容 Midjourney / DALL-E / ChatGPT / Gemini / Flux / 国内平台
2. 不附加任何平台专属参数标签（不写 --ar、--stylize 等，不写权重括号）
3. 如果用户是中文使用者且用国内平台（即梦、可灵等），可以输出中文 prompt
4. 标注"适用于各大 AI 生图平台"

如果用户后续选择了平台，再按需追加对应参数。
