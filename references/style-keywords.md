# 视觉关键词库 — 模型视角三轴分类

当用户需要关键词参考时使用。注意这不是一个"往 prompt 里塞更多词"的清单——而是帮你理解**不同词在模型处理管线中的角色差异**。挑选有实际影响的词，不堆砌。

使用前先回顾 `how-models-see.md` 中的核心原则：Content（画什么）> Structure（怎么组织）> Style（长什么样）> Quality（基本安慰剂）。

---

## 一、Content 轴 — "画面里有什么"

这些词通过 cross-attention 绑定到具体空间区域，决定画面的**内容**。越靠前、越具体越有效。

### 主体类别参考

| 类别 | 具象描述（好） | 抽象描述（弱） |
|------|-------------|-------------|
| 人物 | `elderly fisherman with weathered hands` | `an old person` |
| 动物 | `snow leopard with spotted fur crouching` | `a cool animal` |
| 建筑 | `crumbling gothic cathedral with flying buttresses` | `an old building` |
| 自然 | `ancient twisted oak tree with exposed roots` | `a tree` |
| 物品 | `tarnished brass pocket watch with roman numerals` | `an old clock` |

### 主体修饰词（按需选 1-2 个，不要堆砌）

**材质/纹理**：`weathered`, `polished`, `rough-hewn`, `velvet`, `cracked porcelain`, `rusty iron`, `worn leather`, `frosted glass`, `hammered copper`, `weathered stone`

**状态**：`overgrown with ivy`, `covered in morning dew`, `half-submerged`, `dust-covered`, `rain-soaked`, `sun-bleached`, `moss-covered`

**规模感**：`towering`, `diminutive`, `colossal`, `life-sized`, `miniature`——这些词真正影响模型的空间理解

### 环境/场景参考

| 环境类型 | 具象描述 |
|---------|---------|
| 森林 | `dense foggy pine forest at dawn`, `sunlit birch grove in autumn` |
| 城市 | `rain-slicked neon-lit street at midnight`, `sun-drenched mediterranean alleyway` |
| 室内 | `cozy library with floor-to-ceiling bookshelves and a fireplace` |
| 废墟 | `overgrown post-apocalyptic cityscape, nature reclaiming concrete` |
| 水域 | `crystal-clear alpine lake reflecting snow-capped peaks` |
| 天空 | `dramatic storm clouds parting to reveal golden light` |

---

## 二、Style 轴 — "画面长什么样"

这些词产生**全局注意力**——它们影响整个画面的美学处理方式，不绑定到某个具体区域。

### 艺术流派/媒介

| 流派 | 代表词 | 效果说明 |
|------|--------|---------|
| **写实摄影** | `photorealistic`, `documentary photography`, `35mm film` | 强：照片级真实的方向信号 |
| **电影感** | `cinematic`, `anamorphic widescreen`, `film grain` | 强：明确的布光和调色倾向 |
| **油画** | `oil painting`, `impasto texture`, `visible brushstrokes` | 中强：质感方向明确 |
| **水彩** | `watercolor wash`, `wet-on-wet technique`, `translucent layers` | 中：风格独特，部分模型支持好 |
| **水墨/国画** | `ink wash painting`, `sumi-e`, `negative space composition` | 中：Flux/SD3 支持较好 |
| **素描** | `charcoal sketch`, `pencil drawing`, `hatching and cross-hatching` | 中：线条风格方向明确 |
| **插画** | `digital illustration`, `flat vector art`, `storybook style` | 中强：方向宽泛但有效 |
| **3D渲染** | `3D render`, `octane render`, `unreal engine 5` | 弱-中：更多是审美标签 |
| **像素艺术** | `pixel art`, `8-bit aesthetic`, `sprite art` | 强：方向极为明确 |
| **拼贴** | `collage art`, `mixed media`, `cut paper style` | 中：独特且可区分 |

### 文化/美学锚点（极强——直接指向特定的视觉风格簇）

| 锚点 | 代表词 | 能级 |
|------|--------|------|
| 吉卜力 | `Studio Ghibli style`, `Hayao Miyazaki aesthetic` | ★★★★★ |
| 赛博朋克 | `Blade Runner aesthetic`, `cyberpunk neo-noir` | ★★★★★ |
| 蒸汽朋克 | `steampunk Victorian era` | ★★★★ |
| 浮世绘 | `ukiyo-e woodblock print style`, `Hokusai inspired` | ★★★★ |
| 巴洛克 | `Baroque painting style`, `chiaroscuro dramatic lighting` | ★★★★ |
| 装饰艺术 | `Art Deco geometric design` | ★★★ |
| 包豪斯 | `Bauhaus minimalism` | ★★★ |
| 超现实主义 | `surrealist painting`, `Salvador Dali inspired` | ★★★★ |
| 波普艺术 | `pop art style`, `Roy Lichtenstein comic dots` | ★★★★ |

### 摄影风格锚点

