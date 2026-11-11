# 02 - 设计令牌系统（完整版）

> 本文档是 `FRONTEND_STANDARDS.md` 第三、四节的详细展开。
> 提供完整的设计令牌（Design Tokens）定义，可直接用于 CSS 变量、Tailwind 配置、组件库主题定制。

---

## 目录

1. [颜色系统](#颜色系统)
2. [字体系统](#字体系统)
3. [间距系统](#间距系统)
4. [圆角系统](#圆角系统)
5. [阴影系统](#阴影系统)
6. [动效令牌](#动效令牌)
7. [Tailwind 配置模板](#tailwind-配置模板)

---

## 颜色系统

### 中性色色阶（所有预设通用）

使用 Zinc（冷灰）作为默认中性色。其他预设可换成 Stone（暖灰）或 Slate（蓝灰）。

```css
/* Zinc 色阶（预设 A、C、F 使用） */
--zinc-50:  #fafafa;
--zinc-100: #f4f4f5;
--zinc-200: #e4e4e7;
--zinc-300: #d4d4d8;
--zinc-400: #a1a1aa;
--zinc-500: #71717a;
--zinc-600: #52525b;
--zinc-700: #3f3f46;
--zinc-800: #27272a;
--zinc-900: #18181b;
--zinc-950: #09090b;
```

```css
/* Stone 色阶（预设 B、G 使用，暖灰） */
--stone-50:  #fafaf9;
--stone-100: #f5f5f4;
--stone-200: #e7e5e4;
--stone-300: #d6d3d1;
--stone-400: #a8a29e;
--stone-500: #78716c;
--stone-600: #57534e;
--stone-700: #44403c;
--stone-800: #292524;
--stone-900: #1c1917;
```

```css
/* Slate 色阶（预设 D 使用，蓝灰） */
--slate-50:  #f8fafc;
--slate-100: #f1f5f9;
--slate-200: #e2e8f0;
--slate-300: #cbd5e1;
--slate-400: #94a3b8;
--slate-500: #64748b;
--slate-600: #475569;
--slate-700: #334155;
--slate-800: #1e293b;
--slate-900: #0f172a;
```

### 语义化颜色变量

将色阶映射到语义变量，方便主题切换：

```css
:root {
  /* 背景 */
  --bg: var(--zinc-50);
  --bg-subtle: var(--zinc-100);
  --bg-muted: var(--zinc-200);

  /* 文字 */
  --text: var(--zinc-950);
  --text-secondary: var(--zinc-600);
  --text-muted: var(--zinc-400);

  /* 边框 */
  --border: var(--zinc-200);
  --border-strong: var(--zinc-300);

  /* 强调色（由预设决定） */
  --accent: var(--zinc-950);
  --accent-hover: var(--zinc-800);
  --accent-foreground: #ffffff;

  /* 状态色 */
  --success: #16a34a;
  --success-bg: #dcfce7;
  --warning: #d97706;
  --warning-bg: #fef3c7;
  --danger: #dc2626;
  --danger-bg: #fee2e2;
}
```

### 深色模式映射

```css
[data-theme="dark"] {
  --bg: var(--zinc-950);
  --bg-subtle: var(--zinc-900);
  --bg-muted: var(--zinc-800);
  --text: var(--zinc-50);
  --text-secondary: var(--zinc-400);
  --text-muted: var(--zinc-500);
  --border: var(--zinc-800);
  --border-strong: var(--zinc-700);
  --accent: var(--zinc-50);
  --accent-hover: var(--zinc-200);
  --accent-foreground: var(--zinc-950);
}
```

---

## 字体系统

### 推荐字体组合

| 风格 | 标题字体 | 正文字体 | 等宽字体 |
|------|---------|---------|---------|
| 现代中性 | Inter | Inter | JetBrains Mono |
| 高端衬线 | Fraunces | Inter | JetBrains Mono |
| 文艺书卷 | Lora / Source Serif | Inter | IBM Plex Mono |
| 科技精密 | Inter | Inter | JetBrains Mono |
| 活力个性 | Space Grotesk | Inter | Space Mono |
| 企业稳重 | system-ui | system-ui | Consolas |

### 中文字体回退策略

```css
font-family: 'Inter', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
```

### 字号阶梯（Type Scale）

基于 16px 基准，使用 1.25 或 1.333 比率：

```css
--text-xs:   12px;   /* 辅助文字、标签 */
--text-sm:   14px;   /* 正文（紧凑） */
--text-base: 16px;   /* 正文（默认） */
--text-lg:   18px;   /* 小标题、强调正文 */
--text-xl:   20px;   /* 卡片标题 */
--text-2xl:  24px;   /* 区块标题 */
--text-3xl:  30px;   /* 大区块标题 */
--text-4xl:  36px;   /* 页面标题 */
--text-5xl:  48px;   /* Hero 主标题 */
```

### 行高与字重

```css
--leading-tight:   1.2;   /* 标题 */
--leading-normal:  1.5;   /* 正文 */
--leading-relaxed: 1.75;  /* 长文阅读 */

--font-normal:  400;
--font-medium:  500;
--font-semibold: 600;
--font-bold:    700;
```

### 字号使用规则

| 用途 | 字号 | 字重 | 行高 |
|------|------|------|------|
| Hero 主标题 | 48px | 700 | 1.1 |
| 页面标题 | 36px | 700 | 1.2 |
| 区块标题 | 24px | 600 | 1.3 |
| 卡片标题 | 20px | 600 | 1.3 |
| 正文 | 16px | 400 | 1.5 |
| 辅助文字 | 14px | 400 | 1.5 |
| 标签/注释 | 12px | 400 | 1.4 |

> 标题最大宽度限制：`max-width: 20ch`（中文约 12~15 字），避免一行太长影响阅读。

---

## 间距系统

### 间距阶梯（4px 基准）

```css
--space-0:   0;
--space-1:   4px;
--space-2:   8px;
--space-3:   12px;
--space-4:   16px;
--space-5:   20px;
--space-6:   24px;
--space-8:   32px;
--space-10:  40px;
--space-12:  48px;
--space-16:  64px;
--space-20:  80px;
--space-24:  96px;
--space-32:  128px;
```

### 间距使用场景

| 场景 | 推荐值 |
|------|--------|
| 页面水平内边距 | `--space-6`（24px），移动端 `--space-4`（16px） |
| 区块之间垂直间距 | `--space-16`（64px）~ `--space-24`（96px） |
| 卡片内边距 | `--space-6`（24px）~ `--space-8`（32px） |
| 卡片之间间距 | `--space-4`（16px）~ `--space-6`（24px） |
| 标题与正文间距 | `--space-3`（12px）~ `--space-4`（16px） |
| 按钮/输入框内边距 | 水平 `--space-4`，垂直 `--space-2` |
| 图标与文字间距 | `--space-2`（8px） |

### 密度乘数

```css
/* 紧凑模式（B 端、表格密集场景） */
--space-multiplier: 0.85;

/* 舒适模式（默认） */
--space-multiplier: 1;

/* 宽松模式（展示型、内容型） */
--space-multiplier: 1.2;
```

---

## 圆角系统

### 圆角阶梯

```css
--radius-none: 0;
--radius-sm:   4px;    /* 小元素：标签、小按钮 */
--radius-md:   8px;    /* 默认：按钮、输入框、卡片 */
--radius-lg:   12px;   /* 大卡片、模态框 */
--radius-xl:   16px;   /* 大型容器 */
--radius-full: 9999px; /* 胶囊、圆形头像 */
```

### 圆角风格（由预设决定）

| 风格 | sm | md | lg | 适用 |
|------|----|----|----|-----|
| 尖锐 | 4px | 6px | 8px | 技术、企业 |
| 适中 | 6px | 8px | 12px | 通用、SaaS |
| 柔和 | 8px | 12px | 16px | 消费、创意 |
| 直角 | 0 | 2px | 4px | 编辑、印刷 |

> 全项目只能用一档风格，不要混用。

---

## 阴影系统

### 阴影阶梯（浅色模式）

```css
--shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
--shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.06), 0 1px 2px -1px rgb(0 0 0 / 0.04);
--shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.07), 0 2px 4px -2px rgb(0 0 0 / 0.05);
--shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.08), 0 4px 6px -4px rgb(0 0 0 / 0.05);
```

### 阴影使用规则

- 普通卡片：`--shadow-xs` 或仅用边框
- 悬浮卡片（hover）：`--shadow-sm`
- 下拉菜单、浮层：`--shadow-md`
- 模态框：`--shadow-lg`
- **深色模式**：阴影透明度减半，或直接用 `--border-strong` 边框代替

### 阴影反模式

- ❌ 不要用彩色阴影（`box-shadow: 0 0 20px rgba(99,102,241,0.3)`）
- ❌ 不要多层阴影叠加营造"发光"效果
- ❌ 卡片不要同时有边框 + 大阴影，二选一

---

## 动效令牌

### 时长

```css
--duration-instant: 100ms;   /* 即时反馈：按钮按下 */
--duration-fast:    150ms;   /* 快速：hover、颜色变化 */
--duration-normal:  200ms;   /* 默认：展开、折叠 */
--duration-slow:    300ms;   /* 慢速：模态框、页面过渡 */
```

### 缓动函数

```css
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);     /* 出场、强调 */
--ease-in-out: cubic-bezier(0.65, 0, 0.35, 1); /* 对称过渡 */
--ease-default: var(--ease-out);
```

### 使用规则

- 默认过渡：`transition: all var(--duration-fast) var(--ease-default)`
- 仅过渡需要变化的属性（颜色、透明度、变换），不要 `transition: all` 影响性能
- 入场动画只用淡入（`opacity`）+ 轻微上移（`translateY` ≤ 8px）
- 禁用飞入、旋转、弹跳等夸张动画

---

## Tailwind 配置模板

将以下配置加入 `tailwind.config.js`，即可让 Tailwind 项目使用本令牌系统：

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      colors: {
        bg: 'var(--bg)',
        'bg-subtle': 'var(--bg-subtle)',
        'bg-muted': 'var(--bg-muted)',
        text: 'var(--text)',
        'text-secondary': 'var(--text-secondary)',
        'text-muted': 'var(--text-muted)',
        border: 'var(--border)',
        accent: {
          DEFAULT: 'var(--accent)',
          hover: 'var(--accent-hover)',
          foreground: 'var(--accent-foreground)',
        },
        success: 'var(--success)',
        warning: 'var(--warning)',
        danger: 'var(--danger)',
      },
      fontFamily: {
        sans: ['Inter', 'PingFang SC', 'Microsoft YaHei', 'sans-serif'],
        mono: ['JetBrains Mono', 'Consolas', 'monospace'],
      },
      fontSize: {
        xs: ['12px', { lineHeight: '1.4' }],
        sm: ['14px', { lineHeight: '1.5' }],
        base: ['16px', { lineHeight: '1.5' }],
        lg: ['18px', { lineHeight: '1.5' }],
        xl: ['20px', { lineHeight: '1.3' }],
        '2xl': ['24px', { lineHeight: '1.3' }],
        '3xl': ['30px', { lineHeight: '1.2' }],
        '4xl': ['36px', { lineHeight: '1.2' }],
        '5xl': ['48px', { lineHeight: '1.1' }],
      },
      spacing: {
        '1': '4px', '2': '8px', '3': '12px', '4': '16px',
        '5': '20px', '6': '24px', '8': '32px', '10': '40px',
        '12': '48px', '16': '64px', '20': '80px', '24': '96px',
        '32': '128px',
      },
      borderRadius: {
        sm: 'var(--radius-sm)',
        md: 'var(--radius-md)',
        lg: 'var(--radius-lg)',
      },
      boxShadow: {
        xs: 'var(--shadow-xs)',
        sm: 'var(--shadow-sm)',
        md: 'var(--shadow-md)',
        lg: 'var(--shadow-lg)',
      },
      transitionDuration: {
        fast: '150ms',
        normal: '200ms',
        slow: '300ms',
      },
      transitionTimingFunction: {
        default: 'var(--ease-default)',
      },
    },
  },
}
```

---

## CSS 变量初始化模板

在项目的全局 CSS 中引入：

```css
:root {
  /* === 在此处粘贴所选预设的颜色变量 === */

  /* 中性色（以 Zinc 为例） */
  --zinc-50: #fafafa; --zinc-100: #f4f4f5; --zinc-200: #e4e4e7;
  --zinc-300: #d4d4d8; --zinc-400: #a1a1aa; --zinc-500: #71717a;
  --zinc-600: #52525b; --zinc-700: #3f3f46; --zinc-800: #27272a;
  --zinc-900: #18181b; --zinc-950: #09090b;

  /* 语义映射 */
  --bg: var(--zinc-50);
  --bg-subtle: var(--zinc-100);
  --text: var(--zinc-950);
  --text-secondary: var(--zinc-600);
  --border: var(--zinc-200);
  --accent: var(--zinc-950);

  /* 状态色 */
  --success: #16a34a; --warning: #d97706; --danger: #dc2626;

  /* 圆角 */
  --radius-sm: 6px; --radius-md: 8px; --radius-lg: 12px;

  /* 阴影 */
  --shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-sm: 0 1px 3px 0 rgb(0 0 0 / 0.06);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.07);
}
```
