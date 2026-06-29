# FitStudio

虚拟 AI 试衣间平台。品牌运营上传 SKU，平台生成试穿效果图并发布到商品详情页，供终端买家浏览。

## Language

**Brand Operator**:
商家侧负责商品上架与素材管理的运营人员，是 FitStudio 的主要使用者。
_Avoid_: 商家、运营、管理员

**SKU**:
品牌方在 FitStudio 中管理的单件可试穿衣服商品，以商品图和基本属性为入口。
_Avoid_: 商品、货品、SPU

**Try-on Image**:
AI 将 SKU 合成到模特身上后产出的试穿效果图，是 FitStudio 的核心交付物。
_Avoid_: 合成图、效果图、上身图

**Publish-ready Rate**:
无需返工即可直接进入发布流程的 Try-on Image 占比，用来衡量生成质量的业务有效性。
_Avoid_: 质量分、好图率、可用率

**Publish-ready Decision**:
Try-on Image 是否可发布采用双层判定：系统先做硬规则初筛，Brand Operator 做最终确认。
_Avoid_: 自动通过、人工拍板、审核结论

**Hard Reject Rule Set**:
第一阶段系统初筛必须拦截四类问题：主体严重缺失、服装贴合明显错误、图像质量不可用、内容安全违规。
_Avoid_: 兜底规则、质检清单、风控规则

**Recovery Loop**:
Try-on Image 被初筛拦截后默认自动重试并附带失败原因标签，超过重试上限后再转入人工处理。
_Avoid_: 重跑机制、失败兜底、补救流程

**Retry Budget**:
第一阶段每个 Try-on Image 最多允许 3 次自动重试，即总计最多 4 次生成机会。
_Avoid_: 重试次数、尝试上限、重跑配额

**Manual Recovery Action**:
进入人工处理后，Brand Operator 先完成失败原因分类，再执行一键再生成，且允许替换素材。
_Avoid_: 人工介入、补救动作、人工重跑

**Quality Window**:
`Publish-ready Rate` 采用最近 7 天滚动窗口评估，作为第一阶段质量目标的观测口径。
_Avoid_: 统计周期、观察期、周报口径

**Minimum Sample Gate**:
仅当最近 7 天窗口内累计 Try-on Image 样本数达到 200 张及以上时，才允许宣称达到质量目标。
_Avoid_: 样本门槛、统计下限、样本基线

**Rollout Slice**:
第一阶段对 Brand Operator 全量直接上线，不采用分桶灰度发布。
_Avoid_: 灰度比例、发布批次、试点范围

**Rollback Guardrail**:
第一阶段暂不设置自动回滚阈值，是否回滚由人工结合业务与安全风险综合判断。
_Avoid_: 回滚规则、止损线、降级条件

**Rollback Authority**:
在第一阶段策略全量上线期间，是否执行回滚由 Brand Operator 进行最终人工判断并拍板。
_Avoid_: 值班负责人、平台运营、双签审批

**Completion Signal**:
本轮需求是否完成仅由 Brand Operator 的主观确认决定，不设置额外指标达标作为硬性验收门槛。
_Avoid_: 指标验收、双重验收、客观门槛

**Shopper**:
在电商商品详情页浏览 Try-on Image 的终端买家；不是 FitStudio 的操作用户。
_Avoid_: 用户、消费者、C 端用户

专注在服装行业

