# Design Tokens（H5 / 儿童友好）

> 目标：前端可直接转成 CSS Variables，快速统一视觉风格。  
> 原则：高可读、低饱和错误色、不羞辱的反馈体验。

## 1. 颜色（Color）

### 1.1 基础色
- `--color-bg`: `#F7F8FA`
- `--color-surface`: `#FFFFFF`
- `--color-text-primary`: `#111827`
- `--color-text-secondary`: `#6B7280`
- `--color-border`: `#E5E7EB`

### 1.2 角色气泡色
- `--color-role-child`: `#E0F2FE`（浅蓝）
- `--color-role-mom`: `#FCE7F3`（浅粉）
- `--color-role-dad`: `#ECFDF5`（浅绿）

### 1.3 反馈色（低饱和）
- `--color-success`: `#22C55E`
- `--color-success-bg`: `#DCFCE7`
- `--color-warning`: `#F59E0B`
- `--color-warning-bg`: `#FEF3C7`
- `--color-error`: `#EF4444`
- `--color-error-bg`: `#FEE2E2`
- `--color-info`: `#3B82F6`
- `--color-info-bg`: `#DBEAFE`

## 2. 字体与字号（Typography）
- 字体：系统默认（iOS/Android）
- 英文正文：`18–22px`（推荐 20）
- 中文辅助：`12–14px`（推荐 13）
- 标题：`22–28px`
- 行高：英文 `1.3`，中文 `1.4`

## 3. 间距（Spacing）
建议 4px 栅格：
- `--space-1`: 4px
- `--space-2`: 8px
- `--space-3`: 12px
- `--space-4`: 16px
- `--space-5`: 20px
- `--space-6`: 24px

## 4. 圆角与阴影
- `--radius-sm`: 8px
- `--radius-md`: 12px
- `--radius-lg`: 16px

阴影（轻量）：
- `--shadow-card`: `0 2px 10px rgba(0,0,0,0.06)`

## 5. 点击区域与控件尺寸（可用性）
- 最小点击目标：44px（iOS）
- 选项按钮高度：≥ 48px
- 底部 CTA 区高度：≥ 56px

## 6. 动效（可选）
- 过渡：`150–200ms ease-out`
- 正确/错误：轻微震动（H5 可选）+ 颜色反馈（避免强烈闪烁）


