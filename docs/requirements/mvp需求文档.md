# FitStudio MVP需求文档

> 领域术语以根目录 [`CONTEXT.md`](../../CONTEXT.md) 为准。本文描述 MVP 范围、用户故事、页面功能与验收标准；技术栈与实现方案另文讨论。

## 1. 概述

FitStudio 是面向服装行业的虚拟 AI 试衣间平台。Brand Operator 录入模特、场景与 SKU，在合成页组合素材并调用大模型生成试穿效果图；满意的成图加入资产库，导出后由人工上传至电商商品详情页，供 Shopper 浏览。

**MVP 目标**：跑通「录入 → 合成 → 记录 → 入库 → 导出」主流程，验证 AI 试衣对 Brand Operator 可用、对 Shopper 有展示价值。

## 2. 角色

| 角色 | 说明 | MVP 是否使用 FitStudio |
|------|------|------------------------|
| **Brand Operator** | 商家侧运营，管理素材、发起合成、导出成图 | 是，唯一操作用户 |
| **Shopper** | 电商详情页终端买家 | 否，仅消费导出后的图片 |

## 3. 问题与方案

### 问题

Brand Operator 需要为服装商品制作模特上身效果图，传统拍摄成本高、周期长，且难以快速尝试不同模特、场景与穿搭组合。

### 方案

1. 将模特、场景、SKU 作为可复用资产录入并管理。
2. 在合成页选择素材组合，由系统填充可编辑的 Prompt Template，调用大模型生成试穿图。
3. 每次合成保留完整 Generation Record（成败可查），成功结果产生若干 Generation Output 变体。
4. Brand Operator 挑选满意的 Output 加入 Try-on Image 资产库，需要时导出（Publish），人工上传至电商详情页。

## 4. MVP 主流程

```
录入 Model Asset / Scene Asset / SKU
        ↓
合成页：选 1 模特 + 1 场景 + 多个 SKU
        ↓
Prompt Template 自动填词 → Brand Operator 可修改 → 选择生成张数 → 发起 Try-on Job
        ↓
产生 Generation Record（合成页列表，成败均可见）
        ↓
成功 → 若干 Generation Output（同套穿搭的不同变体）
        ↓
满意 → 加入 Asset Library（Try-on Image 分库）
        ↓
Publish → 导出图片 → 人工上传电商详情页 → Shopper 浏览
```

## 5. 用户故事

### 录入资产

1. 作为 Brand Operator，我希望上传自己的 Model Asset，以便在合成时使用品牌专属模特。
2. 作为 Brand Operator，我希望从 Platform Model Library 选用预设模特，以便无需自备模特图也能快速试衣。
3. 作为 Brand Operator，我希望上传自己的 Scene Asset，以便控制成图背景风格。
4. 作为 Brand Operator，我希望从 Platform Scene Library 选用预设场景，以便快速获得常用拍摄环境。
5. 作为 Brand Operator，我希望预先录入 SKU（名称、Garment Category、商品图），以便合成时从已有商品库中选用。
6. 作为 Brand Operator，我希望在录入 SKU 时可选填款号，以便与内部商品系统对应。
7. 作为 Brand Operator，我希望在 Asset Library 中分栏查看模特、SKU、场景、试穿图，以便统一管理素材。

### 试衣合成

8. 作为 Brand Operator，我希望在合成页选择一个 Model Asset、一个 Scene Asset 与一个或多个 SKU，以便组合整套穿搭（如上衣 + 下装 + 配饰）。
9. 作为 Brand Operator，我希望系统根据所选素材自动填充 Prompt Template，以便降低编写提示词的门槛。
10. 作为 Brand Operator，我希望在发起合成前手动修改提示词，以便微调生成效果。
11. 作为 Brand Operator，我希望指定本次生成的张数，以便一次获得多个变体供挑选。
12. 作为 Brand Operator，我希望点击合成后调用大模型，以便得到 AI 试穿效果图。
13. 作为 Brand Operator，我希望每次合成产生一条 Generation Record，以便追溯每次调用的输入与结果。
14. 作为 Brand Operator，我希望在合成页列表中查看所有 Generation Record，以便回顾历史尝试。
15. 作为 Brand Operator，我希望在每条 Generation Record 中查看当时使用的提示词、参考图与生成参数，以便复现或排查问题。
16. 作为 Brand Operator，我希望合成失败时仍保留 Generation Record 并显示错误信息，以便了解失败原因并决定是否重试。
17. 作为 Brand Operator，我希望合成成功时在同一 Record 下看到多张 Generation Output，以便从中挑选最满意的一张。
18. 作为 Brand Operator，我希望明确多张 Output 是同一套穿搭的不同变体，而不是每个 SKU 各一张图，以便正确理解生成结果。

### 资产入库与发布

19. 作为 Brand Operator，我希望将满意的 Generation Output 加入 Asset Library 的 Try-on Image 分库，以便将「试验结果」沉淀为「正式资产」。
20. 作为 Brand Operator，我希望入库的 Try-on Image 保留来源的 Model Asset、Scene Asset 与 SKU 关联，以便在资产页了解这张图的组合构成。
21. 作为 Brand Operator，我希望未入库的候选图仅保留在 Generation Record 中，以便资产库只展示认可的结果。
22. 作为 Brand Operator，我希望从 Try-on Image 资产库中导出图片，以便上传到电商商品详情页。
23. 作为 Shopper，我希望在电商商品详情页看到 Try-on Image，以便直观了解服装上身效果（MVP 由 Brand Operator 人工上传实现）。

## 6. 页面与功能

### 6.1 Asset Library（资产库）

四个分栏，支持列表展示与基本操作（查看、新增、删除；编辑为可选增强）。

