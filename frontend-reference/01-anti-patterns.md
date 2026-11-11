# 01 - AI 味反面清单（详细版）

> 本文档是 `FRONTEND_STANDARDS.md` 第二节的详细展开。
> 每个反模式都配有**错误示例**和**正确做法**，以及背后的设计逻辑。
> 记住：避免这些错误，比追求"好看"更重要。

---

## 目录

1. [配色类反模式](#配色类反模式)
2. [布局类反模式](#布局类反模式)
3. [组件类反模式](#组件类反模式)
4. [动效类反模式](#动效类反模式)
5. [内容类反模式](#内容类反模式)

---

## 配色类反模式

### 反模式 1：紫蓝渐变背景

**为什么土**：这是 AI 生成 UI 时概率最高的配色，已经形成强烈的"AI 感"条件反射。看到紫蓝渐变 = 廉价模板。

**错误示例**：
```html
<!-- ❌ 紫蓝渐变背景 -->
<div style="background: linear-gradient(135deg, #6366f1, #8b5cf6);">
```

**正确做法**：
- 纯色背景 + 单一强调色
- 如果一定要用渐变，只在极小区域（如按钮 hover、标签）且用同色系深浅过渡，不要跨色相

```css
/* ✅ 纯色背景，强调色用在按钮和链接上 */
:root {
  --bg: #ffffff;
  --accent: #09090b;
}
.hero { background: var(--bg); }
.btn-primary { background: var(--accent); color: #fff; }
```

---

### 反模式 2：彩色图标满天飞

**为什么土**：每个图标一个颜色，页面变成调色板，视觉焦点涣散，像 2010 年的网页。

**错误示例**：
```html
<!-- ❌ 每个图标不同颜色 -->
<Icon name="users" color="#3b82f6" />
<Icon name="chart" color="#10b981" />
<Icon name="bell" color="#f59e0b" />
<Icon name="settings" color="#ef4444" />
```

**正确做法**：
- 图标默认统一为灰色（`--text-secondary`）
- 仅在 hover、选中、激活状态才变色，且只变强调色

```css
/* ✅ 图标统一灰色，hover 变强调色 */
.icon { color: var(--text-secondary); }
.icon:hover, .icon.active { color: var(--accent); }
```

---

### 反模式 3：渐变文字滥用

**为什么土**：`bg-clip-text text-transparent` 配渐变，AI 最爱用来做标题。偶尔用在品牌名上可以，满屏用就廉价。

**错误示例**：
```html
<!-- ❌ 每个标题都渐变文字 -->
<h1 class="bg-gradient-to-r from-purple-500 to-pink-500 bg-clip-text text-transparent">
  产品特性
</h1>
```

**正确做法**：
- 渐变文字仅用于**品牌名或 Hero 主标语**，全页最多 1 处
- 其余标题用纯色，靠字号和字重体现层级

```html
<!-- ✅ 只有品牌名用渐变，其余纯色 -->
<h1 class="text-5xl font-bold text-zinc-900">
  打造<span class="bg-gradient-to-r from-amber-500 to-orange-600 bg-clip-text text-transparent">极致</span>体验
</h1>
```

---

### 反模式 4：满屏状态色当装饰色

**为什么土**：成功绿、警告橙、危险红是**功能性颜色**，只用于状态反馈。把它们当装饰色用，会让用户误以为有状态提示。

**错误示例**：
```html
<!-- ❌ 用绿色做卡片背景装饰 -->
<div class="bg-green-100 text-green-800 p-4">特性介绍卡片</div>
```

**正确做法**：
- 状态色只用在：按钮状态、表单校验、Badge 标签、通知提示
- 装饰性背景用中性色（灰阶的 subtle/muted）

---

## 布局类反模式

### 反模式 5：Hero 区千篇一律

**为什么土**：AI 生成的落地页 Hero 区几乎都是同一个公式：
```
[大标题] [副标题] [主按钮 + 次按钮] [渐变背景 + 装饰图形]
```
毫无记忆点。

**正确做法**：根据产品特点选择不同结构：

1. **左文右图**（适合有产品截图的工具）：
```
┌─────────────────────┐
│ 标题    │           │
│ 副标题  │  产品截图  │
│ 按钮    │           │
└─────────────────────┘
```

2. **全宽图 + 叠加文字**（适合品牌/展示型）：
```
┌─────────────────────┐
│                     │
│   [背景图/视频]      │
│                     │
│   标题 + 按钮        │
└─────────────────────┘
```

3. **居中纯文字**（适合极简产品）：
```
┌─────────────────────┐
│                     │
│      大标题         │
│      副标题         │
│      [按钮]         │
│                     │
└─────────────────────┘
```

4. **不对称布局**（适合创意产品）：标题偏左，视觉元素偏右下。

> 核心：不要每次都用居中 + 两个按钮。根据内容选择布局。

---

### 反模式 6：过度留白或留白不足

**为什么土**：
- 留白不足：内容挤在一起，像 PPT 塞满字
- 过度留白：页面空荡荡，像没做完

**正确做法**：
- 区块之间：`64px ~ 96px`
- 卡片内边距：`24px ~ 32px`
- 元素组之间：`16px ~ 24px`
- 元素内部：`8px ~ 12px`

用间距阶梯里的值，不要随手写。

---

### 反模式 7：到处用分割线

**为什么土**：能用留白区分的地方非要画一条线，显得页面"碎"。

**正确做法**：
- 优先用**留白**区分区块
- 分割线只用于：导航栏底、卡片边框、表格内行分隔
- 用浅灰色（`--border`），不要用深色

---

## 组件类反模式

### 反模式 8：每个按钮都是主按钮

**为什么土**：一个页面上 3 个实心彩色按钮，用户不知道点哪个，视觉噪音大。

**正确做法**：
- 一个页面/区域最多 **1 个主按钮**（实心填充）
- 其余操作用**次按钮**（描边）或**幽灵按钮**（仅文字，hover 有底色）

```html
<!-- ❌ 三个主按钮 -->
<button class="bg-blue-600 text-white">创建</button>
<button class="bg-blue-600 text-white">导入</button>
<button class="bg-blue-600 text-white">删除</button>

<!-- ✅ 一个主按钮 + 次按钮 + 危险幽灵按钮 -->
<button class="bg-accent text-white">创建项目</button>
<button class="border border-border text-text">导入</button>
<button class="text-danger hover:bg-danger/10">删除</button>
```

---

### 反模式 9：圆角 + 大阴影同时出现

**为什么土**：大圆角（`rounded-2xl`/`3xl`）加大阴影，组合起来像儿童产品或玩具 UI。

**正确做法**：
- 圆角适中（`md`/`lg`）+ 淡阴影 → 现代专业
- 大圆角只能配**极淡阴影或无阴影**，靠边框定义边界

```css
/* ❌ 大圆角 + 大阴影 */
.card { border-radius: 24px; box-shadow: 0 20px 40px rgba(0,0,0,0.15); }

/* ✅ 适中圆角 + 淡阴影 */
.card { border-radius: 8px; box-shadow: 0 1px 2px rgba(0,0,0,0.05); border: 1px solid var(--border); }
```

---

### 反模式 10：用 Emoji 当图标

**为什么土**：Emoji 风格不统一（各系统渲染不同）、色彩杂乱、显得业余。

**正确做法**：用矢量图标库：
- **Lucide Icons**（推荐，现代简洁，开源）
- **Phosphor Icons**（6 种粗细，风格统一）
- **Heroicons**（Tailwind 出品，2 种尺寸）

```html
<!-- ❌ Emoji -->
<button>🚀 开始使用</button>

<!-- ✅ 矢量图标 -->
<button><LucideIcon name="rocket" /> 开始使用</button>
```

---

### 反模式 11：输入框标签放左边

**为什么土**：标签在左边会让表单左右不齐，移动端更难适配，是老派后台的典型特征。

**正确做法**：
- 标签放在输入框**上方**
- 标签字号 `13px`，字重 500，和输入框间距 `8px`

```html
<!-- ❌ 标签在左 -->
<div>
  <label>邮箱</label>
  <input type="email">
</div>

<!-- ✅ 标签在上 -->
<div class="flex flex-col gap-2">
  <label class="text-sm font-medium">邮箱地址</label>
  <input type="email" class="h-10 rounded-sm border px-3">
</div>
```

---

### 反模式 12：卡片叠卡片 + 层层阴影

**为什么土**：卡片里面套卡片，每层都有阴影，像千层饼，视觉层级混乱。

**正确做法**：
- 卡片最多嵌套 1 层
- 外层卡片用边框，内层用背景色区分，不要都用阴影
- 或者直接不用嵌套卡片，用留白分组

---

## 动效类反模式

### 反模式 13：飞入飞出动画

**为什么土**：元素从左边飞入、从下边滑入，是 2015 年的 PPT 风格。AI 特别爱加这些。

**正确做法**：
- 动画只用 `transition`（状态过渡：hover、展开、颜色变化）
- 入场动画：仅用淡入（`opacity`），不要位移
- 时长 `150ms ~ 250ms`，缓动用 `ease-out`

```css
/* ❌ 飞入动画 */
@keyframes slideInLeft {
  from { transform: translateX(-100px); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

/* ✅ 淡入 + 微上移 */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
.element { animation: fadeUp 0.4s ease-out; }
```

---

### 反模式 14：毛玻璃滥用

**为什么土**：`backdrop-blur` 在 AI 输出中几乎必现，但大部分场景不需要。滥用导致可读性下降、性能变差。

**正确做法**：
- 毛玻璃仅用于：**固定导航栏**（滚动时内容透出）、**模态浮层**
- 全页最多 1~2 处
- 配合半透明背景色（`bg-white/70`），不要只 blur 不加底色

```css
/* ❌ 所有卡片都毛玻璃 */
.card { backdrop-filter: blur(12px); background: rgba(255,255,255,0.1); }

/* ✅ 只在导航栏用 */
.navbar { 
  backdrop-filter: blur(8px); 
  background: rgba(255,255,255,0.75);
  border-bottom: 1px solid var(--border);
}
```

---

## 内容类反模式

### 反模式 15：Lorem Ipsum 占位文案上线

**为什么土**：用假文案交付，显得不专业，也无法判断真实排版效果。

**正确做法**：用真实或拟真的文案。如果暂时没有，用简短的中文占位（如"此处为产品描述"），不要用 Lorem Ipsum。

---

### 反模式 16：用颜色区分信息层级

**为什么土**：红色标题 > 黑色正文这种做法，是新手典型错误。颜色不是层级工具。

**正确做法**：层级靠以下手段（优先级从高到低）：
1. 字号大小
2. 字重（粗细）
3. 颜色**深浅**（不是色相）
4. 间距

```html
<!-- ❌ 靠颜色区分层级 -->
<h2 style="color: red;">重要标题</h2>
<p style="color: black;">正文内容</p>

<!-- ✅ 靠字号字重区分 -->
<h2 class="text-2xl font-semibold text-text">重要标题</h2>
<p class="text-base text-text-secondary">正文内容</p>
```

---

## 自检流程

每次写完一个页面，按以下顺序过一遍：

1. **配色**：页面上彩色是否 ≤ 3 种（强调色 + 状态色）？
2. **图标**：是否统一一套？有没有 emoji？颜色是否统一？
3. **按钮**：主按钮是否只有 1 个？
4. **圆角阴影**：圆角是否统一？有没有大圆角+大阴影？
5. **动画**：有没有飞入飞出？毛玻璃是否超过 2 处？
6. **间距**：所有间距是否来自阶梯？有没有随手写的值？
7. **层级**：是否靠字号字重区分，而不是颜色？
8. **文案**：有没有 Lorem Ipsum？
