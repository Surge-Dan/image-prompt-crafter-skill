# Prompt 结构解析 — 视觉翻译者方法论

这篇文档展示从「用户想要什么」到「Prompt 长什么样」的完整转译过程。**每一种输出策略的选择，都取决于你要表达的东西是什么。**

---

## 两种终极方向

生图 Prompt 的质量不在长短，在于是否准确地传达了该传达的东西。

### 方向 A：极简克制 — 概念驱动的海报/设计

适用于词义转译、海报设计、品牌视觉、艺术表达。目标不是「画得像」，而是「想得深」。

**核心方法：语义解析 → 隐喻构建 → 视觉转译**

以「孤独」为例：

**Step 1：语义解析**
- 字面义：独自一人，无人陪伴
- 情绪气质：冷、轻、空、静、慢
- 隐含象征：被遗忘、不被理解、与世界的断裂
- 语义张力：孤独不是「一个人」，而是「一个人 + 某种缺席」

**Step 2：隐喻构建（多想几个，选最锋利的）**
- 人群中只有你的伞在滴水 → 孤独是「和别人不一样」
- 一只被翻过来的乌龟，旁边没有人 → 孤独是「需要帮助但无人回应」
- 一张长桌，只有最角落有一副用过的餐具 → 孤独是「曾经有人但现在没有了」

**Step 3：视觉转译**
```
A long empty dining table seen from above, only one place setting at the far corner with a half-empty glass and a crumpled napkin, the rest of the table bare and dark, a single overhead light casting a cold pool of illumination on the lone setting, the surrounding space falling into shadow, moody stillness
```

没有「lonely」这个词，但每一个元素都在说孤独。

**Step 4：如果这是海报（加入文字共生）**
```
A bold conceptual poster design, the word "LONELY" typeset in large thin sans-serif letters across the upper half, beneath the text a long empty dining table seen from above, only one place setting at the far corner, the rest bare and dark, a single overhead light casting a cold pool on the lone setting, Bauhaus graphic art style, minimal color palette of charcoal and cream, clean geometric composition
```

文字和图像组成一句完整的视觉句子。

---

#### 完整案例 1：「焦虑」— 从词到极简海报

**Step 1：语义解析**
- 字面义：对未来的过度担忧，心神不宁
- 情绪气质：热、紧、快、闷
- 语义张力：焦虑是「对还没发生的事情的反应」——人被困在一个不存在的未来里

**Step 2：隐喻构建**

候选隐喻：
- ✗ 一个人抓头发（太直白）
- ✗ 一团乱线笼住一个人（太常见）
- ✓ 一支笔被反复按压但不出墨，纸已经划破——焦虑是「使劲但无产出」

**Step 3：视觉转译（纯图像版）**
```
A close-up view of a ballpoint pen pressed hard against a piece of paper, the pen tip has repeatedly scratched the same spot over and over, the paper is worn thin and torn through from the repetition, hundreds of tiny overlapping circular scratches visible in the damaged area, the hand gripping the pen is tense with white knuckles, harsh overhead fluorescent light casting a clinical pallor, shallow depth of field
```

**Step 4：海报版（加入文字共生）**
```
A bold conceptual poster, the word "ANXIETY" typeset in large distressed sans-serif letters, the letterforms appear to be vibrating slightly as if repeatedly traced over, beneath the word a close-up of a ballpoint pen pressed hard into paper that has been worn through from obsessive repeated scratching in the same spot, hundreds of tiny overlapping circular marks visible in the torn area, a tense white-knuckled hand gripping the pen, harsh clinical fluorescent light, raw aggressive graphic design, black and white with a single accent of urgent red, screenprint texture
```

---

#### 完整案例 2：「希望」— 从词到极简图像

**Step 1：语义解析**
- 字面义：对未来的积极期待
- 情绪气质：轻、暖、向上、开放
- 语义张力：希望不是「确信会好」，而是「不确定，但还愿意再试」

**Step 2：隐喻构建**

候选隐喻：
- ✗ 日出（太老套）
- ✗ 一只手伸向光（太常见）
- ✓ 一颗种子从一本合上的书里发芽，书页被根茎撑开——希望是「被封存的东西重新活过来」

