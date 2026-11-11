# 场景范式：状态页（空 / 加载 / 错误）

> 状态页是用户体验的"兜底"，做不好会显得产品粗糙。
> 核心原则：**简洁、有引导、不吓人**。

---

## 反模式提醒

❌ **避免**：
- 空状态放巨大的插画或 emoji
- 加载态用满屏旋转的大菊花（spinner）
- 错误页用红色大背景 + 感叹号
- 所有状态都只有一行文字，没有引导操作
- 骨架屏用彩色块

---

## 范式 1：空状态

**适合**：列表无数据、搜索无结果、用户未创建内容。

```
┌─────────────────────────────┐
│                             │
│        [图标]                │
│                             │
│      还没有任何内容           │
│   创建你的第一个项目吧         │
│                             │
│      [ 创建项目 ]            │
│                             │
└─────────────────────────────┘
```

**要点**：
- 图标：单色线性图标，`48px ~ 64px`，颜色 `--text-muted`
- 标题：`18px`，字重 600，颜色 `--text`
- 描述：`14px`，颜色 `--text-secondary`，一句话说明原因
- 操作按钮：主按钮，引导用户下一步
- 整体居中，上下留白 `80px+`

**参考实现**：
```tsx
<div className="flex flex-col items-center justify-center py-20 text-center">
  <InboxIcon size={48} className="text-text-muted" />
  <h3 className="mt-4 text-lg font-semibold">还没有任何内容</h3>
  <p className="mt-2 text-sm text-text-secondary">创建你的第一个项目，开始使用吧</p>
  <button className="mt-6 h-10 px-5 rounded-sm bg-accent text-accent-foreground text-sm font-medium">
    创建项目
  </button>
</div>
```

### 空状态变体

| 场景 | 图标 | 文案建议 |
|------|------|---------|
| 列表为空 | Inbox | "还没有数据" + 创建按钮 |
| 搜索无结果 | Search | "没有找到相关结果" + 建议换关键词 |
| 无权限 | Lock | "你没有访问权限" + 联系管理员 |
| 网络断开 | WifiOff | "网络连接失败" + 重试按钮 |

---

## 范式 2：加载态

**适合**：数据请求中、页面切换、提交中。

### 2a. 骨架屏（推荐，用于列表/卡片）

```
┌─────────────────────────────┐
│  ████  ████████████          │
│  ████  ██████████            │
│                             │
│  ┌──────┐ ┌──────┐ ┌──────┐ │
│  │░░░░░░│ │░░░░░░│ │░░░░░░│ │
│  │░░░░░░│ │░░░░░░│ │░░░░░░│ │
│  └──────┘ └──────┘ └──────┘ │
└─────────────────────────────┘
```

**要点**：
- 骨架块用 `--bg-muted`，不要用彩色
- 形状模仿真实内容布局，不要用通用矩形
- 动画：`pulse`（淡入淡出），不要用左右滑动的 shimmer
- 高度与真实内容一致，避免加载完成后布局跳动

```tsx
{/* 骨架屏卡片 */}
<div className="rounded-lg border border-border p-6">
  <div className="h-5 w-24 bg-bg-muted rounded animate-pulse" />
  <div className="mt-4 h-8 w-32 bg-bg-muted rounded animate-pulse" />
  <div className="mt-6 h-3 w-full bg-bg-muted rounded animate-pulse" />
  <div className="mt-2 h-3 w-2/3 bg-bg-muted rounded animate-pulse" />
</div>
```

### 2b. 局部 Spinner（用于按钮、小区域）

```
[  加载中...  ]
```

**要点**：
- 尺寸：`16px`（按钮内）、`24px`（小区域）、`32px`（大区域）
- 颜色：`--text-muted` 或 `--accent`
- 配合文字"加载中..."，不要只转圈
- 按钮加载时禁用，宽度不变（防跳动）

```tsx
<button disabled className="h-10 px-5 rounded-sm bg-accent/70 text-accent-foreground text-sm font-medium">
  <Spinner size={16} className="mr-2 animate-spin" />
  提交中...
</button>
```

### 2c. 全屏加载（仅用于页面级）

```
┌─────────────────────────────┐
│                             │
│                             │
│           ⟳                 │
│        加载中...             │
│                             │
│                             │
└─────────────────────────────┘
```

**要点**：
- 仅在整页数据加载时使用，不要用于局部
- Spinner `32px`，颜色 `--text-muted`
- 背景半透明 `--bg/80`，不要用纯黑遮罩

---

## 范式 3：错误态

**适合**：请求失败、权限不足、系统异常。

### 3a. 内联错误（表单字段）

```
邮箱地址
[____________]
请输入有效的邮箱地址
```

**要点**：
- 位置：输入框下方 `8px`
- 颜色：`--danger`
- 字号：`12px ~ 13px`
- 输入框边框变红 `--danger`
- 可加警告图标（`14px`）

```tsx
<div className="space-y-1.5">
  <label className="text-sm font-medium">邮箱地址</label>
  <input className={`w-full h-10 rounded-sm border px-3 ${error ? 'border-danger' : 'border-border'}`} />
  {error && (
    <p className="text-xs text-danger flex items-center gap-1">
      <AlertCircle size={14} /> 请输入有效的邮箱地址
    </p>
  )}
</div>
```

### 3b. 页面级错误（404 / 500）

```
┌─────────────────────────────┐
│                             │
│          404                 │
│                             │
│      页面走丢了               │
│   你访问的页面不存在或已移动    │
│                             │
│      [ 返回首页 ]            │
│                             │
└─────────────────────────────┘
```

**要点**：
- 错误码（404/500）字号 `72px+`，字重 700，颜色 `--text`（不要红色）
- 标题：`24px`，描述：`14px` `--text-secondary`
- 按钮：返回首页或上一页
- 不要用红色大背景，保持克制

```tsx
<div className="flex flex-col items-center justify-center min-h-[60vh] text-center">
  <h1 className="text-7xl font-bold tracking-tight">404</h1>
  <h2 className="mt-4 text-2xl font-semibold">页面走丢了</h2>
  <p className="mt-2 text-text-secondary">你访问的页面不存在或已被移动</p>
  <button className="mt-8 h-10 px-6 rounded-sm bg-accent text-accent-foreground text-sm font-medium">
    返回首页
  </button>
</div>
```

### 3c. 请求失败提示条

```
┌─────────────────────────────┐
│ ⚠ 加载失败，请稍后重试  [重试] │
└─────────────────────────────┘
```

**要点**：
- 位置：内容区顶部
- 背景：`--danger-bg`（浅红），不要纯红
- 文字：`--danger`（深红）
- 右侧放"重试"按钮
- 可手动关闭

---

## 状态页通用规则

1. **颜色**：错误用 `--danger`，加载用 `--text-muted`，空状态用 `--text-muted`。不要用红色大背景吓人。
2. **图标**：统一线性图标，单色，不要用 emoji。
3. **引导**：每个状态都要有下一步操作（创建、重试、返回），不要只展示文字。
4. **动画**：骨架屏用 `pulse`，spinner 用 `spin`，时长 `1s ~ 1.5s`，缓动 `linear`。
5. **布局**：居中对齐，上下留白充足（`80px+`），不要挤在顶部。
6. **文案**：简洁友好，不说教。"加载中"比"正在努力加载数据中"好。
