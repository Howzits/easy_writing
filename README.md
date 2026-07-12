# reviewing-article-quality

一个用于审查中文长文质量的 Agent Skill，适用于文章、随笔、Newsletter、微信公众号文章、商业分析等内容。

它不会只做措辞润色，而是按照“先思考、再结构、后表达”的顺序，检查文章的核心命题、事实与因果、逻辑结构、读者理解成本、证据、简洁度和文风，并给出有优先级的修改建议。

本 SKILL 可用于 **Codex** 和 **Claude Code**。

## 主要能力

- 重建文章的核心问题、结论和证据路径。
- 区分事实、因果、结构、读者理解、证据、简洁度和风格问题。
- 使用 100 分量表评估思考质量、读者桥梁和文字表达。
- 将问题标记为 `Critical`、`Major`、`Minor` 或 `Optional`。
- 为高影响问题提供可直接参考的替换示例。
- 输出按优先级排列的修改清单。
- 在原文不完整时明确说明评分仅供参考，避免凭空补充事实。

## 项目结构

```text
reviewing-article-quality/
├── SKILL.md                 # 入口文件：触发条件、审查流程和输出要求
├── references/
│   └── rubric.md            # 详细评分标准
└── agents/
    └── openai.yaml          # Codex 中的展示名称和默认提示词
```

安装时应复制整个 `reviewing-article-quality` 目录，不能只复制 `SKILL.md`，否则评分规则不会随 SKILL 一同安装。

## 安装

### 1. 获取项目

```bash
git clone https://github.com/Howzits/easy_writing.git
cd easy_writing
```

如果你已经下载或克隆了本项目，直接进入项目根目录即可。

### 2. 安装到 Codex

个人级安装会让该 SKILL 在所有项目中可用：

```bash
SOURCE="$PWD/reviewing-article-quality"
SKILLS_HOME="$HOME/.agents/skills"
TARGET="$SKILLS_HOME/reviewing-article-quality"

if [ -e "$TARGET" ]; then
  echo "安装目标已存在：$TARGET"
  echo "如需升级，请参阅下方的“更新”章节。"
  exit 1
fi

mkdir -p "$SKILLS_HOME"
cp -R "$SOURCE" "$TARGET"
```

Codex 会自动检测 SKILL 变更；如果没有出现在技能列表中，请重启 Codex。

默认目标目录为：

```text
~/.agents/skills/reviewing-article-quality/
```

### 3. 安装到 Claude Code

#### 个人级安装

安装后可在所有项目中使用：

```bash
SOURCE="$PWD/reviewing-article-quality"
SKILLS_HOME="$HOME/.claude/skills"
TARGET="$SKILLS_HOME/reviewing-article-quality"

if [ -e "$TARGET" ]; then
  echo "安装目标已存在：$TARGET"
  echo "如需升级，请参阅下方的“更新”章节。"
  exit 1
fi

mkdir -p "$SKILLS_HOME"
cp -R "$SOURCE" "$TARGET"
```

默认目标目录为：

```text
~/.claude/skills/reviewing-article-quality/
```

#### 项目级安装

如果只希望某个项目使用，或者希望团队通过 Git 共享该 SKILL，请在目标项目根目录执行：

```bash
SOURCE="/path/to/easy_writing/reviewing-article-quality"
TARGET="$PWD/.claude/skills/reviewing-article-quality"

if [ -e "$TARGET" ]; then
  echo "安装目标已存在：$TARGET"
  exit 1
fi

mkdir -p "$PWD/.claude/skills"
cp -R "$SOURCE" "$TARGET"
```

请将 `/path/to/easy_writing` 替换为本项目的实际绝对路径。

Claude Code 通常能够实时发现 `~/.claude/skills/` 或 `.claude/skills/` 中的变更；如果 SKILL 顶层目录是在会话启动后首次创建的，请重新启动 Claude Code。

## 使用

### Codex

在提示词中显式指定 SKILL：

```text
使用 $reviewing-article-quality 审查 article.md，找出最影响文章质量的问题，并给出修改顺序。
```

也可以直接提出与描述匹配的自然语言请求：

```text
请审查这篇公众号文章的思考质量、逻辑结构和文字表达，给出有优先级的修改建议。
```

### Claude Code

在 Claude Code 中可以用目录名直接调用：

```text
/reviewing-article-quality 请审查 article.md
```

也可以让 Claude Code 根据 SKILL 的描述自动选择：

```text
请审查 article.md，重点检查核心命题、因果推理、读者理解成本和证据质量。
```

### 直接粘贴文章

如果文章不在本地文件中，可以把正文粘贴到提示词后：

```text
请使用 reviewing-article-quality 审查下面这篇文章。

目标读者：对 AI 产品感兴趣的非技术管理者
发布渠道：微信公众号
希望重点检查：论点是否成立、结构是否清楚

文章正文：
……
```

### 指定审查重点

提供以下信息可以让建议更贴近实际写作目标：

- 目标读者和发布渠道；
- 文章希望回答的问题；
- 希望读者读完后理解或采取的行动；
- 需要重点检查的维度；
- 是否允许调整文章立场、结构或语气。

例如：

```text
/reviewing-article-quality 审查 report.md。

这是给公司管理层看的商业分析。请保留原有结论和正式语气，重点检查因果关系、证据强度和段落顺序。最后给出前三项最值得优先修改的内容。
```

## 输出内容

SKILL 会按以下顺序返回结果：

1. 一句话总评；
2. 评分卡：总分、思考质量、读者桥梁、文字表达；
3. 重建后的核心命题；
4. 按严重程度排列的问题；
5. 最高影响问题的直接替换示例；
6. 按顺序执行的修改清单。

每一项重要批评都应指出对应原文或明确缺失的内容，并解释问题、影响和修改动作。评分用于辅助判断，不代表绝对标准。

## 更新

先在本项目目录获取最新版本：

```bash
git pull --ff-only
```

然后备份旧版本并重新复制。Codex 用户执行：

```bash
SOURCE="$PWD/reviewing-article-quality"
TARGET="$HOME/.agents/skills/reviewing-article-quality"
BACKUP="${TARGET}.backup.$(date +%Y%m%d%H%M%S)"

test -d "$TARGET" || { echo "未找到已安装的 SKILL：$TARGET"; exit 1; }
mv "$TARGET" "$BACKUP"
cp -R "$SOURCE" "$TARGET"
echo "旧版本已备份到：$BACKUP"
```

Claude Code 个人级用户将 `TARGET` 改为：

```bash
TARGET="$HOME/.claude/skills/reviewing-article-quality"
```

确认新版本正常后，可以手动删除带时间戳的备份目录。

## 卸载

卸载前请确认路径无误。

Codex：

```bash
rm -rf "$HOME/.agents/skills/reviewing-article-quality"
```

Claude Code 个人级安装：

```bash
rm -rf "$HOME/.claude/skills/reviewing-article-quality"
```

Claude Code 项目级安装：

```bash
rm -rf "$PWD/.claude/skills/reviewing-article-quality"
```

## 注意事项

- SKILL 主要评估文章的论证和表达质量，不会自动证明文章中的外部事实为真。
- 遇到需要事实核验的主张时，应另行要求 Codex 或 Claude Code 查询可靠来源。
- 输入不是完整文章时，评分应被视为暂定结果。
- `agents/openai.yaml` 用于 Codex 的界面展示；Claude Code 主要读取 `SKILL.md` 及其引用的 `references/rubric.md`。

## 参考文档

- [Codex：Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Claude Code：Extend Claude with skills](https://code.claude.com/docs/en/skills)
