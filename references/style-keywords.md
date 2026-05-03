# 视觉关键词库 — 模型视角五轴分类

当用户需要关键词参考时使用。注意这不是一个"往 prompt 里塞更多词"的清单——而是帮你理解**不同词在模型处理管线中的角色差异**。挑选有实际影响的词，不堆砌。

使用前先回顾 `how-models-see.md` 中的核心原则：Content（画什么）> Structure（怎么组织）> Style（长什么样）> Color（什么色调）> Quality（基本安慰剂）。

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
| 食物 | `steaming bowl of ramen with a perfectly soft-boiled egg sliced in half` | `some food` |
| 交通工具 | `rust-patched vintage Volkswagen bus from the 1970s, faded orange paint` | `an old car` |

### 主体修饰词（按需选 1-2 个，不要堆砌）

**材质/纹理**：`weathered`, `polished`, `rough-hewn`, `velvet`, `cracked porcelain`, `rusty iron`, `worn leather`, `frosted glass`, `hammered copper`, `weathered stone`, `raw concrete`, `oxidized bronze`, `burnished wood`, `patinated copper`

**状态**：`overgrown with ivy`, `covered in morning dew`, `half-submerged`, `dust-covered`, `rain-soaked`, `sun-bleached`, `moss-covered`, `frost-bitten`, `wind-battered`, `eroded by time`

**规模感**：`towering`, `diminutive`, `colossal`, `life-sized`, `miniature`, `imposing`, `dwarfing everything around it`——这些词真正影响模型的空间理解

**年龄感**：`brand new`, `freshly painted`, `faded with age`, `centuries-old`, `showing its years`, `weathered by decades of use`

**情感性外观**：不直接说"happy"，说 `with a barely contained smile tugging at the corner of her mouth`；不直接说"tired"，说 `with the kind of exhausted stillness that comes after a 14-hour shift`

### 环境/场景参考

| 环境类型 | 具象描述 |
|---------|---------|
| 森林 | `dense foggy pine forest at dawn`, `sunlit birch grove in autumn`, `ancient moss-covered temperate rainforest` |
| 城市 | `rain-slicked neon-lit street at midnight`, `sun-drenched mediterranean alleyway`, `deserted financial district at 3am` |
| 室内 | `cozy library with floor-to-ceiling bookshelves and a fireplace`, `minimalist Japanese apartment with tatami and sliding shoji screens` |
| 废墟 | `overgrown post-apocalyptic cityscape, nature reclaiming concrete`, `abandoned 1950s diner with peeling chrome and cracked vinyl` |
| 水域 | `crystal-clear alpine lake reflecting snow-capped peaks`, `turbulent ocean during an approaching storm, whitecaps and dark sky` |
| 天空 | `dramatic storm clouds parting to reveal golden light`, `vast desert night sky dense with stars and the faint band of the milky way` |
| 工业 | `rusting steel factory interior with broken windows and scattered machinery`, `active cargo port at night under orange sodium vapor floodlights` |
| 乡村 | `golden wheat field at harvest time under a blazing midday sun`, `quiet English countryside village with stone cottages and a winding lane` |

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
| **丝网印刷** | `screenprint aesthetic`, `risograph texture`, `offset lithography` | 中强：独特的印刷质感和色彩分离 |
| **版画** | `woodblock print`, `linocut style`, `copperplate etching` | 中强：强烈的线条和对比 |
| **涂鸦/街头** | `graffiti mural`, `spray paint texture`, `stencil art` | 中：方向明确，适合城市场景 |

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
| 印象派 | `Impressionist painting`, `Monet-inspired soft focus` | ★★★★ |
| 新艺术运动 | `Art Nouveau style`, `Alphonse Mucha decorative lines` | ★★★★ |
| 日式侘寂 | `wabi-sabi aesthetic`, `imperfect weathered beauty` | ★★★★ |
| 复古未来主义 | `retro-futurism`, `1970s vision of the future` | ★★★ |
| 暗黑学院 | `dark academia aesthetic`, `moody old library atmosphere` | ★★★ |
| 中式国潮 | `new Chinese style`, `Eastern aesthetic meets modern design` | ★★★ |

