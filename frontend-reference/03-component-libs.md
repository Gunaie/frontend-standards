# 03 - 精选组件库与去主题化指南

> 组件库是效率工具，但**默认主题 = 土味来源**。
> 本文档推荐经过筛选的组件库，并说明如何自定义主题，避免"一眼看穿用了什么库"。

---

## 目录

1. [组件库推荐](#组件库推荐)
2. [去默认主题的通用方法](#去默认主题的通用方法)
3. [各库定制指南](#各库定制指南)
4. [图标库](#图标库)

---

## 组件库推荐

### 第一梯队（强烈推荐）

#### shadcn/ui
- **技术栈**：React + Radix UI + Tailwind CSS
- **特点**：组件源码直接复制到项目中，不是 npm 包，完全可控
- **优点**：默认样式已经很克制，极易定制，无运行时依赖
- **缺点**：需要自己维护组件代码，升级靠手动
- **适合**：所有 React 项目，尤其是需要高度定制的
- **官网**：ui.shadcn.com

#### Radix UI
- **技术栈**：React，无样式（Headless）
- **特点**：只提供行为和可访问性，样式完全自己写
- **优点**：零默认样式，100% 自定义，无障碍支持好
- **缺点**：需要自己写所有样式，开发量大
- **适合**：对视觉要求高、有设计资源的团队
- **官网**：radix-ui.com

### 第二梯队（可用，必须定制主题）

#### Ant Design (antd)
- **技术栈**：React
- **特点**：组件最全，中文文档好，B 端生态成熟
- **缺点**：默认蓝色主题辨识度太高，不改主题就很土
- **适合**：B 端后台、企业系统（配合预设 D）
- **定制要点**：必须改 `primaryColor`、`borderRadius`，建议覆盖 `theme.token`

#### MUI (Material UI)
- **技术栈**：React
- **特点**：组件丰富，文档好
- **缺点**：Material Design 风格烙印重，默认紫色
- **适合**：需要快速搭建的项目
- **定制要点**：必须改 `palette.primary` 和 `shape.borderRadius`

#### Element Plus
- **技术栈**：Vue 3
- **特点**：Vue 生态首选，中文友好
- **缺点**：默认蓝色，组件偏"方块感"
- **适合**：Vue 3 后台项目
- **定制要点**：覆盖 CSS 变量 `--el-color-primary`、`--el-border-radius-base`

### 第三梯队（特定场景）

#### Chakra UI
- **特点**：样式 props 写在组件上，开发快
- **适合**：原型、内部工具
- **注意**：默认样式偏"圆润可爱"，需要调整

#### Headless UI
- **特点**：Tailwind 出品，无样式组件
- **适合**：Tailwind 项目，只需要几个核心组件时

---

## 去默认主题的通用方法

### 核心原则

> **组件库只提供行为，不提供视觉。** 视觉必须来自你自己的设计令牌。

### 三步去主题化

#### 第一步：替换主题色

所有组件库都有主题色配置，必须改成你项目的 `--accent`：

```js
// 错误：使用默认蓝色
// 正确：使用项目强调色
primaryColor: 'var(--accent)'  // 或具体色值
```

#### 第二步：统一圆角

默认圆角通常偏大或偏小，统一成你预设的圆角尺度：

```js
borderRadius: {
  sm: 6,   // 对应 --radius-sm
  md: 8,   // 对应 --radius-md
  lg: 12,  // 对应 --radius-lg
}
```

#### 第三步：覆盖字号和字重

组件库默认字号往往偏大，改成你的字号阶梯：

```js
fontSize: 14,
fontFamily: 'Inter, PingFang SC, sans-serif',
fontWeight: {
  regular: 400,
  medium: 500,
  semibold: 600,
}
```

### 额外技巧

- **边框优先于阴影**：把组件的默认阴影关掉，改用边框
- **去掉多余装饰**：很多组件库默认有 hover 底色、focus 光环，按需保留
- **禁用动画**：组件库默认动画可能和你的动效令牌冲突，统一关掉或替换

---

## 各库定制指南

### shadcn/ui 定制

shadcn/ui 的组件在 `components/ui/` 目录下，直接改每个组件的 className：

```tsx
// components/ui/button.tsx
function buttonVariants() {
  return cva(
    "inline-flex items-center justify-center rounded-md font-medium transition-colors focus-visible:outline-none disabled:opacity-50",
    {
      variants: {
        variant: {
          default: "bg-accent text-accent-foreground hover:bg-accent-hover",
          outline: "border border-border bg-transparent hover:bg-muted",
          ghost: "hover:bg-muted",
        },
        size: {
          default: "h-10 px-4 py-2",
          sm: "h-9 px-3 text-sm",
          lg: "h-11 px-8",
        },
      },
    }
  )
}
```

> shadcn/ui 默认就用了 CSS 变量（`bg-accent`、`text-accent-foreground`），所以只要在全局 CSS 定义好 `--accent` 等变量，主题自动适配。

### Ant Design 定制

在 `App.tsx` 或入口文件用 `ConfigProvider`：

```tsx
import { ConfigProvider } from 'antd'

<ConfigProvider
  theme={{
    token: {
      colorPrimary: '#09090b',        // 改成你的强调色
      borderRadius: 8,                 // 统一圆角
      fontFamily: 'Inter, PingFang SC, sans-serif',
      fontSize: 14,
      colorBgContainer: '#ffffff',
      colorBorder: '#e4e4e7',
    },
    components: {
      Button: {
        borderRadius: 8,
        controlHeight: 40,
        fontWeight: 500,
      },
      Card: {
        borderRadius: 8,
        boxShadowTertiary: 'none',     // 关掉默认阴影
      },
    },
  }}
>
  <App />
</ConfigProvider>
```

### MUI 定制

```tsx
import { createTheme, ThemeProvider } from '@mui/material'

const theme = createTheme({
  palette: {
    primary: { main: '#09090b' },
    background: { default: '#fafafa', paper: '#ffffff' },
    text: { primary: '#09090b', secondary: '#52525b' },
    divider: '#e4e4e7',
  },
  shape: { borderRadius: 8 },
  typography: {
    fontFamily: 'Inter, PingFang SC, sans-serif',
    fontSize: 14,
    button: { textTransform: 'none' },  // 去掉默认大写
  },
  components: {
    MuiButton: {
      styleOverrides: {
        root: { borderRadius: 8, boxShadow: 'none' },
      },
    },
    MuiCard: {
      styleOverrides: {
        root: { boxShadow: 'none', border: '1px solid #e4e4e7' },
      },
    },
  },
})
```

### Element Plus 定制

```ts
import ElementPlus from 'element-plus'

app.use(ElementPlus, {
  // 通过 CSS 变量覆盖
})
```

在全局 CSS 中覆盖：

```css
:root {
  --el-color-primary: #09090b;
  --el-color-primary-light-3: #3f3f46;
  --el-color-primary-light-5: #71717a;
  --el-color-primary-light-7: #a1a1aa;
  --el-color-primary-light-8: #d4d4d8;
  --el-color-primary-light-9: #f4f4f5;
  --el-border-radius-base: 8px;
  --el-font-family: 'Inter', 'PingFang SC', sans-serif;
}
```

---

## 图标库

### 推荐

#### Lucide Icons（首选）
- **风格**：现代、简洁、2px 描边
- **数量**：1000+ 图标
- **特点**：开源、树摇友好、与 shadcn/ui 配套
- **使用**：`npm install lucide-react`

```tsx
import { Rocket, Settings, Bell } from 'lucide-react'

<Rocket size={20} />
```

#### Phosphor Icons
- **风格**：6 种粗细（thin/light/regular/bold/fill/duotone）
- **特点**：风格统一、覆盖面广
- **适合**：需要不同粗细图标的场景

#### Heroicons
- **风格**：2 种尺寸（24px solid / 24px outline）
- **特点**：Tailwind 出品，与 Tailwind 配合好

### 使用规范

- 全项目**只用一套**图标库
- 统一尺寸：正文 `16px`，导航 `20px`
- 默认颜色 `--text-secondary`，hover/active 用 `--accent`
- 图标与文字间距 `8px`
- **禁止**用 emoji 当图标

---

## 选型决策树

```
需要高度定制？
├─ 是 → shadcn/ui（React）或 Radix UI
└─ 否
   ├─ React B 端 → Ant Design（必须改主题）
   ├─ React 通用 → shadcn/ui
   ├─ Vue → Element Plus（必须改主题）
   └─ 快速原型 → Chakra UI
```

> **记住**：选哪个库不重要，重要的是**必须定制主题**。用默认主题的项目，不管用什么库都会土。
