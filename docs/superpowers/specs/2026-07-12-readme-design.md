# 中文 README 设计

## 目标

为 `reviewing-article-quality` SKILL 提供一份位于项目根目录的中文 `README.md`，帮助 Codex 和 Claude Code 用户完成安装，并通过示例理解如何调用及解读输出。

## 目标读者

- 希望在 Codex 中安装个人 SKILL 的用户。
- 希望在 Claude Code 中安装同一套文章审查指令的用户。
- 不要求读者预先了解本仓库的目录结构。

## 文档结构

1. 项目简介：概括 SKILL 的用途、适用文本和核心审查原则。
2. 能力与输出：说明审查维度以及固定输出结构。
3. 安装：分别给出 Codex 和 Claude Code 的复制安装、Git 克隆安装方式，并解释安装目标目录。
4. 使用：提供显式调用、自然语言调用和带审查目标调用的示例。
5. 项目结构：解释 `SKILL.md`、`references/rubric.md` 和 `agents/openai.yaml` 的职责。
6. 更新与卸载：给出安全、明确的命令。

## 平台兼容策略

仓库保留现有 SKILL 结构，不复制或改写核心规则。Codex 安装到 `${CODEX_HOME:-$HOME/.codex}/skills/reviewing-article-quality`。Claude Code 安装到用户级 `$HOME/.claude/skills/reviewing-article-quality`；同时说明也可放到项目级 `.claude/skills/`，便于团队随仓库共享。

示例优先使用显式名称 `reviewing-article-quality`，同时说明自然语言请求也可以触发该能力。Claude Code 部分不依赖 `agents/openai.yaml`，因为核心工作流与评分规则都位于 `SKILL.md` 和 `references/` 中。

## 边界

- 只新增 README 和本设计文档，不修改 SKILL 的审查逻辑。
- 不承诺未经本地文件支持的平台特性。
- 安装命令避免覆盖已有同名目录；覆盖或升级操作单独说明。

## 验证

- 核对 README 中出现的源文件和目录均真实存在。
- 检查 shell 命令语法，并在临时目录模拟复制安装。
- 检查 Markdown 标题、代码块和相对链接。
- 对照 `SKILL.md` 与评分规则，确认功能描述没有超出实际能力。
