## Game Audio prompt Claude Skills


### AI-music-prompt
根据游戏/影视场景描述生成AI音乐创作提示词。

Generate AI music creation prompts based on the description of game/film scenes.

**特点**：
- 两阶段工作流（先找参考案例 → 再生成 prompt）
- 相符/割裂各 7 个参考案例（含 YouTube/Spotify 链接）
- 严禁人声、只允许音乐语言
- 基于真实作品的音乐参数提炼

**Features**:
- Two-stage workflow (first find reference cases → then generate prompt)
- 7 reference cases each (including YouTube/Spotify links)
- Prohibit human voices, only allow musical language
- Extract music parameters based on real works

**使用方式**：
在 Claude 会话中输入场景描述，Skill 自动激活，无需指定模式。

**Usage Method**:
Enter the scene description in the Claude conversation, and the Skill will automatically activate without the need to specify the mode.

---

## 安装方法

## Installation Method

```bash
mkdir -p ~/.claude/skills
cp -r game-audio-music-prompt ~/.claude/skills/
```

然后在任何 Claude 会话中提供场景描述，自动使用。
Then, in any Claude conversation, provide the scene description and let it be automatically used.
