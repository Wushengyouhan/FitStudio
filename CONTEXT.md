# FitStudio

虚拟 AI 试衣间平台，专注服装行业。品牌运营录入 SKU、模特与场景素材，在合成页组合发起试衣并保留调用记录；满意的结果加入资产库，MVP 阶段导出后由人工上传至商品详情页，供终端买家浏览。

## Language

### 角色

**Brand Operator**:
商家侧负责商品上架与素材管理的运营人员，是 FitStudio 的主要使用者。
_Avoid_: 商家、运营、管理员

**Shopper**:
在电商商品详情页浏览 Try-on Image 的终端买家；不是 FitStudio 的操作用户。
_Avoid_: 用户、消费者、C 端用户

### 录入资产

**Model Asset**:
用于试衣合成的模特参考图；Brand Operator 可自行上传，也可从 Platform Model Library 选用。
_Avoid_: 模特图、人像素材、模特资源

**Platform Model Library**:
FitStudio 提供的预设模特集合，供 Brand Operator 直接选用而无需自行上传。
_Avoid_: 公共模特池、系统模特、官方模特库

**Scene Asset**:
试衣合成使用的背景场景参考图，决定成图中模特所处的环境；Brand Operator 可自行上传，也可从 Platform Scene Library 选用。
_Avoid_: 背景图、场景素材、环境图

**Platform Scene Library**:
FitStudio 提供的预设场景集合，供 Brand Operator 直接选用而无需自行上传。
_Avoid_: 公共场景池、系统背景、官方场景库

**Garment Category**:
SKU 所属的穿着品类，标识上衣、下装、配饰等，用于多 SKU 穿搭组合时的选取与区分。
_Avoid_: 商品分类、类型、类目

**SKU**:
品牌方预先录入 FitStudio 的单件可试穿衣服商品；录入时必填名称、Garment Category 与一张商品图，款号可选；试衣合成时从已有 SKU 中选用，不在合成时临时上传。
_Avoid_: 商品、货品、SPU

### 合成

**Try-on Job**:
Brand Operator 在合成页选定一个 Model Asset、一个 Scene Asset 与一个或多个 SKU，确认或修改 Prompt Template 填充后的提示词，并指定本次生成张数后发起的试衣合成请求；每次发起产生一条 Generation Record。
_Avoid_: 合成任务、生成请求、试衣单

**Prompt Template**:
FitStudio 根据所选 Model Asset、Scene Asset 与 SKU 自动填充默认提示词的模板；Brand Operator 可在发起 Try-on Job 前手动修改。
_Avoid_: 提示词模板、Prompt、生成咒语

**Generation Record**:
一次 Try-on Job 的完整调用记录，无论成败均保留于合成页列表；包含当时实际使用的提示词、参考图、生成参数与状态，失败时保留错误信息且无 Generation Output，成功时含若干 Generation Output 供 Brand Operator 挑选。
_Avoid_: 调用记录、生成历史、任务日志

**Generation Output**:
Generation Record 成功时产出的单张候选成图；多张 Output 为同一套穿搭的不同变体，而非每个 SKU 各一张。
_Avoid_: 生成结果、候选图、出图

### 资产与发布

**Try-on Image**:
Brand Operator 从 Generation Output 中挑选并加入 Asset Library 的试穿成图，保留来源的 Model Asset、Scene Asset 与 SKU 关联；未入库的候选图仍仅保留在 Generation Record 中。
_Avoid_: 合成图、效果图、定稿图

**Asset Library**:
Brand Operator 管理已入库素材的分区集合；MVP 按 Model Asset、SKU、Scene Asset、Try-on Image 四类分库展示与维护。Generation Record 不属于 Asset Library，仅在合成页展示。
_Avoid_: 素材中心、资源管理、媒体库

**Publish**:
Brand Operator 从 Asset Library 的 Try-on Image 分库中导出图片；MVP 阶段由人工上传至电商商品详情页，FitStudio 不对接外部电商平台。
_Avoid_: 上线、同步、推送
