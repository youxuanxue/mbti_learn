# 内容策划与关卡设计（基于 `part2.json`）

## 1. 内容目标与原则

### 1.1 目标
把 `part2.json` 的“家庭常用表达”做成可玩的**对话场景关卡**，让学习者在情境里完成“选句/补全对话/意图匹配”，从而达到“会用在对话里”。

### 1.2 原则（6–12岁 + 亲子）
- **短回合**：每关 6–10 题以内，2–3 分钟完成。
- **少输入**：MVP 只做点读 + 选择，靠高频决策和即时反馈。
- **强情境**：每句必须绑定“角色 + 意图 + 场景”。
- **可复用模板**：用少量题型覆盖大量句子。
- **可复习**：错题回炉 + 收藏句子，形成复习闭环。

## 2. 句库概况与内容切分
`part2.json` 结构：chapter（name）→ contents（[{en,ch}]）。

MVP 优先：
- CHAPTER 11-01 要求一起玩的时候（邀请/无聊/一起玩）
- CHAPTER 11-02 下次和你玩（推迟/忙/等待）
- CHAPTER 11-03 约定玩的时间（计时/承诺/结束）
扩展：
- 11-04 躲猫猫（peek-a-boo）
- 11-05 剪刀石头布（规则/胜负）
- 11-06 捉迷藏（it/seeker/hider）
- 11-07 捉人游戏（tag）
- 12-01 画画（请求/描述/鼓励）

## 3. 内容模型（建议用于内容后台/前端 JSON）

### 3.1 结构定义（概念）
- **Scene（场景）**
  - id / title / chapter_ref
  - roles（Child/Mom/Dad）
  - intents（意图集合）
  - sentence_ids（句子引用）
  - difficulty（1–5）
- **Sentence（句子）**
  - id / en / ch
  - role（建议）
  - intent（建议）
  - tags：time、rule、emotion、game、request…
- **Level（关卡）**
  - scene_id
  - level_type（A/B/C）
  - rounds（题目数组）

> 说明：MVP 可先在内容侧“人工标注 role/intent”，后续再考虑半自动/LLM 辅助标注。

## 4. 意图体系（MVP 统一口径）
用于把句子放进“对话里”：
- **InvitePlay**：邀请一起玩
- **RefuseNow**：现在不行/忙
- **DelayWithTime**：等 X 分钟/做完再玩
- **NegotiateDuration**：玩多久/太长/太短
- **EndPlay**：时间到/结束
- **Rules**：规则/犯规/平手/赢家先
- **SeekHide**：躲猫猫/捉迷藏相关
- **Encourage**：鼓励/夸奖（后续扩充）

## 5. 关卡模板（题型库）

## 5.1 模板 A：情境选择（最关键）
**目的**：训练“在此情境下该说哪句”。

- 输入：对话上下文（1–2 句）+ 缺口（角色/意图）
- 输出：2–4 个候选句（含 1 个最佳）
- 反馈：为什么更合适；错句适用场景

可变体：
- 同一句在不同情境下是否合适（提高迁移）
- 选“更温柔/更坚定”版本（F/T 差异化）

## 5.2 模板 B：下一句选择（对话拼图）
**目的**：训练轮次与对话推进。
- 显示 2–4 轮对话，最后一轮缺失
- 用户从选项中选出下一句

## 5.3 模板 C：意图匹配（MVP 先用选择替代拖拽）
**目的**：让孩子理解“这句话是在做什么”（拒绝/约时间/结束…）。
- 给一句英文 → 问“这句更像在：A拒绝 B约时间 C结束…”
- 适合作为复习题

## 6. MBTI/学习偏好与内容编排（可落地）

### 6.1 类型到题型偏好（推荐权重）
- **E**：A/B 比例更高（更多对话轮次），减少纯分类题
- **I**：A/C 比例更高（更安静更聚焦）
- **S**：更高重复率（同意图多例句），反馈短
- **N**：加入“为什么”解释与分支情境
- **T**：反馈以“规则/条件”为主（Now/After/5 minutes）
- **F**：反馈以“情绪安抚/礼貌”为主（Mommy also wants to…）
- **J**：关卡有明确目标（学会 5 句/完成 8 题）
- **P**：随机事件卡（例如“惊喜加分：选对直接连胜”）

## 7. 样例：把 CHAPTER11-02 做成一个 3 分钟关卡包

### 7.1 句子池（节选）
来自 `CHAPTER11-02 下次和你玩`：
- Not now.
- Wait for five minutes.
- I'll play with you after I do the dishes.
- Sorry. Mommy's too busy.
- Mommy also wants to play with you, but I can't right now.

### 7.2 关卡脚本（模板 A + B 混合）

**关卡标题**：下次和你玩（妈妈现在很忙）
**学习目标**：学会“推迟 + 给时间/给原因”的回应
**题目数**：8

#### Round 1（A：情境选择）
- Context（Child）：I want to play right now.
- Prompt（Mom）：你现在要怎么回应？
- Options：
  - Not now. ✅
  - It's play time!
  - Yay! Sounds great!
- Feedback（正确）：现在要拒绝/推迟，用 “Not now.” 最直接。

#### Round 2（A：情境选择）
- Context（Child）：Play with me right away.
- Prompt（Mom）：更温柔一点怎么说？
- Options：
  - Mommy also wants to play with you, but I can't right now. ✅
  - I always win at rock, paper, scissors.
  - Ready or not, here I come!
- Feedback：这句先表达“我也想”，再说明“现在不行”，更适合亲子对话。

#### Round 3（A：情境选择）
- Context（Mom）：I'm working.
- Prompt（Mom）：给出一个具体时间。
- Options：
  - Wait for five minutes. ✅
  - Time's up! Done!
  - I hate to play alone.

#### Round 4（B：下一句选择）
- Dialogue:
  - Child：Mom, can you play with me?
  - Mom：Sorry. Mommy's too busy.
  - Child：Then when are you free then?
  - Mom：_____
- Options：
  - I'll play with you after I do the dishes. ✅
  - Let's play hide and seek.
  - Paper beats rock.

#### Round 5（C：意图匹配）
- Sentence：Wait for five minutes.
- Question：这句话更像在做什么？
- Options：A 推迟并给时间 ✅ / B 结束游戏 / C 邀请一起玩

（Round 6–8：重复不同情境变体，强化迁移。）

## 8. 样例：CHAPTER11-03 “约定玩的时间”做成规则闯关
核心把 “timer / promise / time's up” 变成可视化“时间卡/规则卡”，每题都与卡片绑定。

推荐题型：
- A：当孩子说 “I want to play for 2 hours.” 妈妈怎么协商？
- B：当 alarm goes off 下一句选什么？
- C：把句子分到“开始/进行中提醒/结束”三个阶段

## 9. 内容制作流程（MVP 可执行）
1) 从 `part2.json` 选 3 个场景（11-01/02/03）
2) 人工标注每句的 role/intent（先 50–120 句）
3) 按模板生成关卡脚本（每场景 1 个关卡包，6–10 题）
4) 内测：看首局完成率与错题集中点
5) 迭代：调整选项干扰项与解释文案


