# 埋点事件映射 & 页面验收清单（给前端工程师 Agent）

> 事件口径对齐：`docs/05_Analytics_QA.md`  
> 本文件补齐“哪个页面触发哪些事件、必须带哪些字段、验收如何判定”

## 1. 事件字典（MVP 必做）

### 1.1 进入与链路
- `landing_view`
- `home_view`
- `paywall_view`

### 1.2 快测
- `quiz_start`
- `quiz_answer`
  - 必填：`q_idx`、`choice`
- `quiz_complete`
  - 必填：`mbti_type`、`learning_tags`

### 1.3 关卡
- `level_start`
  - 必填：`level_id`、`scene_id`、`template_type`
- `question_view`
  - 必填：`level_id`、`question_id`、`type`
- `answer_submit`
  - 必填：`level_id`、`question_id`、`chosen_option_id`、`is_correct`
- `sentence_audio_play`
  - 必填：`sentence_id`、`level_id`
- `level_complete`
  - 必填：`level_id`、`correct_count`、`total_count`、`duration_ms`

### 1.4 收藏与复习
- `favorite_toggle`
  - 必填：`sentence_id`、`on`
- `library_view`
- `library_start_review_click`
  - 必填：`level_id`
- `retry_wrong_click`

### 1.5 商业化
- `founder_join_click` / `founder_join_success`
- `subscribe_start` / `subscribe_success` / `subscribe_fail`

## 2. 事件属性（建议全局默认附带）
每个事件尽量自动携带：
- `user_id`
- `session_id`
- `mbti_type`（若已测）
- `learning_tags`（若已测）
- `content_version`（来自 seed 文件或 remote config）

## 3. 页面→事件映射（必触发）

### P0 `/`
- `landing_view`
- CTA 点击：`cta_click`（可选）

### P1 `/quiz`
- 首次进入：`quiz_start`
- 每题答题：`quiz_answer`
- 完成：`quiz_complete`

### P2 `/result`
- `result_view`（可选，但建议）
- 点击开玩：`result_start_level_click`（可选）

### P3 `/home`
- `home_view`
- 点击推荐卡：`home_start_level_click`
- 点击订阅入口：`paywall_entry_click`

### P4 `/level/:levelId`
- 页面加载成功：`level_start`
- 每题呈现：`question_view`
- 提交答案：`answer_submit`
- 点读：`sentence_audio_play`
- 完成：`level_complete`

### P5 `/summary/:levelId`
- `summary_view`（可选，但建议）
- 下一关：`summary_next_level_click`（可选）
- 收藏：`favorite_toggle`
- 去订阅：`paywall_entry_click`

### P6 `/library`
- `library_view`
- 收藏开关：`favorite_toggle`
- 开始复习：`library_start_review_click`

### P7 `/paywall`
- `paywall_view`
- `founder_join_click` → 成功 `founder_join_success`
- `subscribe_start` → 成功/失败

## 4. 页面验收清单（给 FE 自测/QA）

## 4.1 首局闭环验收（P0→P5）
- 能从 `/` 进入 `/quiz` 完成 4 题，并生成 `mbti_type`
- `/result` 点击“立即开玩”能进入有效 `levelId`
- 关卡页能完成全部 rounds
- 每题答完后必须出现反馈，并且按钮“继续”进入下一题
- 完成关卡后进入结算页，显示掌握句子与错题（若有）
- 核心事件全部上报（见第 1 章）

## 4.2 儿童友好验收（对齐 UI/UX）
- 选项数量 ≤ 4（建议 2–3）
- 点击目标 ≥ 44px
- 默认中文开启，且可“一键隐藏中文”
- 错误反馈不羞辱：必须包含“为什么更合适/这句通常在什么场景用”

## 4.3 弱网/异常流验收
- 内容加载失败：提示 + 重试按钮（不白屏）
- 音频不可用：提示 + 不阻塞做题
- 重复点击支付/加入：幂等（按钮 loading + 禁用）


