---
name: gemini-music-prompt
description: 当用户需要为游戏场景或影视场景生成 Gemini 音乐创作提示词时使用。当用户提到"生成音乐prompt"、"Gemini音乐"、"场景配乐"、"音乐提示词"、"相符"、"割裂"、"game audio prompt"、"music prompt for scene"、"bgm prompt"等关键词时激活。Use when the user wants to generate a Gemini Music creation prompt based on a scene description and a match/contrast style mode.
version: 2.0.0
---

# Gemini Music Prompt Generator（场景配乐提示词生成器）

根据**场景描述**，先发现真实影视/游戏参考案例，再由用户选择案例和模式，生成可直接粘贴进 Gemini Music 的英文提示词。

---

## 工作流概览

```
[用户输入场景描述]
        ↓
  阶段一：案例发现
  相符参考 M1–M7 + 割裂参考 C1–C7
  （含 YouTube/Spotify 链接）
        ↓
[用户选择模式 + 案例编号]
  如：相符，M1 M3 M5
        ↓
  阶段二：提炼 + 生成
  音乐共性提炼 → Gemini Music Prompt
```

---

## 阶段一：参考案例发现

**触发条件**：用户提供场景描述（无需指定模式，自动进入阶段一）

### Step 1 — 分析场景四维度

从场景中提取：
- **情绪**：tense / fearful / joyful / melancholic / peaceful / mysterious / romantic / desperate / triumphant
- **节奏**：fast-urgent / slow-steady / erratic-chaotic / flowing-gentle / driving
- **张力**：high（激烈/高潮）/ medium（蓄力）/ low（平缓/消解）
- **主题**：chase-action / horror-dread / romance / exploration / combat / mystery / tragedy / triumph

### Step 2 — 召回参考案例

为**相符**和**割裂**各找 7 个案例，分两层：

**案例分层规则**：
- **编号 1–4（实例型）**：真实存在、有记录地被用于类似场景的著名配乐
  - 例：《敦刻尔克》"The Mole" — 已知用于战场窒息场景（相符）
  - 例：《银河护卫队》"Come and Get Your Love" — 已知用于反差喜剧动作开场（割裂）
- **编号 5–7（推荐型）**：未必用于类似场景，但音乐特质极为契合、值得聆听参考的优秀作品

**编号规则**：
- 相符：M1, M2, M3, M4（实例型）→ M5, M6, M7（推荐型）
- 割裂：C1, C2, C3, C4（实例型）→ C5, C6, C7（推荐型）

### Step 3 — 案例展示格式

每个案例按以下格式输出：

```
**[编号]** 《作品名》— 曲目名
🎬/🎮 类型（电影/游戏/剧集）| 作曲：XXX（年份）
🎯 理由：[1句话说明为何相符/割裂]
🎵 音乐特征：[BPM / 调式 / 核心乐器 / 情绪词]
▶️ YouTube: [链接]
🎵 Spotify: [链接]
```

**链接策略**：
- YouTube 统一使用搜索链接：`https://www.youtube.com/results?search_query=曲目名+作品名`
- Spotify 统一使用搜索链接：`https://open.spotify.com/search/曲目名`
- 搜索关键词尽量精准（含曲目名 + 作品名 + 作曲家），方便用户第一条结果即可命中

### Step 4 — 阶段一完整输出结构

```
## 场景分析
情绪：[值]
节奏：[值]
张力：[值]
主题：[值]

---

## 相符风格参考（M1–M7）

**M1** 《作品名》— 曲目名
🎬 电影 | 作曲：XXX（年份）
🎯 理由：...
🎵 音乐特征：...
▶️ YouTube: https://...
🎵 Spotify: https://...

[M2–M7 同格式]

---

## 割裂风格参考（C1–C7）

[C1–C7 同格式]

---
💡 请告诉我：你想要【相符】还是【割裂】风格，以及想基于哪几个案例来生成 prompt
（例如：相符，M1 M3 M5）
```

---

## 阶段二：Prompt 生成

