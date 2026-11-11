# 场景范式：导航栏

> 导航栏是全站出现频率最高的组件，也是最容易"丑"和"反复返工"的地方。
> 核心原则：**高度固定、间距统一、状态清晰、不堆砌装饰**。

---

## 反模式提醒

❌ **避免**：
- 导航栏高度随意变化（首页 80px，内页 64px）
- Logo + 菜单 + 搜索 + 按钮 + 通知 + 头像 全挤在一行
- 菜单项 hover 用彩色背景块
- 选中态用底部彩色粗线（3px+）
- 滚动时导航栏没有任何变化（要么一直透明要么一直实色）
- 移动端汉堡菜单点开后是全屏黑色遮罩 + 巨大文字

---

## 范式 1：顶部导航栏（落地页/营销站）

**适合**：官网、落地页、内容站。

```
┌──────────────────────────────────────────────┐
│  Logo    菜单1  菜单2  菜单3        按钮  头像  │
└──────────────────────────────────────────────┘
```

**尺寸规范**：
- 高度：`64px`（桌面）、`56px`（移动端）
- 左右内边距：`24px`
- Logo 与菜单间距：`32px`
- 菜单项之间间距：`8px`（padding 左右各 `12px`）
- 右侧操作区：按钮 + 头像，间距 `12px`

**状态**：
- 默认：透明或实色背景（取决于 Hero 是否有图）
- 滚动后：实色背景 + 底边框（`1px solid var(--border)`），可加 `backdrop-blur(8px)`
- 菜单项 hover：文字颜色变 `--accent`，不加背景
- 选中菜单项：文字 `--accent` + 字重 500，底部 `2px` 强调色线（可选）

**参考实现**：
```tsx
<header className="sticky top-0 z-50 border-b border-border bg-bg/80 backdrop-blur">
  <div className="container mx-auto px-6 h-16 flex items-center justify-between">
    {/* Logo */}
    <a href="/" className="flex items-center gap-2">
      <Logo className="w-8 h-8" />
      <span className="font-bold text-lg">Brand</span>
    </a>

    {/* 桌面菜单 */}
    <nav className="hidden md:flex items-center gap-1">
      {menuItems.map((item) => (
        <a
          key={item.key}
          href={item.href}
          className="px-3 h-9 flex items-center rounded-sm text-sm text-text-secondary hover:text-accent transition-colors"
        >
          {item.label}
        </a>
      ))}
    </nav>

    {/* 右侧操作 */}
    <div className="flex items-center gap-3">
      <button className="h-9 px-4 rounded-sm text-sm font-medium text-text-secondary hover:text-text">
        登录
      </button>
      <button className="h-9 px-4 rounded-sm bg-accent text-accent-foreground text-sm font-medium">
        免费开始
      </button>
    </div>
  </div>
</header>
```

---

## 范式 2：侧边导航栏（后台/应用）

**适合**：Dashboard、管理后台、SaaS 应用内。

```
┌────┬─────────────────────────────────────────┐
│    │  顶部栏（搜索、通知、用户）                 │
│ 导 ├─────────────────────────────────────────┤
│ 航 │                                         │
│ 栏 │            主内容区                       │
│    │                                         │
└────┴─────────────────────────────────────────┘
```

**尺寸规范**：
- 宽度：`240px`（展开）、`64px`（折叠，仅图标）
- 菜单项高度：`40px`
- 菜单项左右内边距：`16px`
- 图标尺寸：`18px`，与文字间距 `12px`
- 分组标题：`12px`，颜色 `--text-muted`，上下间距 `16px`

**状态**：
- 默认：文字 `--text-secondary`，图标 `--text-secondary`
- hover：背景 `--bg-muted`，文字 `--text`
- 选中：背景 `--bg-muted`，文字 `--accent`，图标 `--accent`，左侧 `2px` 强调色竖条
- 折叠态：仅显示图标，tooltip 显示文字

