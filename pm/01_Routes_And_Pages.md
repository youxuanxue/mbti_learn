# 路由与页面规格（H5 / P0–P7）

> 对齐：`docs/02_UI_UX.md` 的 P0–P7 页面定义  
> MVP：点读 + 选择；目标“会用在对话里”；含订阅与 0 元创始会员

## 1. 路由表（建议）

| 页面 | 路由 | 入口 | 出口 |
|---|---|---|---|
| P0 落地页 | `/` | 外部链接/分享 | `/quiz` 或 `/home` |
| P1 快测 | `/quiz` | P0 CTA / `/home` 引导 | `/result` |
| P2 结果页 | `/result` | `/quiz` 完成 | `/level/:levelId`（推荐首关） |
| P3 首页 | `/home` | 首次链路末尾 / 回访 | `/level/:levelId`、`/paywall`、`/library` |
| P4 关卡页 | `/level/:levelId` | 首页推荐/继续上次 | `/summary/:levelId` |
| P5 结算页 | `/summary/:levelId` | 关卡完成 | 下一关 `/level/:nextId`、`/paywall`、`/home` |
| P6 收藏/复习 | `/library` | 首页入口 | `/level/:levelId` 或返回 |
| P7 订阅/创始会员 | `/paywall` | 首页/结算触发 | 返回 `/home` 或原页面 |

## 2. 全局状态机（页面级）
MVP 需要确保“首局闭环”稳定：
1) 首次进入：若无 `mbti_type` → 引导快测（P1）
2) 若已有 `mbti_type`：
   - `last_level_id` 存在 → 首页展示“继续上次”
   - 否则 → 首页展示“今日推荐”

全局存储建议（localStorage）：
- `user_id`（匿名也可）
- `mbti_type`、`learning_tags[]`
- `last_level_id`、`last_scene_id`
- `favorites[]`（sentence_id）
- `founder_status`、`subscription_status`（MVP 可先占位）

## 3. 页面规格（逐页）

## 3.1 P0 落地页（`/`）
**目标**：最短路径进入体验链路。

### UI 组件
- 标题/一句话定位
- CTA1：开始测试（跳 `/quiz`）
- CTA2：直接开玩（跳 `/home`，若无测评则在首页引导快测）

### 状态
- 正常
- 弱网：骨架屏 + 继续按钮可点击

### 必要事件
- `landing_view`
- `cta_click`（属性：`target=quiz|home`）

## 3.2 P1 MBTI 趣味快测（`/quiz`）
**目标**：4 题、30 秒完成；产出 `mbti_type` 与 `learning_tags`。

### 数据模型（简化）
- questions[4]：每题 A/B
- answers：`E/I`、`S/N`、`T/F`、`J/P`

### UI/交互
- 进度：`current/4`
- 选项点击即进入下一题（无提交按钮）
- 完成后跳 `/result`

### 状态与边界
- 中途退出：保留已选答案（可选），回到继续作答
- “我不确定”（若启用）：默认随机/不计分（产品可选）

### 必要事件
- `quiz_start`
- `quiz_answer`（属性：`q_idx`,`choice=A|B`）
- `quiz_complete`（属性：`mbti_type`,`learning_tags`）

## 3.3 P2 类型结果页（`/result`）
**目标**：展示类型卡 + 推荐理由 + “立即开玩”。

### UI
- MBTI 类型卡（如 ENFP）
- 学习性格标签（如 热场型/规则型/共情型/探索型）
- 推荐理由 3 条（文案可先静态）
- CTA：立即开玩 → 跳转推荐首关（levelId）

### 推荐首关规则（MVP）
优先 11-01/02/03（详见 `docs/01_PRD.md`）
- J 倾向：11-03（约定时间）
- F 倾向：11-01（邀请/安抚）
- 其他默认：11-02（推迟/给时间）

### 必要事件
- `result_view`
- `result_start_level_click`（属性：`level_id`）

