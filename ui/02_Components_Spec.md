# 组件规格（给前端工程师 Agent）

> 对齐：`docs/02_UI_UX.md` 的组件建议  
> 目标：组件可复用支撑 P0–P7 全流程

## 1. SentenceCard
用于展示句子（英文/中文/点读/收藏）。

### Props（建议）
- `sentenceId?: string`（优先）或 `en/ch` 直传
- `en: string`（展示用，取 `en_display`）
- `ch?: string`
- `showChinese: boolean`
- `onAudioPlay?: (sentenceId) => void`
- `isFavorite?: boolean`
- `onToggleFavorite?: (sentenceId, on) => void`

### 交互
- 点击“扬声器”触发点读（必须用户手势）
- 点击收藏切换状态（显示 toast “已收藏/已取消”）

## 2. DialogueBubble
用于对话气泡（Child/Mom/Dad 角色色区分）。

### Props
- `role: 'Child' | 'Mom' | 'Dad' | 'System'`
- `content`: ReactNode（可嵌 SentenceCard 的纯文本版）

### 样式
- 根据 role 使用 `--color-role-*`
- 需要长句自动换行

## 3. OptionButton
用于选择题选项（2–4 个）。

### Props
- `state: 'default' | 'selected' | 'correct' | 'wrong' | 'disabled'`
- `primaryText: string`（英文）
- `secondaryText?: string`（中文）
- `onClick`

### 规范
- 高度 ≥ 48px
- 错误态用低饱和红底（`--color-error-bg`），避免刺眼

## 4. ProgressBar
用于题目/关卡进度。

### Props
- `current: number`
- `total: number`

## 5. FeedbackPanel（题目反馈层）

### Props
- `type: 'correct' | 'wrong'`
- `textZh: string`
- `onNext: () => void`

### 行为
- 提交后显示；锁定选项；“继续”按钮固定底部

## 6. PageScaffold（页面骨架）
统一顶部导航与底部 CTA。

### Props
- `title?: string`
- `showBack?: boolean`
- `rightSlot?: ReactNode`（静音/音量）
- `bottomBar?: ReactNode`（CTA）

## 7. PaywallCard（订阅/创始会员）
用于 `/paywall` 的权益模块。

### Props
- `title`
- `priceText`（创始会员可为“0元”）
- `benefits: string[]`
- `ctaText`
- `onCtaClick`
- `badgeText?`（例如“限时/名额有限”）