### 摄影风格锚点

| 风格 | 代表词 | 适用场景 |
|------|--------|---------|
| 时尚大片 | `high fashion editorial photography` | 人物/服装 |
| 纪实摄影 | `documentary photojournalism`, `Magnum style` | 街头/新闻感 |
| 美食摄影 | `commercial food photography`, `soft window lighting` | 食物 |
| 产品摄影 | `clean product photography`, `studio lighting on white seamless` | 产品 |
| 微距 | `macro photography`, `extreme close-up detail` | 自然/纹理 |
| 随手拍 | `snapshot aesthetic`, `point-and-shoot camera`, `natural unposed moment` | 生活感 |
| 人像摄影 | `portrait photography`, `85mm lens`, `shallow depth of field` | 人物 |
| 风光摄影 | `landscape photography`, `wide angle`, `golden hour` | 自然 |
| 街拍 | `street photography`, `candid moment`, `35mm focal length` | 城市生活 |
| 胶片摄影 | `analog film photography`, `Portra 400`, `film grain`, `light leaks` | 怀旧/文艺 |

### 特殊视觉效果

| 效果 | 代表词 | 适用场景 |
|------|--------|---------|
| 双重曝光 | `double exposure`, `ghostly layered imagery` | 梦幻/记忆/超现实 |
| 漏光 | `film light leak`, `accidental overexposure on the edge` | 胶片复古感 |
| 慢快门 | `long exposure`, `light trails`, `motion blur` | 夜景/动态/城市 |
| 红外摄影 | `infrared photography`, `surreal pink foliage` | 超现实自然 |
| 移轴 | `tilt-shift photography`, `miniature model effect` | 微缩世界感 |
| 鱼眼 | `fisheye lens`, `extreme barrel distortion` | 实验/青年文化 |
| 柔焦 | `soft focus`, `dreamy haze filter`, `vaseline on lens effect` | 梦幻/浪漫/复古 |

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
| `diagonal composition` | 对角线构图 | ★★★ |
| `frame within a frame` | 框式构图（门框/窗户框住主体） | ★★★ |
| `leading lines` | 引导线构图 | ★★ |
| `face filling the entire frame` | 面部占满画面 | ★★★★ |
| `full frame composition, no empty space` | 紧凑构图、无留白 | ★★★ |

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
| `anamorphic lens flare` | 宽银幕横向拉丝光晕 | ★★★ |
| `vignette` | 边缘暗角 | ★★★★ |
| `chromatic aberration` | 色散/紫边效果 | ★★★ |
| `razor-thin plane of focus` | 极薄景深（微距感） | ★★★ |

---

## 四、Lighting 轴 — "光线怎么打"

光线是画面情绪的第一载体。一句好的光线描述胜过三句氛围形容词。

### 自然光

| 词 | 视觉效果 |
|----|---------|
| `golden hour backlighting` | 暖金色逆光，轮廓发光 |
| `soft diffused overcast light` | 柔和的阴天散射光，无硬阴影 |
| `dappled sunlight through leaves` | 树叶间斑驳的光斑 |
| `dramatic storm light` | 暴风雨前/后的戏剧性光线，暗云+金色边缘 |
| `misty morning fog with god rays` | 晨雾中的体积光/丁达尔效应 |
| `sunset alpenglow` | 落日余晖映照山顶的粉橙色 |
| `harsh midday sun directly overhead` | 正午顶光，短而深的阴影 |
| `blue hour twilight` | 日落后的深蓝时刻，天空是靛蓝色但还有微光 |
| `late afternoon sun streaming through a window at a low angle` | 低角度窗光，拉长的影子和温暖色温 |
| `winter morning light, cold and crisp with long blue shadows` | 冬日清晨光，冷色调、长阴影 |
| `sunlight filtered through sheer curtains` | 透过纱帘的柔和日光，朦胧质感 |
| `the last shaft of sunset light catching only the very top of a subject` | 最后的落日余晖只照亮物体最高处 |