| 分栏 | 内容 | 新增方式 |
|------|------|----------|
| Model Asset | 已入库模特 | 上传 或 从 Platform Model Library 选用 |
| Scene Asset | 已入库场景 | 上传 或 从 Platform Scene Library 选用 |
| SKU | 已录入商品 | 填写名称、Garment Category、商品图；款号可选 |
| Try-on Image | 已入库试穿成图 | 仅能从 Generation Output「加入资产」产生，不支持直接上传 |

**说明**：Generation Record 不在资产库中展示。

### 6.2 合成页

参考「左侧配置 + 中间记录列表 + 右侧历史缩略图」布局（具体交互可设计阶段细化）。

**左侧 — 合成配置**

- 选择 Model Asset（从已入库或平台库）
- 选择 Scene Asset（从已入库或平台库）
- 选择多个 SKU（从已录入 SKU，按 Garment Category 区分）
- Prompt Template 自动填充的提示词编辑区
- 生成张数选择
- 「合成」按钮 → 发起 Try-on Job

**中间 — Generation Record 列表**

- 按时间倒序展示调用记录
- 每条记录展示：状态（进行中 / 成功 / 失败）、时间、所用模型/参数摘要
- 点击记录可查看详情：完整提示词、参考图快照、生成参数、错误信息（若失败）、当前记录下的 Generation Output 大图（若成功）

**右侧 — 历史缩略图**

- 竖排展示历次 Generation Output 的缩略图，便于快速浏览历史生成结果
- 点击缩略图可定位到中间对应的 Generation Record 并查看大图
- 对满意的缩略图可执行「加入资产」→ 写入 Try-on Image 分库
- 支持下载单张 Output（可选，便于快速取用）

### 6.3 Try-on Image 资产操作

- 预览大图
- 查看关联的 Model Asset、Scene Asset、SKU
- Publish：导出图片文件（如下载）

## 7. 业务规则

| 规则 | 说明 |
|------|------|
| SKU 录入必填 | 名称、Garment Category、一张商品图 |
| SKU 录入选填 | 款号 |
| 合成输入 | 1 个 Model Asset + 1 个 Scene Asset + 1 个或多个 SKU |
| 合成产出 | 1 条 Generation Record；成功时含 N 张 Generation Output（N 为 Brand Operator 所选张数） |
| Output 含义 | 多张 Output = 同套穿搭的不同变体，非每 SKU 一张 |
| 入库 | 仅 Brand Operator 主动操作可将 Generation Output 变为 Try-on Image |
| 发布 | MVP 仅导出文件，不对接电商平台 API |
| 失败处理 | 保留 Generation Record + 错误信息；Brand Operator 可修改参数后重新发起 Try-on Job，无自动重试要求 |
| Garment Category | MVP 至少支持：上衣、下装、配饰（可扩展） |

## 8. 验收标准

### 8.1 录入

- [ ] 可上传 Model Asset 并在资产库模特分栏看到
- [ ] 可从 Platform Model Library 选用模特并出现在资产库
- [ ] 可上传 Scene Asset 并在资产库场景分栏看到
- [ ] 可从 Platform Scene Library 选用场景并出现在资产库
- [ ] 可录入 SKU（名称 + Garment Category + 商品图），并在 SKU 分栏看到
- [ ] 未填必填项时无法保存 SKU

### 8.2 合成

- [ ] 合成页可选择已入库的 Model Asset、Scene Asset 与一个或多个 SKU
- [ ] 选择素材后 Prompt Template 自动填充提示词
- [ ] 可修改提示词后发起合成
- [ ] 可选择生成张数并发起 Try-on Job
- [ ] 发起后合成页列表出现新的 Generation Record
- [ ] 合成进行中 Record 显示进行中状态
- [ ] 合成成功时 Record 内可见对应数量的 Generation Output
- [ ] 合成失败时 Record 显示失败状态与错误信息，无 Output
- [ ] 可查看 Record 详情中的提示词、参考图与参数

### 8.3 入库与发布

- [ ] 可将 Generation Output「加入资产」，出现在 Try-on Image 分栏
- [ ] Try-on Image 可查看来源模特、场景、SKU 关联
- [ ] 未加入资产的 Output 不出现在 Try-on Image 分栏
- [ ] 可从 Try-on Image 分栏导出图片文件

### 8.4 端到端

- [ ] Brand Operator 可独立完成：录入素材 → 合成 → 挑选 → 入库 → 导出
- [ ] 导出图片可人工上传至电商详情页并由 Shopper 浏览（FitStudio 外验证）

## 9. 不在 MVP 范围内

- 可发布率、质量指标、统计看板
- Hard Reject 自动规则引擎
- 失败后自动重试与 Retry Budget
- 灰度发布、自动回滚
- 电商平台 API 对接与自动上架
- Shopper 端 FitStudio 界面
- 平台库（模特/场景）的后台管理界面（MVP 可用种子数据预置）
- 多租户 / 权限体系（若需要可极简处理，不单独立项）
- 改图、局部重绘等二次编辑能力

## 10. 待定事项

以下不阻塞 MVP 开发，实现前或 PRD 迭代时确认：

| 议题 | 选项 |
|------|------|
| 平台库选用机制 | 引用 vs 复制到品牌资产 |
| 资产删除 | 删除后历史 Generation Record 如何展示关联 |
| 同一 Output 重复入库 | 建议禁止 |
| 生成张数上限 | 需产品与技术共同确定 |
| Prompt Template 具体内容 | 需与模型能力对齐后定义 |

## 11. 关联文档

- 领域词汇：[`CONTEXT.md`](../../CONTEXT.md)
- 技术设计：`docs/design/`（待补充）
- 架构决策：`docs/adr/`（待补充）
