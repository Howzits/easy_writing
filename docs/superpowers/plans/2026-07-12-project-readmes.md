# 项目 README 体系 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 为项目根目录和两个 Skill 创建自包含、相互一致且同时支持 Codex 与 Claude Code 的 README 文档。

**Architecture:** 根 README 作为项目入口，负责项目定位、Skill 对比、推荐流程和统一安装；每个 Skill README 负责自身能力边界、完整安装与调用说明。所有文档引用实际文件和目录，不新增脚本或改变 Skill 行为。

**Tech Stack:** Markdown、Shell、Git。

## Global Constraints

- 中文为主，命令、路径和产品名保留英文。
- Codex 显式调用使用 `$skill-name`，默认个人目录为 `${CODEX_HOME:-$HOME/.codex}/skills`。
- Claude Code 显式调用使用 `/skill-name`，个人目录为 `$HOME/.claude/skills`，项目目录为 `.claude/skills`。
- 不修改两个 `SKILL.md` 的行为规则。
- 不把当前已有的无关删除或 `.DS_Store` 变更混入提交。

---

### Task 1: 创建 Skill 独立 README

**Files:**
- Create: `reviewing-article-quality/README.md`
- Create: `checking-before-publishing/README.md`

**Interfaces:**
- Consumes: 两个目录中的 `SKILL.md`、`agents/openai.yaml` 及现有参考资料。
- Produces: 可随单个 Skill 独立分发的安装与使用文档。

- [ ] **Step 1: 运行 README 缺失检查**

```bash
test -f reviewing-article-quality/README.md && test -f checking-before-publishing/README.md
```

Expected: FAIL，证明两个独立 README 尚不存在。

- [ ] **Step 2: 创建文章质量审查 README**

写入定位、能力、非目标、输出、目录结构、Codex/Claude Code 安装与调用、直接粘贴示例、更新、卸载和注意事项。

- [ ] **Step 3: 创建发布前检查 README**

写入严格校对边界、固定五段输出、10 个标题、3 组无文字 `2.35:1` 封面提示词、目录结构、Codex/Claude Code 安装与调用、更新、卸载和注意事项。

- [ ] **Step 4: 重跑存在性检查**

```bash
test -f reviewing-article-quality/README.md && test -f checking-before-publishing/README.md
```

Expected: PASS。

### Task 2: 重写根 README 为项目总览

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: Task 1 创建的两个 Skill README。
- Produces: 项目级入口、对比、推荐工作流和统一安装说明。

- [ ] **Step 1: 运行项目级内容检查并观察失败**

```bash
rg -q '推荐工作流' README.md && rg -q 'checking-before-publishing/README.md' README.md
```

Expected: FAIL，因为旧 README 主要描述单个 Skill。

- [ ] **Step 2: 重写根 README**

加入项目定位、两个 Skill 对比表、推荐工作流、项目结构、全部或单独安装、快速调用、独立 README 链接、更新、卸载和注意事项。

- [ ] **Step 3: 重跑项目级内容检查**

```bash
rg -q '推荐工作流' README.md && rg -q 'checking-before-publishing/README.md' README.md
```

Expected: PASS。

### Task 3: 验证与提交

**Files:**
- Test: `README.md`
- Test: `reviewing-article-quality/README.md`
- Test: `checking-before-publishing/README.md`

**Interfaces:**
- Consumes: Tasks 1–2 的三个 README。
- Produces: 内容一致、链接有效且提交范围干净的文档变更。

- [ ] **Step 1: 检查关键要求**

```bash
rg -q '100 分' reviewing-article-quality/README.md
rg -q 'Critical.*Major.*Minor.*Optional' reviewing-article-quality/README.md
rg -q '10 个' checking-before-publishing/README.md
rg -q '3 组' checking-before-publishing/README.md
rg -q '2\.35:1' checking-before-publishing/README.md
rg -q '不润色' checking-before-publishing/README.md
```

Expected: 全部 PASS。

- [ ] **Step 2: 检查内部链接和占位符**

```bash
test -f reviewing-article-quality/SKILL.md
test -f reviewing-article-quality/references/rubric.md
test -f checking-before-publishing/SKILL.md
! rg -n 'TBD|TODO|待定' README.md reviewing-article-quality/README.md checking-before-publishing/README.md
```

Expected: 全部 PASS。

- [ ] **Step 3: 检查并提交指定文件**

```bash
git diff --check
git add README.md reviewing-article-quality/README.md checking-before-publishing/README.md docs/superpowers/plans/2026-07-12-project-readmes.md
git commit -m "docs: 添加项目和 Skill 使用说明"
```

Expected: 只提交三个 README 和本计划，不包含其他已有工作树变更。
