# Agent Skills 说明

本项目安装了 [mattpocock/skills](https://github.com/mattpocock/skills) 中的一组 Agent Skills，用于在开发 FitStudio 时按软件工程方法协作，而不是「凭感觉写代码」。

Skills 文件位于 `.agents/skills/`，版本锁定见项目根目录 `skills-lock.json`。

## 技能一览

共 **13 个**，分为三类：**手动调用**、**AI 自动选用**、**依赖项**。

### 手动调用（输入 `/技能名`）

| Skill | 一句话 | 什么时候用 |
|-------|--------|------------|
| `setup-matt-pocock-skills` | 项目一次性初始化 | **最先跑**。配置 Issue 追踪（GitHub / 本地）、Triage 标签、文档目录（`CONTEXT.md`、`docs/adr/`） |
| `grill-with-docs` | 需求拷问 + 边聊边写文档 | 有代码库时，开工前对齐需求；产出 `CONTEXT.md`（领域术语）和 ADR |
| `to-prd` | 把当前对话整理成 PRD | 需求聊清楚后，不追问，直接合成 PRD 并发布到 Issue 追踪 |
| `to-issues` | 把 PRD 拆成可独立开发的 Issue | PRD 完成后，按**垂直切片**拆任务（每条 Issue 端到端可交付） |
| `prototype` | 快速做可抛原型 | 逻辑或 UI 说不清时：终端小应用验证状态机，或一屏多版 UI 对比 |
| `grill-me` | 纯需求拷问，不写文档 | **没有代码库**时用；不落盘，只帮你把方案想清楚 |
| `handoff` | 生成交接文档 | 对话太长、要换会话，或去跑 prototype 时，把上下文打包给下一个 Agent |
| `ask-matt` | 技能路由器 | 不确定用哪个 skill 时问它，会按你的情况推荐流程 |

### AI 自动选用（不用输入，任务匹配时自动用）

| Skill | 一句话 | 什么时候触发 |
|-------|--------|--------------|
| `tdd` | 测试驱动开发（红-绿-重构） | 写功能 / 修 bug 要走测试先行，或提到 TDD、集成测试 |
| `diagnosing-bugs` | 系统化排错 | 说「帮我 debug」「这个挂了」「很慢」等 |
| `codebase-design` | 深度模块设计 | 设计接口、找测试接缝（seam）、让模块「小接口、大行为」 |
| `grilling` | 逐问逐答的需求拷问循环 | 被 `grill-me` / `grill-with-docs` 调用；一次只问一个问题 |
| `domain-modeling` | 维护领域模型文档 | 被 `grill-with-docs` 调用；写 / 更新 `CONTEXT.md` 和 ADR |

### 依赖项为什么保留

`grilling` 和 `domain-modeling` 没有独立斜杠命令，但是：

- `grill-me` / `grill-with-docs` → 依赖 **`grilling`**（拷问循环）
- `grill-with-docs` → 依赖 **`domain-modeling`**（写 `CONTEXT.md` 和 ADR）

删掉它们，前两个 skill 无法正常工作。

## 推荐工作流

`ask-matt` 定义的主线流程：

```
/setup-matt-pocock-skills     ← 先配置项目
        ↓
/grill-with-docs              ← 对齐领域模型与需求
        ↓
（可选）/prototype            ← UI / 合成逻辑不确定时
        ↓
/to-prd                       ← 生成 PRD
        ↓
/to-issues                    ← 拆成独立 Issue
        ↓
开始写代码 → 自动用 tdd / codebase-design
出 bug 了 → 自动用 diagnosing-bugs
```

### 上下文管理

- **步骤 1–3**（grill → PRD → issues）尽量保持在**同一次对话**里，不要中途 compact，这样 PRD 和 Issue 能继承同一套思考。
- 每个 Issue 的实现建议**新开对话**，避免上下文过长。
- 对话快满时，用 `/handoff` 生成交接文档，在新会话里引用该文件继续。

## FitStudio 场景对照

| 场景 | 用哪个 |
|------|--------|
| 第一次用这套 skills | `/setup-matt-pocock-skills` |
| 「模特、服装、场景、合成」术语怎么定 | `/grill-with-docs` |
| 试衣 UI 要几种布局 | `/prototype`（UI 分支） |
| 合成流程的状态机对不对 | `/prototype`（Logic 分支） |
| 聊完了，要正式需求文档 | `/to-prd` |
| PRD 好了，要拆开发任务 | `/to-issues` |
| 写「选服装 → 调用 AI 合成」功能 | 直接开发，Agent 会用 `tdd` |
| 合成效果不稳定 | 说「帮我 debug」，Agent 会用 `diagnosing-bugs` |
| 对话太长要新开窗口 | `/handoff` |
| 不知道下一步用啥 | `/ask-matt` |

## 安装与维护

### 精确安装（仅本项目需要的 skills）

```bash
npx skills add mattpocock/skills \
  -s setup-matt-pocock-skills,grill-with-docs,to-prd,to-issues,prototype,tdd,diagnosing-bugs,codebase-design,grill-me,handoff,ask-matt \
  -y
```

`grilling` 和 `domain-modeling` 会作为依赖一并安装。

### 查看已安装的 skills

```bash
npx skills list -p
```

### 更新 skills

```bash
npx skills update -p -y
```

## 相关文档

- [mattpocock/skills README](https://github.com/mattpocock/skills)
- [skills.sh](https://skills.sh/mattpocock/skills) — 技能市场与安装统计