**Step 3：视觉转译**
```
A close-up of a thick closed hardcover book lying on a sunlit windowsill, a tiny green seedling has pushed its way out from between the tightly pressed pages, the leaves fresh and dewy catching the morning sun, the pages slightly warped and lifted from the pressure of growth within, a fine film of dust on the book cover contrasting with the vibrant young sprout, soft morning light, shallow depth of field focused on the delicate stem
```

---

### 方向 B：极致精细 — 氛围驱动的写实/摄影

适用于氛围叙事、人物肖像、场景还原、生活切片。目标不是「概念强」，而是「沉浸感强」。

**核心方法：构建一个完整的时空切片**

以「午后的咖啡店」为例，同样是三句话可以写成三个层级：

**层级 1：信息罗列（不好）**
```
A coffee shop interior, afternoon light, a woman sitting
```

**层级 2：完整画面（还行）**
```
A quiet coffee shop in mid-afternoon, warm sunlight streaming through a large window, a young woman sitting alone at a corner table reading a book
```

**层级 3：时空切片（好）——加入材质、尺度、氛围、不完美**
```
A quiet neighborhood coffee shop around 3pm, warm afternoon sunlight streaming diagonally through a dusty windowpane creating visible light beams, a young woman in her twenties sitting at a corner table with a half-finished latte and a worn paperback, her fingers absentmindedly tracing the rim of the ceramic cup, soft jazz barely audible, film photography aesthetic with subtle grain and slightly warm color cast, candid intimate atmosphere
```

层级 3 的差异在于：它不是描述「有什么」，而是构建了一个**特定的时刻**。有时间（3pm）、有光线路径（dusty windowpane creating visible beams）、有动作细节（absentmindedly tracing the rim）、有环境层次（soft jazz barely audible）、有质感选择（film photography with subtle grain）。

---

#### 时空切片的 5 个构建层

构建一个完整的时空切片，从底层到顶层依次叠加：

**第 1 层：时间锚点（WHEN）**
不说「下午」，说「3pm on a Tuesday in late October」
- 具体时刻 → 决定了光线的角度、色温、阴影长度
- 具体季节 → 决定了植被状态、天空颜色、空气质感
- 具体到星期几 → 决定了场景里的人流密度和节奏

**第 2 层：空间锚点（WHERE）**
不说「屋里」，说「the corner booth of a family-run diner that hasn't been renovated since 1987」
- 空间的具体特征 → 决定了材质语言和环境层次
- 空间的年龄感 → 决定了磨损、褪色、修补痕迹
- 空间的功能 → 决定了物品的摆放逻辑

**第 3 层：光线设计（HOW LIT）**
不说「很亮」，说「late afternoon sun cutting through venetian blinds in razor-sharp parallel lines across the tabletop」
- 光源类型（自然/人工/混合）
- 光线路径（直射/散射/反射）→ 设定了明暗对比的基调
- 光的颜色和质感（金色/冷白/荧光绿）

**第 4 层：材质/质感（HOW IT FEELS）**
每个表面有自己独特的光反射特性：
- 皮肤：取决于年龄、光照角度、是否有汗水/油脂
- 织物：棉的散射 vs 丝绸的定向反射 vs 牛仔的粗糙纹理
- 金属：拉丝的不规则反射 vs 抛光的镜面反射 vs 生锈的漫反射
- 玻璃：干净的通透 vs 沾了水珠的折射 vs 磨砂的半透
- 木材：抛光的反光 vs 未处理的哑光 vs 风化开裂的纹理

**第 5 层：人的痕迹（THE HUMAN TRACE）**
人是画面的灵魂，但人可以通过痕迹出现而不需要真正出现在画面中：
- 直接出现：人物的姿态、表情、动作、衣着
- 间接出现：折叠一半的报纸、推到一边的椅子、桌上两杯只喝了一口的咖啡（暗示两个人刚刚一起坐在这里）

---

#### 完整案例 3：从三个句子到一个时空切片

