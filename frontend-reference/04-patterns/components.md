# 场景范式：通用组件（Tabs / 面包屑 / 下拉 / Badge / Avatar / Tooltip / 分页）

> 零散但高频的小组件，统一规范避免"每个页面各做各的"。

---

## 1. Tabs 标签页

**适合**：在同一区域切换不同内容视图。

```
┌─────────────────────────────────────┐
│  全部  进行中  已完成  已归档          │
│  ────                                │
│                                     │
│  当前 Tab 的内容...                    │
└─────────────────────────────────────┘
```

**要点**：
- Tab 项高度 `40px`，左右 padding `16px`
- 选中态：底部 `2px` 强调色线，文字 `--accent` 字重 500
- 未选中：文字 `--text-secondary`，hover 变 `--text`
- 不要用背景色块区分选中态
- Tab 之间无分割线

```tsx
<div className="border-b border-border">
  <nav className="flex gap-1">
    {tabs.map((tab) => (
      <button
        key={tab.key}
        className={`h-10 px-4 text-sm border-b-2 transition-colors
          ${tab.active
            ? 'border-accent text-accent font-medium'
            : 'border-transparent text-text-secondary hover:text-text'}`}
      >
        {tab.label}
        {tab.count != null && (
          <span className="ml-1.5 text-xs text-text-muted">{tab.count}</span>
        )}
      </button>
    ))}
  </nav>
</div>
```

---

## 2. 面包屑

**适合**：多层级页面，显示当前位置。

```
首页 / 项目 / 项目详情 / 设置
```

**要点**：
- 字号 `14px`，颜色 `--text-secondary`
- 分隔符用 `/`，颜色 `--text-muted`，左右间距 `8px`
- 当前页：`--text` 色，不可点击
- 其余项：hover 变 `--accent`
- 高度 `32px`，放在页面标题上方

```tsx
<nav className="flex items-center gap-2 text-sm text-text-secondary">
  {items.map((item, i) => (
    <span key={i} className="flex items-center gap-2">
      {i > 0 && <span className="text-text-muted">/</span>}
      {i === items.length - 1
        ? <span className="text-text">{item.label}</span>
        : <a href={item.href} className="hover:text-accent">{item.label}</a>
      }
    </span>
  ))}
</nav>
```

---

## 3. 下拉菜单

**适合**：用户头像菜单、更多操作、筛选器。

```
┌──────────────────────┐
│  个人资料              │
│  账户设置              │
│  ──────────          │
│  退出登录              │
└──────────────────────┘
```

**要点**：
- 宽度：根据内容，`160px ~ 240px`
- 圆角：`--radius-md`
- 边框：`1px solid var(--border)` + `--shadow-md`
- 菜单项高度 `36px`，左右 padding `12px`
- hover：背景 `--bg-muted`
- 分割线：`1px solid var(--border)`，上下 `4px` 间距
- 图标（如有）：`16px`，颜色 `--text-secondary`，与文字间距 `8px`

```tsx
<div className="w-48 rounded-md border border-border bg-bg py-1 shadow-md">
  {items.map((item, i) => (
    item.divider
      ? <div key={i} className="my-1 h-px bg-border" />
      : <a key={i} href={item.href} className="flex items-center gap-2 h-9 px-3 text-sm text-text-secondary hover:bg-bg-muted hover:text-text">
          {item.icon && <item.icon size={16} />}
          {item.label}
        </a>
  ))}
</div>
```

---

## 4. Badge / Tag 标签

**适合**：状态标记、分类、数量提示。

**状态 Badge**：
```
[ 已启用 ]  [ 待审核 ]  [ 已过期 ]
```
- 高度 `22px`，padding `0 8px`
- 圆角 `--radius-full`
- 背景用浅色（`success-bg`/`warning-bg`/`danger-bg`），文字用深色（`success`/`warning`/`danger`）
- 字号 `12px`

```tsx
<span className="inline-flex items-center h-[22px] px-2 rounded-full text-xs bg-success-bg text-success">
  已启用
</span>
```

**数量 Badge**（红点/数字）：
- 未读红点：`8px` 圆点，`--danger` 色
- 数字角标：最小宽 `16px`，高 `16px`，圆角 `--radius-full`，`--danger` 背景白字

---

## 5. Avatar 头像

**适合**：用户头像、团队成员列表。

**要点**：
- 尺寸：`24px`（小）、`32px`（默认）、`40px`（大）、`64px`（资料页）
- 圆角 `--radius-full`
- 无图时用首字母，背景 `--bg-muted`，文字 `--text-secondary`
- 在线状态点：右下角，`8px`，`--success` 色，带 `2px` 白色边框

```tsx
<div className="relative">
  <div className="w-8 h-8 rounded-full bg-bg-muted flex items-center justify-center text-sm font-medium text-text-secondary">
    A
  </div>
  <span className="absolute bottom-0 right-0 w-2 h-2 rounded-full bg-success border-2 border-bg" />
</div>
```

**头像组**（多人）：
- 第 2 个起向左偏移 `-8px`
- 每个带 `2px` 白色边框
- 超过 N 个显示 `+N`

---

## 6. Tooltip 提示

**适合**：图标按钮、缩写词的解释。

**要点**：
- 触发方式：hover 或 focus
- 背景：`--text`（深色），文字：`--bg`（浅色）
- 圆角 `--radius-sm`，padding `4px 8px`
- 字号 `12px`
- 与目标元素间距 `6px`
- 显示延迟 `300ms`，隐藏延迟 `100ms`
- 不要用 Tooltip 承载重要信息（只做补充说明）

---

## 7. 分页

**适合**：数据列表，数据量较大时。

```
←  1  2  3  4  5  →    共 128 条
```

**要点**：
- 页码按钮：`32px × 32px`，圆角 `--radius-sm`
- 当前页：`--accent` 背景，`--accent-foreground` 文字
- 非当前页：hover 背景 `--bg-muted`
- 省略号 `...`：不可点击
- 左右箭头按钮：禁用时 `--text-muted`
- 右侧显示总数（`14px`，`--text-secondary`）

```tsx
<div className="flex items-center gap-1">
  <button className="w-8 h-8 rounded-sm text-text-muted hover:text-text" disabled>←</button>
  <button className="w-8 h-8 rounded-sm bg-accent text-accent-foreground text-sm">1</button>
  <button className="w-8 h-8 rounded-sm hover:bg-bg-muted text-sm">2</button>
  <button className="w-8 h-8 rounded-sm hover:bg-bg-muted text-sm">3</button>
  <span className="px-1 text-text-muted">...</span>
  <button className="w-8 h-8 rounded-sm hover:bg-bg-muted text-sm">10</button>
  <button className="w-8 h-8 rounded-sm text-text-secondary hover:text-text">→</button>
  <span className="ml-4 text-sm text-text-secondary">共 128 条</span>
</div>
```

---

## 组件通用规则

1. **尺寸统一**：同一类组件在全站用相同尺寸，不要这个页面 32px 那个页面 40px
2. **状态一致**：hover/active/disabled 态用统一的颜色和交互方式
3. **间距用阶梯值**：所有 padding/margin 来自间距阶梯
4. **圆角用变量**：不用硬编码，统一 `--radius-sm/md/lg/full`
