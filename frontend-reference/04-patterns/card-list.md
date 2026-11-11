# 场景范式：卡片列表 / 网格布局

> 卡片是展示多条同类内容的核心组件。
> 核心原则：**卡片样式统一、信息层级清晰、Hover 有反馈但不夸张**。

---

## 反模式提醒

❌ **避免**：
- 每张卡片配色不同（彩虹卡片）
- 卡片用大阴影 + 大圆角（玩具感）
- 卡片内信息过载（什么都往里塞）
- 卡片高度参差不齐（除非故意做瀑布流）
- 卡片之间间距不一致
- Hover 时卡片放大或浮起过高

---

## 范式 1：等宽网格

**适合**：产品列表、功能入口、内容卡片。

```
┌────────┐ ┌────────┐ ┌────────┐
│  卡片1   │ │  卡片2   │ │  卡片3   │
│         │ │         │ │         │
└────────┘ └────────┘ └────────┘
┌────────┐ ┌────────┐ ┌────────┐
│  卡片4   │ │  卡片5   │ │  卡片6   │
└────────┘ └────────┘ └────────┘
```

**要点**：
- 列数：桌面 3~4 列，平板 2 列，移动端 1 列
- 卡片间距：`16px ~ 24px`
- 卡片高度：统一（用 `align-items: stretch`）
- 圆角：`--radius-md`，边框 `--border`，无阴影或 `--shadow-xs`

```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  {items.map((item) => (
    <div key={item.id} className="rounded-md border border-border bg-bg p-6 hover:border-accent transition-colors">
      <h3 className="font-semibold">{item.title}</h3>
      <p className="mt-2 text-sm text-text-secondary">{item.description}</p>
    </div>
  ))}
</div>
```

---

## 范式 2：横向列表

**适合**：搜索结果、文章列表、消息列表。

```
┌─────────────────────────────────────┐
│ [图]  标题                            │
│       描述文字...                      │
│       标签  时间              [操作]   │
├─────────────────────────────────────┤
│ [图]  标题                            │
│       ...                            │
└─────────────────────────────────────┘
```

**要点**：
- 每行高度 `80px ~ 96px`
- 左侧缩略图：`64px ~ 80px` 方形或 16:9
- 中间内容区：标题（16px 600）+ 描述（14px --text-secondary，1~2 行截断）
- 右侧操作区：按钮或时间
- 行间用 `--border` 分隔线，或用卡片间距

```tsx
<div className="divide-y divide-border rounded-md border border-border bg-bg">
  {items.map((item) => (
    <div key={item.id} className="flex items-center gap-4 p-4 hover:bg-bg-muted transition-colors">
      <img src={item.thumb} className="w-16 h-16 rounded-sm object-cover" />
      <div className="flex-1 min-w-0">
        <h3 className="font-medium truncate">{item.title}</h3>
        <p className="mt-1 text-sm text-text-secondary line-clamp-1">{item.desc}</p>
        <div className="mt-2 flex gap-2">
          {item.tags.map((t) => (
            <span key={t} className="text-xs text-text-muted">{t}</span>
          ))}
        </div>
      </div>
      <button className="h-9 px-3 rounded-sm border border-border text-sm">查看</button>
    </div>
  ))}
</div>
```

---

## 范式 3：瀑布流

**适合**：图片墙、作品集、高度不一的内容。

```
┌──────┐ ┌──────┐ ┌──────┐
│      │ │      │ │      │
│      │ └──────┘ │      │
└──────┘ ┌──────┐ │      │
         │      │ └──────┘
         └──────┘ ┌──────┐
                  │      │
                  └──────┘
```

**要点**：
- 用 CSS `columns` 或 JS 瀑布流（如 Masonry）
- 卡片间距：`16px`
- 图片比例不固定，随内容
- 仅用于视觉为主的内容，不用于需要对齐的列表

---

## 卡片通用规则

### 卡片结构
```
┌─────────────────────┐
│  封面图（可选）        │  ← 圆角上，无下圆角
├─────────────────────┤
│  标题                │  ← 16~18px, 600
│  描述（1~2行）        │  ← 14px, --text-secondary
│                     │
│  标签 / 元信息        │  ← 12px, --text-muted
├─────────────────────┤
│  操作区（可选）       │  ← 按钮或链接
└─────────────────────┘
```

### 样式
- 边框 `1px solid var(--border)` 优先，阴影仅 hover 时用 `--shadow-sm`
- 圆角 `--radius-md`
- 内边距 `20px ~ 24px`
- Hover：边框变 `--accent` 或极淡阴影，不要放大或上浮超过 `4px`

### 信息层级
1. 标题（粗、大）
2. 描述（次级色、小）
3. 元信息（最浅、最小）
- 不要在卡片里放超过 3 行信息

### 图片
- 比例统一（16:9 或 1:1），用 `object-fit: cover`
- 圆角与卡片一致，仅顶部圆角
- 加载时用骨架屏占位

### 操作
- 主要操作放在卡片底部或右下角
- 次要操作 hover 时才显示（如删除、收藏）
- 整个卡片可点击时，加 `cursor-pointer`，但内部按钮需 `stopPropagation`
