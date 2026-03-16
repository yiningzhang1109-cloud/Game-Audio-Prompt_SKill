## Game Audio prompt Claude Skills


### AI-music-prompt
根据游戏/影视场景描述生成AI音乐创作提示词。

**特点**：
- 两阶段工作流（先找参考案例 → 再生成 prompt）
- 相符/割裂各 7 个参考案例（含 YouTube/Spotify 链接）
- 严禁人声、只允许音乐语言
- 基于真实作品的音乐参数提炼

**使用方式**：
在 Claude 会话中输入场景描述，Skill 自动激活，无需指定模式。

---

## 安装方法

```bash
mkdir -p ~/.claude/skills
cp -r game-audio-music-prompt ~/.claude/skills/
```

然后在任何 Claude 会话中提供场景描述，自动使用。
