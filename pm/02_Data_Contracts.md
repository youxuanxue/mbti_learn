# 数据契约（给前端工程师 Agent）

> 目标：前端可以不等后端，先用本地 JSON 跑通；后续换成 API 不改业务逻辑。

## 1. 本地数据文件（MVP）
- 句子与场景：`content/content_seed.json`
- 关卡脚本：`content/levels_seed.json`

前端建议在构建时打包，并支持“远程覆盖”（见第 4 章 Remote Config）。

## 2. Quiz / MBTI 数据契约

### 2.1 QuizQuestion（前端内置即可）
```json
{
  "id": "q_ei",
  "dimension": "E_I",
  "title_zh": "你更想怎么学？",
  "options": [
    { "id": "A", "label_zh": "和人一起玩对话更有劲", "value": "E" },
    { "id": "B", "label_zh": "我先自己闯关，熟了再一起玩", "value": "I" }
  ]
}
```

### 2.2 QuizResult（前端计算后持久化）
```json
{
  "mbti_type": "ENFP",
  "dimensions": { "E_I": "E", "S_N": "N", "T_F": "F", "J_P": "P" },
  "learning_tags": ["热场型", "探索型"],
  "created_at": 1760000000000
}
```

## 3. Content Seed（`content/content_seed.json`）

### 3.1 Scene
```json
{
  "id": "scene_11_02_play_later",
  "chapter_name": "CHAPTER11-02 下次和你玩",
  "title": "下次和你玩（现在不行）",
  "roles": ["Child", "Mom", "Dad"],
  "learning_goals_zh": ["..."],
  "primary_intents": ["DeclineNow", "DelayWithTime", "AskWait"],
  "sentence_ids": ["s_11_02_001", "s_11_02_002"]
}
```

### 3.2 Sentence
```json
{
  "id": "s_11_02_009",
  "chapter_name": "CHAPTER11-02 下次和你玩",
  "en_raw": "Wait for five minutes.",
  "en_display": "Wait for five minutes.",
  "ch": "等我五分钟。",
  "role_suggested": "Mom",
  "intent": "DelayWithTime",
  "tags": ["wait", "time"],
  "difficulty": 1
}
```

说明：
- `en_raw` 保留原始来源
- `en_display` 用于 UI 展示（可做轻量纠错与统一标点）

## 4. Level Script（`content/levels_seed.json`）

### 4.1 Level
```json
{
  "id": "lvl_11_02_002",
  "scene_id": "scene_11_02_play_later",
  "template": "A",
  "title_zh": "给出具体时间（等 5 分钟）",
  "estimated_minutes": 3,
  "rounds": []
}
```

### 4.2 Round：choice
```json
{
  "id": "r1",
  "type": "choice",
  "context": [
    { "role": "Mom", "sentence_id": "s_11_02_003" }
  ],
  "prompt_role": "Mom",
  "prompt_zh": "接下来妈妈想给一个具体等待时间：5 分钟。选哪句？",
  "options": [
    { "id": "o1", "sentence_id": "s_11_02_009" },
    { "id": "o2", "sentence_id": "s_11_03_013" }
  ],
  "correct_option_id": "o1",
  "feedback": {
    "correct_zh": "…",
    "wrong_by_option_zh": { "o2": "…" }
  }
}
```

### 4.3 Round：next_line
```json
{
  "id": "r1",
  "type": "next_line",
  "dialogue_so_far": [
    { "role": "Child", "sentence_id": "s_11_01_005" },
    { "role": "Mom", "sentence_id": "s_11_02_003" }
  ],
  "prompt_role": "Child",
  "prompt_zh": "孩子追问：那你什么时候有空？选哪句？",
  "options": [
    { "id": "o1", "sentence_id": "s_11_02_021" }
  ],
  "correct_option_id": "o1",
  "feedback": { "correct_zh": "…", "wrong_by_option_zh": {} }
}
```

### 4.4 Round：intent_choice
```json
{
  "id": "r1",
  "type": "intent_choice",
  "sentence_id": "s_11_03_017",
  "prompt_zh": "这句话更像在做什么？",
  "options": [
    { "id": "o1", "intent": "EndPlay", "label_zh": "结束游戏" },
    { "id": "o2", "intent": "InvitePlay", "label_zh": "邀请一起玩" }
  ],
  "correct_option_id": "o1",
  "feedback": {
    "correct_zh": "…",
    "wrong_by_option_zh": { "o2": "…" }
  }
}
```

## 5. Remote Config（可选：后端上线后替换）
MVP 可先本地写死；后续建议一个配置接口提供：

```json
{
  "content_version": "0.1.0",
  "today_recommendations": [
    { "mbti_hint": "J", "level_id": "lvl_11_03_001" },
    { "mbti_hint": "F", "level_id": "lvl_11_01_001" }
  ],
  "paywall": {
    "founder_enabled": true,
    "subscribe_enabled": true
  }
}
```

## 6. 本地存储 Key 约定（localStorage）
- `mbti.user_id`：匿名 ID
- `mbti.quiz_result`：QuizResult JSON
- `mbti.last_level_id`
- `mbti.favorites`：`string[]`（sentence_id）
- `mbti.founder_status`：`none|joined`
- `mbti.subscription_status`：`none|active`