**用户说：** "帮我写一张傍晚厨房的温馨画面"

**第 1 层——时间锚点：**
`Around 6:45pm on a late autumn evening, the sun has already set but the sky outside the window is still glowing a deep indigo`

**第 2 层——空间锚点：**
`A modest kitchen in an old apartment, cream-colored ceramic tiles on the walls with faint hairline cracks, a wooden countertop worn smooth in the center from decades of meal prep`

**第 3 层——光线设计：**
`The only light source is a single warm bulb in a brass pendant fixture above the counter, casting a soft amber pool that barely reaches the edges of the room, the corners fading into warm shadow`

**第 4 层——材质/质感：**
`A cast iron pot sits on the stove with condensation beading on the lid, steam escaping in lazy curls, a wooden cutting board bears the darkened stains of years of use, a cotton dish towel draped over the oven handle`

**第 5 层——人的痕迹：**
`A single place setting on the small kitchen table — one plate, one glass, one set of chopsticks resting on a ceramic rest, a radio on the windowsill playing quietly, tuned to an old jazz station`

**组合成一个完整的 Prompt：**
```
A modest kitchen in an old apartment around 6:45pm on a late autumn evening, the sky outside the window glowing deep indigo after sunset, a single warm bulb in a brass pendant above the counter casting a soft amber pool, a cast iron pot on the stove with condensation beading on the lid and lazy curls of steam escaping, a well-worn wooden cutting board stained from years of use, a single place setting on the small table — one plate, one glass, chopsticks on a ceramic rest, a radio on the windowsill playing softly to an empty room, the edges of the kitchen fading into warm shadow, quiet intimacy, shot on Portra 400 film with natural grain
```

---

### 精细写实的完整范例

以下是一个极致的细节驱动 Prompt，展示如何用精确的视觉描述构建一个完整的、有质感的、有「人味」的时空切片：

```
A snapshot taken with an old digital camera, lens angled downward from above, a young woman in her early twenties riding a public escalator descending, she looks up toward the lens (approximately 24mm focal length) with a subtle faint smile, her makeup style reminiscent of East Asian social media influencers, fair foundation, heavy blush, glossy lip tint, a beauty mark below her right eye, her hair a voluminous platinum blonde short bob with defined bangs, she wears a white sheer chiffon cardigan over a deep V-neck white top adorned with tiny red cherry prints revealing a striking neckline, her exposed chest and neck skin fair and delicate catching a sheen under the cold fluorescent overhead lights, a dark red leather handbag slung over her shoulder, a fine delicate necklace resting beside it, the photo is candid and slightly tilted (Dutch angle), mimicking amateur smartphone photography, in the background diagonally oriented bright blue escalator handrails, metal stairs with yellow warning lines, departing pedestrians, the image exhibits raw authenticity with intentionally preserved imperfections, visible digital noise and grain, flat artificial lighting, a retro orange date stamp in the bottom left corner ("260428"), radiating an intimate nostalgic urban film leftover atmosphere, 3:4 aspect ratio
```

这个 Prompt 的力量来自：
- **技术参数具象化**：24mm focal length、Dutch angle、3:4 ratio——不是标签，是描述
- **材质层层叠加**：sheer chiffon, fair and delicate skin, cold fluorescent light sheen, digital noise/grain
- **不完美是工具**：slightly tilted, amateur smartphone photography, flat artificial lighting, date stamp——这些「缺陷」是氛围的来源
- **情绪藏在细节里**：subtle faint smile, absentmindedly tracing, nostalgia in the date stamp——不直接说情绪，情绪自己渗透出来

---

## 视觉策略速查

根据你要表达的东西，选最合适的策略：