## 3.4 P3 首页（`/home`）
**目标**：让用户 1 秒内知道“今天玩什么”，并提供订阅入口。

### UI
- 今日推荐卡：场景名、目标、预计用时
- 继续上次（若有）
- 入口：创始会员/订阅（`/paywall`）、收藏（`/library`）

### 状态
- 无测评：显示“先测一下更准”引导 → `/quiz`
- 有测评无关卡数据：fallback 到内置默认关卡 ID
- 订阅/创始会员状态（MVP 可先用假数据）

### 必要事件
- `home_view`
- `home_start_level_click`（属性：`level_id`,`source=today|continue`）
- `paywall_entry_click`（属性：`source=home`）

## 3.5 P4 关卡页（`/level/:levelId`）
**目标**：渲染关卡脚本，完成题目并给反馈，结束后进入结算。

### 数据来源
- `content/levels_seed.json`（levels）
- `content/content_seed.json`（sentences）

### Round 类型与渲染规则（MVP）
1) `choice`
   - 显示 `context[]`（可为自定义文案或句子）
   - 显示 `prompt_zh`
   - options：按 `sentence_id` 渲染 SentenceCard（英文大字 + 中文小字 + 点读）
2) `next_line`
   - 显示 `dialogue_so_far[]`（按 role 画气泡）
   - 选择下一句 options
3) `intent_choice`
   - 显示一句 sentence + 问“这句更像在做什么？”

### 点读音频
MVP 不做 TTS/ASR 的情况下：
- 方案 A（最快）：前端集成 Web Speech API TTS（注意 iOS 限制，需要用户手势触发）
- 方案 B：后端/静态音频资源（后续补齐）

### 状态
- 加载中：骨架屏
- 题目作答中：选项可点
- 已作答：展示反馈层；锁定选项；“继续下一题”按钮
- 结束：自动跳 `/summary/:levelId`

### 必要事件（关卡核心）
对齐 `docs/05_Analytics_QA.md`：
- `level_start`（属性：`level_id`,`scene_id`,`template_type`）
- `question_view`（属性：`question_id`,`type`）
- `answer_submit`（属性：`question_id`,`chosen_option_id`,`is_correct`）
- `sentence_audio_play`（属性：`sentence_id`）
- `level_complete`（属性：`level_id`,`correct_count`,`total_count`,`duration_ms`）

## 3.6 P5 结算页（`/summary/:levelId`）
**目标**：展示“掌握/错题/收藏”，引导下一关与付费。

### UI
- 本关表现：正确数、星级（可用正确率映射）
- 掌握句子列表（可点读/收藏）
- 错题列表（可“再练错题” → 进入同关卡复练模式或单独复习关卡）
- CTA：下一关 / 回首页 / 去订阅

### 必要事件
- `summary_view`
- `summary_next_level_click`
- `paywall_entry_click`（属性：`source=summary`）
- `favorite_toggle`（属性：`sentence_id`,`on=true|false`）
- `retry_wrong_click`

## 3.7 P6 收藏/复习（`/library`）
**目标**：把收藏句子变成可复习的轻任务。

### UI
- 收藏句子列表（SentenceCard）
- 操作：点读、取消收藏
- CTA：开始复习（可进入 `lvl_review_002` 之类的复习关）

### 必要事件
- `library_view`
- `favorite_toggle`
- `library_start_review_click`（属性：`level_id`）

## 3.8 P7 订阅/创始会员（`/paywall`）
**目标**：转化；同时提供 0 元创始会员加入。

### UI
- 模块 1：创始会员 0 元加入（权益、条件）
- 模块 2：订阅（权益、周期、价格占位）
- FAQ：自动续费/取消

### 状态
- 未登录/匿名：允许加入创始会员（用 user_id 绑定）
- 支付成功：更新 `subscription_status`

### 必要事件
- `paywall_view`
- `founder_join_click`、`founder_join_success`
- `subscribe_start`、`subscribe_success`、`subscribe_fail`


