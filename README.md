# mbti_learn

面向 **6–12 岁小学生及亲子家庭** 的英语口语（对话场景）游戏化学习产品：用 **MBTI + 学习性格/偏好** 做轻量分型，基于 `part2.json` 的“美国家庭万用英语表达”设计可复用的场景关卡与玩法。

## 文档入口（docs）

- `docs/01_PRD.md`：产品需求文档（MVP：H5、点读+选择、目标“会用在对话里”、订阅制与0元创始会员）
- `docs/02_UI_UX.md`：信息架构、关键页面、交互与视觉规范（H5）
- `docs/03_Content_Design.md`：内容结构化方案、关卡模板、章节映射、样例脚本（基于 `part2.json`）
- `docs/04_Subscription_Ops.md`：订阅与创始会员策略、活动与增长、内容运营节奏
- `docs/05_Analytics_QA.md`：埋点指标、验收口径、A/B 实验建议与风控
- `docs/06_Level_Scripts.md`：10 个可直接上线的关卡脚本（阅读版）
- `docs/07_CTO_Org_Teams.md`：CTO 研发组织与团队目标/任务（含产品/设计/UI/内容/QA/数据）

## 给前端工程师 Agent 的物料（可直接开工）

- `pm/`：路由/页面规格、数据契约、埋点与验收、实现指南
- `ui/`：设计 tokens、组件规格、页面线框与状态

## 数据

- `part2.json`：内容句库（CHAPTER 11/12 等）
- `content/content_seed.json`：MVP 种子内容（含 role/intent/tags + 可展示英文）
- `content/levels_seed.json`：MVP 关卡脚本（10 个，可直接驱动前端）
