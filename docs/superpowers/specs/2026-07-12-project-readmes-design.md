# 项目 README 体系设计

## 目标

为项目建立三级自包含文档：根 README 解释整个项目，两个 Skill 各自拥有独立 README。文档同时覆盖 Codex 与 Claude Code，使整个项目和单独复制的 Skill 都能被正确理解、安装和使用。

## 信息架构

### 根目录 README

根 `README.md` 负责项目级信息：

- 说明 `easy_writing` 是一组中文写作 Agent Skills。
- 对比 `reviewing-article-quality` 与 `checking-before-publishing` 的用途、阶段、修改深度和输出。
- 给出推荐工作流：质量审查 → 作者修改 → 发布前检查。
- 展示完整项目结构。
- 提供 Codex 和 Claude Code 的统一安装方式，可选择安装一个或全部 Skill。
- 提供两个 Skill 的最短调用示例。
- 说明更新、卸载、事实核验边界和兼容性注意事项。

根 README 不重复每个 Skill 的全部细节，而是链接到各自 README。

### reviewing-article-quality README

`reviewing-article-quality/README.md` 独立说明：

- 适用于修改阶段的深度文章质量审查。
- 检查核心命题、事实与因果、逻辑结构、读者理解成本、证据、简洁度和文风。
- 100 分评分、问题严重度与固定输出结构。
- 能力边界：不自动证明外部事实，不在输入不完整时给出确定评分。
- Skill 文件结构，以及安装时必须复制完整目录的原因。
- Codex 与 Claude Code 的个人级和项目级安装方式。
- 显式调用、自然语言调用和直接粘贴文章的示例。
- 更新、卸载和常见注意事项。

### checking-before-publishing README

`checking-before-publishing/README.md` 独立说明：

- 适用于文章发布前最后检查。
- 只修改明确的错别字、原意清晰的漏字或赘字和标点问题。
- 不润色、不调整句式或段落、不改变观点、事实、数据、引文、专有名词和语气。
- 固定输出完整正文、修改清单、待作者确认、10 个标题和 3 组封面提示词。
- 封面提示词不含标题或任何文字，固定横版比例 `2.35:1`。
- Codex 与 Claude Code 的个人级和项目级安装方式。
- 文件输入、直接粘贴文章、显式调用和自然语言调用示例。
- 更新、卸载和常见注意事项。

## 文档一致性规则

- 所有路径、目录名和调用名与实际 Skill 文件一致。
- Codex 显式调用使用 `$skill-name`；Claude Code 显式调用使用 `/skill-name`。
- 安装命令复制完整 Skill 目录，不只复制 `SKILL.md`。
- 默认 Codex 安装目录使用 `${CODEX_HOME:-$HOME/.codex}/skills`。
- 默认 Claude Code 安装目录使用 `$HOME/.claude/skills`；项目级目录使用 `.claude/skills`。
- 已存在的安装目标不静默覆盖；更新流程先创建时间戳备份。
- 文档以中文为主，命令、路径和产品名保留英文。

## 非目标

- 不修改任何 Skill 的行为规则。
- 不新增安装脚本、自动化发布或外部依赖。
- 不修改或恢复当前工作树中与本任务无关的文件删除。

## 验收标准

- 根目录和每个 Skill 均存在 README。
- 根 README 能清晰解释项目、两个 Skill 的区别和推荐使用顺序。
- 每个 Skill README 离开根目录后仍能独立指导安装和使用。
- Codex 与 Claude Code 的安装和调用示例路径准确。
- `checking-before-publishing` 的严格修改边界、10 个标题、3 组无文字封面提示词和 `2.35:1` 均有明确说明。
- `reviewing-article-quality` 的评分、严重度、输出和事实核验边界均有明确说明。
- README 中不存在失效的项目内链接、占位符或与 Skill 文件冲突的描述。
