# 文章发布前检查 Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建并验证一个只修正明确文字错误、同时生成公众号候选标题和无文字封面提示词的 `checking-before-publishing` Skill。

**Architecture:** 使用单一 `SKILL.md` 定义严格校对边界、判断流程和固定输出契约，使用 `agents/openai.yaml` 提供界面元数据。无需脚本或额外参考资料；通过结构校验、关键词断言和代表性文章人工行为演练验收。

**Tech Stack:** Markdown、YAML、Skill Creator 自带的 `init_skill.py` 与 `quick_validate.py`、Shell 检查命令。

## Global Constraints

- 只修改明确的错别字、标点及原意清晰的明显漏字或赘字。
- 不润色，不调整句式、段落、语序、语气、观点、事实、数据、引文或专有名词。
- 输出校对后正文、修改清单、待作者确认、10 个候选标题和 3 组封面提示词。
- 封面不含标题及任何文字、字母、数字、Logo 或水印，比例固定为 `2.35:1`。

---

### Task 1: 创建 Skill 骨架与失败检查

**Files:**
- Create: `checking-before-publishing/SKILL.md`
- Create: `checking-before-publishing/agents/openai.yaml`

**Interfaces:**
- Consumes: `docs/superpowers/specs/2026-07-12-checking-before-publishing-design.md`
- Produces: 可被 Codex 发现的 `checking-before-publishing` Skill 目录。

- [ ] **Step 1: 在 Skill 尚不存在时运行失败检查**

```bash
test -f checking-before-publishing/SKILL.md
```

Expected: FAIL，退出码为 `1`，证明检查能识别缺失的 Skill。

- [ ] **Step 2: 使用官方初始化脚本创建骨架**

```bash
python /Users/chenhao/.codex/skills/.system/skill-creator/scripts/init_skill.py checking-before-publishing \
  --path /Users/chenhao/Downloads/easy_writing \
  --interface 'display_name=文章发布前检查' \
  --interface 'short_description=严格校对错别字与标点，并生成标题和封面提示词' \
  --interface 'default_prompt=使用 $checking-before-publishing 检查这篇公众号文章的发布终稿。'
```

Expected: 创建 `SKILL.md` 与 `agents/openai.yaml`。

- [ ] **Step 3: 检查骨架存在**

```bash
test -f checking-before-publishing/SKILL.md && test -f checking-before-publishing/agents/openai.yaml
```

Expected: PASS，退出码为 `0`。

### Task 2: 写入最小行为契约

**Files:**
- Modify: `checking-before-publishing/SKILL.md`
- Modify: `checking-before-publishing/agents/openai.yaml`

**Interfaces:**
- Consumes: Task 1 创建的 Skill 骨架。
- Produces: 固定五段输出、严格修改边界及标题和封面要求。

- [ ] **Step 1: 运行内容断言并观察失败**

```bash
rg -q '2\.35:1' checking-before-publishing/SKILL.md && \
rg -q '10 个' checking-before-publishing/SKILL.md && \
rg -q '3 组' checking-before-publishing/SKILL.md
```

Expected: FAIL，因为初始化模板尚未包含行为契约。

- [ ] **Step 2: 将设计规格写入 `SKILL.md`**

写入仅含 `name`、`description` 的 YAML frontmatter；正文使用命令式语言，依次定义核心原则、允许修改、禁止修改、执行流程、固定输出格式、标题约束、封面提示词约束及常见错误。

- [ ] **Step 3: 校验界面元数据**

```bash
sed -n '1,80p' checking-before-publishing/agents/openai.yaml
```

Expected: 字符串全部加引号，`default_prompt` 明确包含 `$checking-before-publishing`。

- [ ] **Step 4: 重跑内容断言**

```bash
rg -q '2\.35:1' checking-before-publishing/SKILL.md && \
rg -q '10 个' checking-before-publishing/SKILL.md && \
rg -q '3 组' checking-before-publishing/SKILL.md
```

Expected: PASS，退出码为 `0`。

### Task 3: 验证与提交

**Files:**
- Test: `checking-before-publishing/SKILL.md`
- Test: `checking-before-publishing/agents/openai.yaml`

**Interfaces:**
- Consumes: Task 2 完成的 Skill。
- Produces: 结构有效且经代表性场景核对的可交付 Skill。

- [ ] **Step 1: 运行官方结构校验**

```bash
python /Users/chenhao/.codex/skills/.system/skill-creator/scripts/quick_validate.py checking-before-publishing
```

Expected: `Skill is valid!`

- [ ] **Step 2: 运行完整约束检查**

```bash
rg -n '错别字|标点|待作者确认|10 个|3 组|2\.35:1|无文字|水印' checking-before-publishing/SKILL.md
```

Expected: 每项关键约束均至少命中一次。

- [ ] **Step 3: 用代表性文章演练行为**

输入含错别字、重复标点、专有名词、事实数据和风格化表达的短文，核对：只改明确错误；不改事实与文风；不确定项进入“待作者确认”；输出 10 个有正文依据的标题；输出 3 组无文字、`2.35:1` 的封面提示词。

- [ ] **Step 4: 检查工作树并提交**

```bash
git diff --check
git status --short
git add checking-before-publishing docs/superpowers/plans/2026-07-12-checking-before-publishing.md
git commit -m "feat: 添加文章发布前检查 skill"
```

Expected: 只提交本计划与新 Skill，不提交已有 `.DS_Store` 修改。
