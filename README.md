# 🎨 Image Prompt Crafter

> AI生图Prompt辅助Skill——不是关键词模板库，是一个**视觉翻译者**。你描述你想要的感觉，它帮你转译成模型能懂的Prompt。
>
> A prompt engineering Skill for AI image generation, built on diffusion model fundamentals — not keyword cookbooks.
***

## 🤔 这个Skill有啥用

大多数人不是没想法，是脑子里那幅画面不知道怎么变成AI能读的Prompt。

这个Skill做的事：

- **先理解**——你说的是一句话、一个词、一种情绪还是一段画面描述，它先判断类型
- **再追问**——只问1-2个能改变画面方向的关键问题，给选择题，不让用户觉得在填问卷
- **然后转译**——如果是抽象概念（孤独、自由、焦虑），走语义解析→隐喻构建→视觉转译的完整流程
- **最后输出**——根据需求复杂度，输出从简洁版到复杂多方向版的Prompt，每个版本带解释

***

## ✨ 特点

- **不是关键词模板**——不堆8K/masterpiece这种无效词，每个词都服务于画面表达
- **从模型原理出发**——理解扩散模型的编码器、注意力、降噪是怎么回事，从根上知道什么Prompt有效、什么会翻车
- **语义解析+隐喻构建**——抽象概念（孤独、自由、时间）不做空洞抽象图案，找到具体视觉载体转译成画面
- **复杂度跟着需求走**——随口说句试试就给简洁版，认真出图就给多方向精细版，不强行拉长
- **失败能诊断**——7种常见翻车模式都有定位和修复路径，不是盲目重写
- **支持多轮迭代**——ChatGPT/Gemini场景主动拆成3-4轮（骨架→血肉→气质），每轮一个目标
- **特殊场景有专项指导**——夜景、雨景、雪景、动态、水下、微距，各有技术要点
- **全程中文交流**——Prompt正文英文（适配主流模型），国内平台同步输出中文版

***

## 🖼️ 支持的平台

| 平台                          | 适配策略                               |
| --------------------------- | ---------------------------------- |
| Midjourney              | 自然语言 + `--ar` `--stylize` `--v` 参数 |
| Stable Diffusion | 逗号分隔标签+正负向Prompt+权重语法         |
| Stable Diffusion   | 自然语言或标签均可（T5编码器）                  |
| Flux                        | 自然语言描述（T5-XXL编码器）                 |
| DALL-E                  | 自然语言句子，利用多轮迭代                      |
| ChatGPT       | 对话式自然语言，逐步调整                       |
| Gemini          | 自然语言描述，可迭代                         |
| 即梦/可灵/通义万相等              | 中文自然语言                             |
| ComfyUI                     | 取决于加载的模型，按对应体系适配                   |

***

## 🚀 安装

### 方式一：让Agent帮你装（推荐）

在Claude Code、Codex、OpenClaw等支持Skill的Agent里，直接说：

```
帮我安装这个 skill：image-prompt-crafter
```

Agent会自己找到仓库、clone到对应目录，不用你操心路径。

### 方式二：Git Clone

```bash
# Trae IDE
git clone https://github.com/你的用户名/image-prompt-crafter.git ~/.trae-cn/skills/image-prompt-crafter

# Claude Code
git clone https://github.com/你的用户名/image-prompt-crafter.git ~/.claude/skills/image-prompt-crafter
```

### 方式三：手动复制

将整个 `image-prompt-crafter` 文件夹复制到skills目录：

```bash
cp -r image-prompt-crafter ~/.trae-cn/skills/
```

安装后重启或新建对话，Skill自动加载。

***

## 🧠 核心理念

这个 Skill 底层是一套从扩散模型管线推出来的 Prompt 工程框架：

- **Text Encoder 怎么把词变成向量**——为什么CLIP模型用逗号分词、T5模型用自然语句，不是玄学，是编码器架构决定的
- **Cross-Attention 怎么把token绑到画面区域**——为什么「红色的猫和蓝色的狗坐在一起」会翻车（属性绑定混乱），怎么用空间词隔离修复
- **Token位置效应**——为什么 prompt 开头的词权重最高，为什么关键词放前面是在浪费最珍贵的注意力
- **CFG怎么引导降噪方向**——为什么某些「质量词」反而让画面变塑料，用具体的材质描述替代空泛形容

搞懂了“为什么”，换什么平台都能写出对的Prompt。


***

## 📄 License

MIT