| 你要表达的感觉 | 推荐策略 | 关键手法 |
|-------------|---------|---------|
| 情绪 / 氛围 | 时空切片 | 构建一个特定时刻，用光线、材质、环境层次来传递情绪 |
| 概念 / 思想 | 隐喻转译 | 找到承载概念的具象载体，通过关系/尺度/缺席来表达 |
| 故事 / 叙事 | 动作瞬间 | 捕捉一个「正在发生」的片刻，让观众脑补前后 |
| 美感 / 设计 | 构图主导 | 留白、尺度对比、色彩克制、几何秩序 |
| 真实 / 生活 | 不完美注入 | 噪点、倾斜、平淡光线、偶然性元素 |
| 力量 / 冲击 | 极端尺度差 | 大小之间 10 倍以上的对比，或数量上的压倒性比例 |
| 诗意 / 文学性 | 文字共生 | 文字和图像相互渗透溶解，一个从另一个里生长 |
| 哲思 / 深度 | 缺席暗示 | 不画发生了什么，画发生之后留下的——让观众自己推导 |

---

## 策略组合指南

最好的 Prompt 往往不是只用一种策略，而是以一个策略为主轴、另一个为辅助。

| 主轴 | 辅助策略 | 适合场景 |
|------|---------|---------|
| 隐喻转译 | 动作瞬间 | 把抽象概念放进一个具体的叙事瞬间中。例：「勇气」→ 一个人的指尖刚刚松开悬崖边缘准备跃向对面 |
| 时空切片 | 不完美注入 | 在精心构建的场景中故意留下真实感的痕迹。例：精心描述的光线 + 「slightly out of focus on the edges」 |
| 构图主导 | 文字共生 | 先设计几何骨架，再把文字嵌进去。例：网格构图 + 文字在网格单元间流动 |
| 极端尺度差 | 缺席暗示 | 用巨物暗示某个不存在的东西的压迫感。例：一个巨大空荡荡的王座占据了整个画面——缺席的王者比在场的更有力量 |
| 文字共生 | 隐喻转译 | 文字的字形本身成为隐喻的载体。例：「FREEDOM」的字母 "M" 被一根铁链锁住脚——字形即牢笼 |

---

## Prompt 质量自检清单

写完任何 Prompt 之后，问自己这 8 个问题：

1. **每一个词我能在脑子里「看到」它对应的画面吗？** — 如果不能，这个词无效
2. **如果把这句话翻译成中文，信息还在吗？** — 翻译后信息减少说明依赖的是语言技巧而非视觉内容
3. **读者读完后脑子里是一个画面还是几个画面打架？** — 几个画面打架=方向不明确
4. **最核心的元素在 prompt 前 10 个词里出现了吗？** — 没出现=权重不够
5. **有没有可以删掉但对画面无影响的词？** — 有就删掉
6. **情绪是「说出来的」还是「渗透出来的」？** — 应该渗透，不该说
7. **这个画面有其他的解法吗？如果有，我的解法是其中最特别的那个吗？** — 不是就再想
8. **用户看到这个 prompt 的第一反应会是「卧槽」还是「哦，还行」？** — 追求前者

---

## 不要做的事

| 反模式 | 为什么不好 | 怎么做 |
|--------|-----------|--------|
| 罗列关键词而非构建画面 | 读起来像清单，不像描述 | 写成完整的视觉句子 |
| 所有 Prompt 一视同仁地加 8K masterpiece | 无信息量，浪费 token | 用具体的纹理/材质词替代 |
| 抽象概念直接画抽象图案 | 空洞，无法产生共鸣 | 找到具象的视觉载体来承载概念 |
| 极简需求给长 Prompt | 过度表达反而稀释 | 精准 > 全面 |
| 精细需求给短 Prompt | 细节不够模型只能乱猜 | 给定够具体的视觉锚点 |
| 多个风格方向混杂 | 互相抵消变成四不像 | 一个强方向 > 三个弱方向 |
| 把情绪词当画面描述 | 「sad atmosphere」没有 visual anchor | 用光线、色彩、空间关系来制造 sad 的感觉 |
| 把空间关系交给运气 | 「in the background」和「in front of」是弱信号 | 用「occupying the left third of the frame」代替 |
| 把中文逻辑直译成英文 | 中文的「高级感」没有英文等价 visual anchor | 找到英文中能触发同样视觉效果的描述方式 |
| 忘记给用户「为什么这样写」的解释 | 用户学会了复制，没学会创作 | 每个输出都附核心隐喻/策略的一句话说明 |
