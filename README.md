# 🎨 Image Prompt Crafter

> 基于扩散模型底层原理的 AI 生图 Prompt 辅助 Skill。帮不会写 Prompt 的人，写出高质量的图像生成描述。
>
> A prompt engineering Skill for AI image generation, built on diffusion model fundamentals — not keyword cookbooks.

***

## 🤔 解决了什么问题

大多数人用 AI 生图时只会写「森林里的小木屋」「一只猫在椅子上」这种极简描述。不是他们没想法，而是脑海中的画面不知道怎么翻译成 AI 能理解的 Prompt。

这个 Skill 像一个有经验的朋友，通过**引导式采访**帮你把模糊想法变成精确、可执行的高质量 Prompt。

***

## ✨ 特点

- **引导式采访** — 不问技术术语，用日常语言渐进式追问，帮你补全缺失的视觉信息
- **模型底层原理驱动** — 理解 CLIP/T5 编码器、交叉注意力机制、CFG 引导的工作原理，从根上知道什么 Prompt 有效
- **复杂度匹配需求** — 快速尝鲜出简洁 Prompt，专业创作出精细 Prompt，不强制长篇大论
- **反模板思维** — 不机械堆砌 8K/masterpiece 等无效质量词，根据具体画面挑选真正需要的描述
- **诊断框架** — 生成的图不对？系统性地定位问题根因，精准修复而非盲目重写
- **中文优先** — 交互全程中文，Prompt 正文英文（适配主流生图模型），国内平台支持中文输出
- **创新引导** — 需求常规时主动提供跳脱惯性思维的创意方向

***

## 🖼️ 支持的平台

| 平台                          | 适配策略                               |
| --------------------------- | ---------------------------------- |
| Midjourney v6               | 自然语言 + `--ar` `--stylize` `--v` 参数 |
| Stable Diffusion 1.5 / SDXL | 逗号分隔标签 + 正负向 Prompt + 权重语法         |
| Stable Diffusion 3 / 3.5    | 自然语言或标签均可（T5 编码器）                  |
| Flux                        | 自然语言描述（T5-XXL 编码器）                 |
| DALL-E 3                    | 自然语言句子，利用多轮迭代                      |
| ChatGPT 生图 (GPT-4o)         | 对话式自然语言，逐步调整                       |
| Gemini 2.0 Flash            | 自然语言描述，可迭代                         |
| 即梦 / 可灵 / 通义万相              | 中文自然语言                             |
| ComfyUI                     | 取决于加载的模型，按对应体系适配                   |

***

## 📁 文件结构

```
image-prompt-crafter/
├── SKILL.md                        ← 核心工作流（复杂匹配 + 引导采访 + 诊断模式）
├── evals/
│   └── evals.json                  ← 5 个测试用例（快速尝鲜 / 模糊需求 / 平台适配 / 诊断优化）
└── references/
    ├── how-models-see.md           ← 扩散模型底层原理（Encoder → Cross-Attention → Denoising）
    ├── prompt-debugging.md         ← 诊断框架（5 类问题诊断树 + 快速预判表）
    ├── style-keywords.md           ← 视觉关键词库（Content / Style / Structure 三轴分类）
    ├── platform-guide.md           ← 平台语法指南（含架构差异表）
    └── prompt-anatomy.md           ← Prompt 结构解析（三段结构 + 最小有效 Prompt + 反模式）
```

***

## 🚀 安装

### Trae IDE / Claude Code

将 `image-prompt-crafter` 文件夹复制到 skills 目录：

```bash
# Trae IDE
cp -r image-prompt-crafter ~/.trae-cn/skills/

# Claude Code
cp -r image-prompt-crafter ~/.claude/skills/
```

重启或新建对话后 Skill 自动加载。

***

## 💡 使用示例

### 快速尝鲜

> 帮我随便写个 prompt 试试效果，画风景就行

```
试试这个：
A peaceful mountain lake at sunrise, mist rolling over calm water, soft golden light

把 sunrise 换成 autumn morning 换换感觉
```

### 模糊需求 + 追问

> 我想生成一张森林里的小木屋的图

Skill 会追问风格偏好和光线方向，然后给出 2-3 个差异化变体：

- **写实摄影方向** — 清晨黄金光线 + 雾气 + 石径
- **童话插画方向** — 蘑菇花园 + 萤火虫 + 水彩线条
- **电影感方向** — 冷暖光对比 + 溪水倒影 + 宽银幕

### 诊断优化

> 我写的 prompt 生成的图不太对：`masterpiece, best quality, 8K, a cat on a chair`

Skill 会分析：主体在第 12 个 token 才出现（前面全是无效噪声词），cat 和 chair 太泛缺乏视觉锚点。然后输出修复版 + 改进对比。

***

## 🧠 核心理念

这个 Skill 的底层不是一个「关键词词库」，而是一套模型感知的 Prompt 工程框架。

它教 AI 理解扩散模型真正的处理管线：Text Encoder 如何把词变成向量，Cross-Attention 如何把 token 绑定到图像区域，CFG 如何引导去噪方向。

**理解了「为什么」，才能在任何新场景、新模型下写出对的 Prompt。**

***

## 📄 License

MIT