| 风格 | 代表词 | 适用场景 |
|------|--------|---------|
| 时尚大片 | `high fashion editorial photography` | 人物/服装 |
| 纪实摄影 | `documentary photojournalism`, `Magnum style` | 街头/新闻感 |
| 美食摄影 | `commercial food photography`, `soft window lighting` | 食物 |
| 产品摄影 | `clean product photography`, `studio lighting on white seamless` | 产品 |
| 微距 | `macro photography`, `extreme close-up detail` | 自然/纹理 |
| 随手拍 | `snapshot aesthetic`, `point-and-shoot camera`, `natural unposed moment` | 生活感 |

---

## 三、Structure 轴 — "画面怎么组织"

这些词控制构图和空间关系，是**最难执行**的维度——模型没有显式的几何推理能力。

### 构图与距离

| 词 | 效果 | 可靠性 |
|----|------|--------|
| `extreme close-up` | 极近特写（面部/局部） | ★★★★★ |
| `close-up portrait` | 面部特写 | ★★★★★ |
| `medium shot` | 半身/中景 | ★★★★ |
| `full body shot` | 全身 | ★★★★ |
| `wide angle landscape` | 广角全景 | ★★★★ |
| `bird's eye view` | 鸟瞰 | ★★★ |
| `worm's eye view` | 仰视 | ★★★ |
| `dutch angle` | 倾斜构图 | ★★★ |
| `top-down flat lay` | 正俯视 | ★★★★ |
| `centered composition` | 中心构图 | ★★★★ |
| `rule of thirds` | 三分法 | ★★（更像是人类偏好标签） |
| `symmetrical` | 对称 | ★★★ |
| `negative space` | 大面积留白 | ★★★ |

### 镜头光学效果

| 词 | 效果 | 可靠性 |
|----|------|--------|
| `shallow depth of field` | 背景虚化 | ★★★★★ |
| `bokeh background` | 光斑虚化 | ★★★★ |
| `wide angle lens` | 空间拉伸感 | ★★★ |
| `macro lens` | 极致细节/微距 | ★★★★ |
| `tilt-shift` | 微缩模型感 | ★★★★ |
| `fisheye lens` | 鱼眼畸变 | ★★★★ |
| `lens flare` | 镜头光晕 | ★★★ |

---

## 四、光线与色彩轴（跨 Style / Structure 的混合轴）

光线和色彩 token 往往是**全局注意力**，但在某些上下文中可以引导空间感。

### 光线类型

**自然光：**
| 词 | 视觉效果 |
|----|---------|
| `golden hour backlighting` | 暖金色逆光，轮廓发光 |
| `soft diffused overcast light` | 柔和的阴天散射光，无硬阴影 |
| `dappled sunlight through leaves` | 树叶间斑驳的光斑 |
| `dramatic storm light` | 暴风雨前/后的戏剧性光线 |
| `misty morning fog with god rays` | 晨雾中的体积光/丁达尔效应 |
| `sunset alpenglow` | 落日余晖映照山顶的粉橙色 |

**人工光：**
| 词 | 视觉效果 |
|----|---------|
| `neon tube lighting` | 霓虹灯管直射光 |
| `warm candlelight` | 烛光暖色 |
| `single overhead bulb` | 单一顶光源，强烈阴影 |
| `cinematic rim lighting` | 轮廓光，主体从背景中分离 |
| `Rembrandt lighting` | 经典的三角形面颊光 |
| `colored gel lighting` | 戏剧性彩色滤光片效果 |

### 色调速查

| 色调方向 | 词 | 适用氛围 |
|---------|-----|---------|
| 暖色系 | `warm amber tones`, `golden hour palette` | 温馨、怀旧、浪漫 |
| 冷色系 | `cool steel blue tones`, `arctic light` | 冷静、科技、孤独 |
| 青橙调 | `teal and orange color grade` | 好莱坞/电影感 |
| 低饱和 | `muted desaturated colors`, `faded film stock` | 怀旧、文艺、日系 |
| 高饱和 | `vivid hyper-saturated colors` | 波普、活力、电子感 |
| 粉彩 | `soft pastel color palette` | 梦幻、少女、春天 |
| 黑白 | `black and white`, `high contrast monochrome` | 经典、纪实、极简 |
| 复古 | `vintage sepia tone`, `faded polaroid colors` | 怀旧、70年代 |

---

## 五、Quality 轴 — 少用，精准用

> ⚠️ **大部分质量词在现代模型中效果微弱或无效。** 只有在以下情况才加入：
> 1. 用户明确要求"高品质/精细"的画面
> 2. 你搭配了具体的纹理/材质描述词，质量词作为补充信号
> 3. 用户用的是较老的模型（SD 1.5 及更早）

### 可能有微弱效果的词（在特定条件下）

| 词 | 实际作用 | 推荐用法 |
|----|---------|---------|
| `sharp focus` | 轻微提升锐度倾向 | 搭配具体的主体描述 |
| `intricate details` | 提示增加纹理密度 | 搭配具体的材质词 |

### 基本无效的词（避免滥用）

`8K`, `masterpiece`, `best quality`, `award-winning`, `trending on ArtStation`, `ultra high resolution`

这些词在 CLIP embedding 空间中几乎不携带可区分的视觉信息。它们出现在大量 prompt 中是因为社交传播的惯性，不是因为它们有效。
