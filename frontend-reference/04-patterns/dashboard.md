# 场景范式：数据看板 / 后台管理

> 后台管理和数据看板是 B 端项目最常见的场景。核心是**信息密度**与**可读性**的平衡。

---

## 反模式提醒

❌ **避免**：
- 彩色卡片满天飞（每个数据卡一个颜色）
- 图表用默认彩虹配色
- 表格铺满全屏，没有留白
- 侧边栏菜单项用彩色图标

---

## 整体布局

```
┌──────────────────────────────────────────────┐
│  顶部导航栏（搜索、通知、用户）                 │
├────────┬─────────────────────────────────────┤
│        │  页面标题 + 操作按钮                   │
│ 侧边栏  ├─────────────────────────────────────┤
│ 菜单   │                                     │
│        │  数据卡片 / 图表 / 表格                │
│        │                                     │
└────────┴─────────────────────────────────────┘
```

### 尺寸规范
- 顶部导航高度：`56px`
- 侧边栏宽度：`240px`（折叠后 `64px`）
- 内容区左右内边距：`24px`
- 内容区顶部内边距：`24px`

---

## 范式 1：数据概览卡片

**适合**：Dashboard 首页展示核心指标。

```
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ 总用户数  │ │ 今日订单  │ │ 收入      │ │ 转化率    │
│ 12,345   │ │ 1,234    │ │ ¥56,789   │ │ 3.2%     │
│ ↑ 12.5%  │ │ ↓ 2.1%   │ │ ↑ 8.3%    │ │ ↑ 0.5%   │
└──────────┘ └──────────┘ └──────────┘ └──────────┘
```

**要点**：
- 卡片样式统一：边框 + 白底，不用彩色背景
- 数值字号 28px 700，标签 14px `--text-secondary`
- 趋势用小箭头 + 百分比，上涨绿色、下跌红色（仅此处可用颜色）
- 卡片宽度均分，间距 16px

```tsx
<div className="grid grid-cols-4 gap-4">
  {stats.map((stat) => (
    <div key={stat.label} className="rounded-lg border border-border bg-bg p-5">
      <p className="text-sm text-text-secondary">{stat.label}</p>
      <p className="mt-2 text-3xl font-bold tracking-tight">{stat.value}</p>
      <p className={`mt-2 text-sm ${stat.trend > 0 ? 'text-success' : 'text-danger'}`}>
        {stat.trend > 0 ? '↑' : '↓'} {Math.abs(stat.trend)}% 较昨日
      </p>
    </div>
  ))}
</div>
```

---

## 范式 2：数据表格

**适合**：列表、记录、配置项。

**要点**：
- 表头背景 `--bg-subtle`，字重 500
- 行高 48px，单元格水平内边距 16px
- 斑马纹：偶数行 `--bg-subtle`（可选，数据量大时用）
- 悬停行背景 `--bg-muted`
- 操作列放最右，固定宽度
- 分页在表格右下角

```tsx
<div className="rounded-lg border border-border overflow-hidden">
  <table className="w-full text-sm">
    <thead className="bg-bg-subtle">
      <tr>
        <th className="text-left p-4 font-medium">名称</th>
        <th className="text-left p-4 font-medium">状态</th>
        <th className="text-left p-4 font-medium">创建时间</th>
        <th className="text-right p-4 font-medium">操作</th>
      </tr>
    </thead>
    <tbody className="divide-y divide-border">
      {items.map((item) => (
        <tr key={item.id} className="hover:bg-bg-muted transition-colors">
          <td className="p-4 font-medium">{item.name}</td>
          <td className="p-4">
            <span className="inline-flex items-center gap-2 text-xs">
              <span className={`w-1.5 h-1.5 rounded-full ${item.status === 'active' ? 'bg-success' : 'bg-text-muted'}`} />
              {item.status}
            </span>
          </td>
          <td className="p-4 text-text-secondary">{item.date}</td>
          <td className="p-4 text-right">
            <button className="text-text-secondary hover:text-text">编辑</button>
          </td>
        </tr>
      ))}
    </tbody>
  </table>
</div>
```

---

## 范式 3：侧边导航

**要点**：
- 菜单项高度 40px，左右内边距 16px
- 图标 + 文字，图标统一灰色，选中态用 `--accent`
- 选中项背景 `--bg-muted`，左侧 2px 强调色竖条
- 分组标题用 12px `--text-muted` 大写

```tsx
<nav className="w-60 border-r border-border p-4">
  <div className="text-xs font-medium text-text-muted uppercase mb-2">主菜单</div>
  <ul className="space-y-1">
    {menuItems.map((item) => (
      <li key={item.key}>
        <a
          href={`#${item.key}`}
          className={`flex items-center gap-3 px-3 h-10 rounded-sm text-sm transition-colors
            ${active ? 'bg-bg-muted text-accent border-l-2 border-accent' : 'text-text-secondary hover:bg-bg-muted hover:text-text'}`}
        >
          <item.icon size={18} />
          {item.label}
        </a>
      </li>
    ))}
  </ul>
</nav>
```

---

## 图表规范

### 配色
- 主数据序列用 `--accent`
- 多序列时用**同色系深浅**或**低饱和色板**，不要用彩虹色
- 推荐多色板（低饱和）：
  ```
  #09090b, #52525b, #a1a1aa, #d4d4d8  // 灰阶
  #1e40af, #3b82f6, #60a5fa, #93c5fd  // 蓝阶
  ```

### 样式
- 网格线用极浅的 `--border`
- 坐标轴文字 12px `--text-muted`
- 图例放在图表上方或右侧，不要遮挡数据
- Tooltip 用白底 + 边框 + 淡阴影，不要用半透明黑底

### 推荐图表库
- **Recharts**：React 生态，定制性好
- **ECharts**：功能最全，中文文档好
- **Chart.js**：轻量，简单场景

---

## 后台通用规则

1. **信息密度**：B 端用紧凑密度（间距 ×0.85），C 端展示页用舒适
2. **状态色**：仅状态色（成功绿/警告橙/危险红）用于状态指示，其余用灰阶
3. **表格**：能不用图标就不用，文字优先
4. **表单**：标签在输入框上方，必填项标 `*`
5. **操作按钮**：主操作（新增、保存）放右上角，危险操作（删除）放行内且用红色文字
6. **空状态**：用图标 + 一句话 + 一个引导按钮，不要用大面积插画
