# 前端工程师 Agent 实现指南（MVP）

## 1. 你要实现的最小闭环
1) `/quiz`：4 题趣味测 → 产出 `mbti_type` + `learning_tags`
2) `/result`：展示类型卡 → 进入推荐首关
3) `/level/:id`：关卡引擎（choice/next_line/intent_choice）+ 点读
4) `/summary/:id`：结算（掌握/错题/收藏）+ 下一关/去订阅
5) `/paywall`：创始会员 0 元加入 + 订阅入口（支付可先 mock）
6) `/home`：今日推荐/继续上次/入口聚合

## 2. 先用本地 JSON 跑起来
- 句子与场景：`content/content_seed.json`
- 关卡脚本：`content/levels_seed.json`

要求：不用后端，也能全链路走通（后续替换成 remote config/API）。

## 3. 关卡引擎实现要点
- 根据 `level.rounds[]` 顺序渲染题目
- 每题只允许提交一次：提交后锁定选项、展示反馈、出现“继续”
- 反馈优先读 `feedback.wrong_by_option_zh[chosen]`，没有就给通用提示
- `next_line` 的 `dialogue_so_far[]` 用角色气泡渲染

## 4. 点读（音频）策略
MVP 推荐用 Web Speech API（TTS）快速实现：
- iOS/Safari 可能要求用户手势触发：把“点读”绑定到 click
- 失败要降级：提示“当前浏览器不支持点读”，不影响答题

## 5. 埋点必须做
按 `pm/03_Events_And_Acceptance.md` 的事件字典实现。
最低要求：
- `quiz_start/quiz_complete`
- `level_start/question_view/answer_submit/level_complete`
- `paywall_view`

## 6. 本地存储（localStorage）
按 `pm/02_Data_Contracts.md` 的 key 约定实现，确保：
- 刷新页面不丢 `mbti_type`
- 能“继续上次关卡”（`last_level_id`）
- 收藏句子持久化

## 7. UI 实现参考
组件规范与 tokens 在 `ui/` 目录：
- `ui/01_Design_Tokens.md`
- `ui/02_Components_Spec.md`
- `ui/03_Screens_And_States.md`

## 8. 交付验收
以 `pm/03_Events_And_Acceptance.md` 的“首局闭环验收”为准，确保一条链路从 `/` 到 `/summary/:id` 全通。