**参考实现**：
```tsx
<aside className="w-60 border-r border-border bg-bg flex flex-col">
  {/* Logo 区 */}
  <div className="h-14 flex items-center px-4 border-b border-border">
    <Logo className="w-7 h-7" />
    <span className="ml-2 font-semibold">Brand</span>
  </div>

  {/* 菜单 */}
  <nav className="flex-1 overflow-y-auto p-3">
    {groups.map((group) => (
      <div key={group.label} className="mb-4">
        <p className="px-3 mb-2 text-xs font-medium text-text-muted uppercase">
          {group.label}
        </p>
        <ul className="space-y-1">
          {group.items.map((item) => (
            <li key={item.key}>
              <a
                href={item.href}
                className={`flex items-center gap-3 px-3 h-10 rounded-sm text-sm transition-colors
                  ${item.active
                    ? 'bg-bg-muted text-accent border-l-2 border-accent -ml-3 pl-[14px]'
                    : 'text-text-secondary hover:bg-bg-muted hover:text-text'}`}
              >
                <item.icon size={18} />
                <span>{item.label}</span>
              </a>
            </li>
          ))}
        </ul>
      </div>
    ))}
  </nav>
</aside>
```

---

## 范式 3：响应式移动端导航

**适合**：所有需要适配移动端的网站。

**做法**：
- 桌面端显示完整菜单，移动端隐藏
- 移动端显示汉堡按钮（`≡`），点击展开抽屉
- 抽屉从右侧滑入，宽度 `280px`，背景 `--bg`
- 抽屉内菜单垂直排列，每项高度 `48px`
- 遮罩层半透明黑 `rgba(0,0,0,0.4)`
- 动画：抽屉 `translate-x` 过渡 `250ms ease-out`

```tsx
{/* 移动端汉堡按钮 */}
<button className="md:hidden p-2" onClick={() => setOpen(true)}>
  <Menu size={20} />
</button>

{/* 抽屉 */}
{open && (
  <>
    <div className="fixed inset-0 bg-black/40 z-40" onClick={() => setOpen(false)} />
    <div className="fixed right-0 top-0 h-full w-72 bg-bg z-50 border-l border-border p-4">
      <div className="flex justify-between items-center mb-6">
        <span className="font-semibold">菜单</span>
        <button onClick={() => setOpen(false)}><X size={20} /></button>
      </div>
      <nav className="flex flex-col">
        {menuItems.map((item) => (
          <a key={item.key} href={item.href} className="h-12 flex items-center text-text-secondary hover:text-accent">
            {item.label}
          </a>
        ))}
      </nav>
    </div>
  </>
)}
```

---

## 导航栏通用规则

### 高度
- 顶部导航：桌面 `64px`，移动 `56px`，全站统一
- 侧边导航：宽度 `240px`，可折叠到 `64px`

### 菜单项
- 高度：顶部 `36px`，侧边 `40px`，移动抽屉 `48px`
- 圆角：`--radius-sm`
- hover：仅文字变色，不加背景（侧边栏可加 `--bg-muted` 背景）
- 选中态：文字 `--accent`，不要用粗体（保持字重一致，避免布局抖动）

### Logo
- 尺寸：高度 `28px ~ 32px`
- 形式：图标 + 文字，或仅图标
- 不要在导航栏放 slogan 或副标题

### 右侧操作区
- 登录 + 注册按钮：登录用幽灵/描边，注册用实心
- 头像：圆形，`32px`，点击展开下拉菜单
- 通知铃铛：`18px` 图标，有未读时显示红点

### 滚动行为
- 落地页：导航栏初始透明，滚动后变实色 + blur
- 应用内：导航栏始终实色
- 滚动时不要改变高度（避免内容跳动）

### 可访问性
- 导航用 `<nav>` 标签
- 当前页用 `aria-current="page"`
- 移动端抽屉有关闭按钮和遮罩点击关闭
- 键盘 `Esc` 可关闭抽屉