### 人工光

| 词 | 视觉效果 |
|----|---------|
| `neon tube lighting` | 霓虹灯管直射光 |
| `warm candlelight` | 烛光暖色，闪烁不均 |
| `single overhead bulb` | 单一顶光源，强烈垂直阴影 |
| `cinematic rim lighting` | 轮廓光，主体从暗背景中分离 |
| `Rembrandt lighting` | 经典的三角形面颊光——肖像摄影 |
| `colored gel lighting` | 戏剧性彩色滤光片效果 |
| `fluorescent overhead lighting` | 荧光灯顶光——冷白、均匀、clinical |
| `sodium vapor streetlight` | 钠灯街光——橙色、不均匀、老旧城市感 |
| `practical lamps in the scene` | 场景中的台灯/壁灯——光从场景内发出 |
| `softbox studio lighting` | 柔光箱——均匀、柔和的商业摄影光 |
| `split lighting, one side illuminated one side in complete shadow` | 分光——半脸光半脸暗，戏剧张力 |
| `a single TV screen illuminating a dark room in flickering blue` | 电视荧幕光——蓝色闪烁照亮暗室 |
| `warm tungsten interior lights spilling through a window from inside` | 室内钨丝灯暖光从窗内溢出 |

### 氛围光

| 词 | 视觉效果 |
|----|---------|
| `volumetric light rays, dusty atmosphere` | 体积光——空气中可见的光束路径 |
| `chiaroscuro, strong contrast between light and shadow` | 明暗对照法——强光影对比 |
| `silhouette against a bright background` | 逆光剪影 |
| `rim light separating subject from dark background` | 轮廓光分离主体和暗背景 |
| `ambient glow, soft light wrapping around the subject` | 环境柔光包裹主体 |
| `high key, bright and airy with minimal shadows` | 高调——亮、通透、少阴影 |
| `low key, dark and moody with deep rich shadows` | 低调——暗、深沉、浓郁阴影 |

---

## 五、Color 轴 — "什么色调"

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
| 深棕+金 | `rich brown and warm gold palette` | 古典、奢华、秋日 |
| 霓虹色系 | `neon pink and electric cyan palette` | 赛博朋克、synthwave、电子 |
| 大地色 | `earthy terracotta and sage green palette` | 自然、手工、侘寂 |
| 海洋色 | `deep navy and crisp white with touches of coral` | 航海、清新、地中海 |
| 日系杂志 | `Japanese editorial color — muted pastels with warm undertones` | 日系时尚、温柔 |
| 莫兰迪 | `Morandi palette — dusty muted tones, greyed-down colors` | 高级克制、文艺静谧 |

### 强调色方案（黑/白/灰 + 一个强调色）

| 基底 | 强调色 | 效果 |
|------|--------|------|
| 暖白纸底 | 朱红 (vermilion red) | 日本书道/印章感 |
| 纯白 | 克莱因蓝 (Klein blue) | 现代艺术、概念海报 |
| 炭灰 | 荧光黄 (neon yellow) | 工业/警示/青年文化 |
| 象牙白/奶油 | 金 (gold foil) | 奢华、邀请函、高级感 |
| 纯黑 | 单一纯白元素 | 极致对比、沉重主题 |

---

## 六、Quality 轴 — 少用，精准用

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

### 真正有效的"品质"替代方案

不说 `8K highly detailed`，说具体纹理：
- `visible wood grain on the aged oak desk`
- `individual strands of silver hair catching the light`
- `fine grain leather with subtle cracking at the stress points`
- `the weave of the linen tablecloth visible in close detail`
- `tiny scratches and micro-abrasions on the polished metal surface`