**触发条件**：用户回复了模式 + 案例编号（如"相符，M1 M2 M5"或"割裂，C3 C5 C7"）

### Step 1 — 提炼所选案例的音乐参数

对每个选中案例分析以下维度：

| 维度 | 分析内容 |
|------|---------|
| 调式/和声 | 大调/小调/无调性/特定音阶 |
| 速度 | BPM 范围 |
| 核心配器 | 主要乐器组 |
| 节奏质感 | 断奏/连奏/驱动性/漂浮感 |
| 动态范围 | 层次厚薄/渐强渐弱模式 |
| 音色质感 | 干/湿、温暖/冷峻、有机/合成 |

### Step 2 — 提炼跨案例共性

找出 2–3 个所有选中案例共享的音乐特征，作为 prompt 的核心骨架。

### Step 3 — 生成 Gemini Music Prompt

**⚠️ 核心原则：Prompt 里只能有音乐，不能有故事**

Gemini Music 只理解音乐参数，不理解叙事情节：

| ❌ 禁止写法 | ✅ 正确写法 |
|-----------|-----------|
| `suitable for a chase scene` | `fast-paced driving strings at 140 BPM` |
| `music for a battle` | `brass stabs with dissonant cluster chords` |
| `the warmth amplifies horror` | `major key music box, 3/4 waltz, 65 BPM` |
| `perfect for psychological thriller` | `irregular silences and sharp percussive hits` |

场景情节、人物、故事背景 → 全部留在"设计说明"里，**一个字都不进入 prompt**。

**🔇 无人声强制规则**：每个 prompt 末尾必须加（无例外）：
```
No vocals, no singing, no choir, purely instrumental.
```
即使风格标签含 `lullaby`、`choral`、`folk` 等通常带人声的词，此声明仍必须加入。

**Prompt 结构模板**：
```
[Genre/style 2-3 words], [mood adjectives 2-3].
[Specific instrumentation] playing [melodic/harmonic character].
[Exact tempo: X BPM] with [rhythmic feel].
[Dynamics and texture details].
Seamless loop with natural phrase endings.
No vocals, no singing, no choir, purely instrumental.
```

**要素清单**：
1. **Genre/style**：风格标签（2–3词）— `dark orchestral`, `ethereal ambient`, `playful music box`, `industrial electronic`
2. **Mood adjectives**：描述音乐本身的情绪（2–3个）— `tense, unsettling, relentless`
3. **Instrumentation**：具体乐器 — `sparse strings and low brass drones`, `gentle piano with pizzicato violins`
4. **Melodic/harmonic character**：旋律和声特征 — `dissonant cluster chords`, `repetitive major key melody`
5. **Tempo**：精确速度 — `slow and irregular (50–60 BPM)`, `driving (140 BPM)`, `slow waltz (65 BPM, 3/4)`
6. **Rhythmic feel**：节奏质感 — `staccato bursts`, `sustained legato phrases`, `sudden dynamic swells`
7. **Dynamics/texture**：动态与音色 — `cold reverberant space`, `dense layered textures`
8. **Loop**（游戏用途必加）— `Seamless loop with natural phrase endings.`
9. **无人声声明**（每次必加）— `No vocals, no singing, no choir, purely instrumental.`

### Step 4 — 阶段二完整输出结构

```
## 参考提炼

| 案例 | 调式 | BPM | 核心配器 | 节奏质感 | 共性 |
|------|------|-----|---------|---------|------|
| M1   | ...  | ... | ...     | ...     | ✓   |
| M3   | ...  | ... | ...     | ...     | ✓   |
| M5   | ...  | ... | ...     | ...     | ✓   |

**共性提炼**：[2–3句话描述跨案例共同的音乐特征]

---

## Gemini Music Prompt

> [完整英文 prompt，引用块，可直接复制粘贴]

---

## 设计说明

[为什么这些案例导向此方向；相符/割裂逻辑如何在 prompt 参数中体现；注意 prompt 里没有出现任何情节描述]

---

## 使用提示

复制上方引用块中的 prompt，前往 [gemini.google.com](https://gemini.google.com) 的音乐创作功能粘贴使用。
```

