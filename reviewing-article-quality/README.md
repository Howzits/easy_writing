# reviewing-article-quality

用于中文长文修改阶段的深度质量审查 Skill。它按照“先思考、再结构、后表达”的顺序诊断文章，并给出有证据、有优先级的修改建议。

## 适用内容

- 文章、随笔和 Newsletter；
- 微信公众号文章；
- 商业分析、观点稿和内部长文；
- 需要检查论证、结构、证据或读者理解成本的完整初稿。

## 主要能力

- 重建文章的核心问题、结论和证据路径。
- 区分事实、因果、结构、读者理解、证据、简洁度和风格问题。
- 使用 100 分量表评估思考质量、读者桥梁和文字表达。
- 将问题标记为 `Critical`、`Major`、`Minor` 或 `Optional`。
- 为高影响问题提供直接替换示例。
- 按影响程度给出修改顺序。

## 能力边界

- 不自动证明文章中的外部事实为真；需要事实核验时应另行查询可靠来源。
- 输入不完整时，评分只作为暂定参考。
- 默认保留作者的核心立场、声音、事实含义和目标读者。
- 不为了填满评分维度而虚构问题，也不把复杂、冗长或充满术语当作高质量。

## 输出内容

Skill 按以下顺序输出：

1. 一句话总评；
2. 评分卡：总分、思考质量、读者桥梁和文字表达；
3. 重建后的核心命题；
4. 按 `Critical`、`Major`、`Minor`、`Optional` 排列的问题；
5. 最高影响问题的直接替换示例；
6. 按顺序执行的修改清单。

每项重要批评都应指出原文证据或明确缺失，并解释问题、影响和修改动作。

## 目录结构

```text
reviewing-article-quality/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── rubric.md
```

安装时必须复制完整目录；`references/rubric.md` 包含详细评分标准。

## 安装到 Codex

个人级安装：

```bash
SOURCE="$PWD/reviewing-article-quality"
SKILLS_HOME="${CODEX_HOME:-$HOME/.codex}/skills"
TARGET="$SKILLS_HOME/reviewing-article-quality"

test ! -e "$TARGET" || { echo "安装目标已存在：$TARGET"; exit 1; }
mkdir -p "$SKILLS_HOME"
cp -R "$SOURCE" "$TARGET"
```

显式调用：

```text
使用 $reviewing-article-quality 审查 article.md，找出最影响文章质量的问题，并给出修改顺序。
```

也可以自然语言调用：

```text
请审查这篇公众号文章的核心命题、因果推理、逻辑结构和证据质量。
```

## 安装到 Claude Code

个人级安装：

```bash
SOURCE="$PWD/reviewing-article-quality"
TARGET="$HOME/.claude/skills/reviewing-article-quality"

test ! -e "$TARGET" || { echo "安装目标已存在：$TARGET"; exit 1; }
mkdir -p "$HOME/.claude/skills"
cp -R "$SOURCE" "$TARGET"
```

项目级安装：

```bash
SOURCE="/path/to/easy_writing/reviewing-article-quality"
TARGET="$PWD/.claude/skills/reviewing-article-quality"

test ! -e "$TARGET" || { echo "安装目标已存在：$TARGET"; exit 1; }
mkdir -p "$PWD/.claude/skills"
cp -R "$SOURCE" "$TARGET"
```

显式调用：

```text
/reviewing-article-quality 审查 article.md
```

## 直接粘贴文章

```text
请使用 reviewing-article-quality 审查下面的文章。

目标读者：对 AI 产品感兴趣的非技术管理者
发布渠道：微信公众号
重点检查：论点、因果关系和结构

文章正文：
……
```

提供目标读者、发布渠道、文章目标和希望重点检查的维度，可以让结果更贴近实际写作任务。

## 更新

```bash
SOURCE="$PWD/reviewing-article-quality"
TARGET="${CODEX_HOME:-$HOME/.codex}/skills/reviewing-article-quality"
BACKUP="${TARGET}.backup.$(date +%Y%m%d%H%M%S)"

test -d "$TARGET" || { echo "未找到已安装的 Skill：$TARGET"; exit 1; }
mv "$TARGET" "$BACKUP"
cp -R "$SOURCE" "$TARGET"
echo "旧版本已备份到：$BACKUP"
```

Claude Code 用户将 `TARGET` 改为 `$HOME/.claude/skills/reviewing-article-quality`。

## 卸载

Codex：

```bash
rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/reviewing-article-quality"
```

Claude Code：

```bash
rm -rf "$HOME/.claude/skills/reviewing-article-quality"
```

执行删除命令前请再次确认目标路径。