---

## 示例

### 阶段一示例

**用户输入**："废弃医院走廊，主角听到远处传来孩子的笑声，脚步声越来越近。"

**输出**（节选）：

```
## 场景分析
情绪：恐惧、不安、压抑
节奏：缓慢、不规则
张力：高（持续积累）
主题：恐怖/心理惊悚

## 相符风格参考（M1–M7）

**M1** 《寂静岭2》— "Promise (Reprise)"
🎮 游戏 | 作曲：山冈晃（2001）
🎯 理由：被用于心理恐怖场景，极简钢琴旋律在寂静中制造窒息感
🎵 音乐特征：40–50 BPM / 小调 / 单钢琴+环境音 / 孤独、压抑
▶️ YouTube: https://www.youtube.com/results?search_query=Silent+Hill+2+Promise+Reprise
🎵 Spotify: https://open.spotify.com/search/Promise%20Reprise%20Silent%20Hill%202

...（M2–M7 同格式）

## 割裂风格参考（C1–C7）

**C1** 《闪灵》— "Midnight, The Stars And You"
🎬 电影 | 斯坦利·库布里克选用（1980）
🎯 理由：在恐惧高潮使用欢快的30年代舞曲，制造强烈心理割裂感，已成割裂配乐经典案例
🎵 音乐特征：120 BPM / 大调 / 大乐队爵士 / 明亮、温柔、甜美
▶️ YouTube: https://www.youtube.com/results?search_query=Midnight+The+Stars+And+You+The+Shining
🎵 Spotify: https://open.spotify.com/search/Midnight%20The%20Stars%20And%20You

...（C2–C7 同格式）

💡 请告诉我：你想要【相符】还是【割裂】风格，以及想基于哪几个案例来生成 prompt
（例如：相符，M1 M3 M5）
```

---

### 阶段二示例

**用户输入**："割裂，C1 C3 C5"

**输出**：

```
## 参考提炼

| 案例 | 调式 | BPM | 核心配器 | 节奏质感 |
|------|------|-----|---------|---------|
| C1（《闪灵》大乐队爵士）| 大调 | 120 | 铜管+钢琴 | 稳定摇摆 |
| C3（...）| 大调 | 70 | 音乐盒+弦乐拨奏 | 轻柔稳定 |
| C5（...）| 大调 | 65 | 木管+轻钢琴 | 3/4 华尔兹 |

**共性提炼**：三个案例均为大调、稳定节拍、明亮音色，以"过度正常"的轻盈感对抗恐惧场景。

---

## Gemini Music Prompt

> Warm innocent children's lullaby, bright and gently cheerful. Simple repetitive music box melody in C major, accompanied by soft pizzicato strings and quiet tinkling glockenspiel. Slow steady 3/4 waltz at 65 BPM with consistent gentle pulse. Intimate dry recording, minimal reverb, low even dynamics throughout with no dramatic swells or tension. Seamless loop with clean phrase endings. No vocals, no singing, no choir, purely instrumental.

---

## 设计说明

三个参考案例的共性是：大调、稳定节拍、轻盈音色——用音乐上的"太正常"来对抗场景的恐惧张力。Prompt 中全部使用音乐参数（调式、BPM、乐器、动态），没有出现任何"医院"或"孩子"等情节词汇。"No dramatic swells" 确保音乐保持平静，不顺应场景张力，维持割裂效果。

---

## 使用提示

复制上方引用块中的 prompt，前往 gemini.google.com 的音乐创作功能粘贴使用。
```

---

## 进阶技巧

- **混合维度**：可在阶段二指定"情绪割裂但节奏相符"，Claude 会分开处理
- **指定时长**：可加"大约2分钟"，Claude 在 prompt 中加入 `approximately 2 minutes`
- **指定乐器限定**：可要求"只用中国传统乐器"或"纯合成器"
- **多版本**：可要求"生成开场版/高潮版/结尾版三个变体 prompt"
- **重新选案例**：阶段一输出后可要求"把 M3 换成其他案例"，Claude 重新召回
