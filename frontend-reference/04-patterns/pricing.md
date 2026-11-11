# 场景范式：定价/对比页

> 定价页是转化的关键页面。本文档提供定价卡片、功能对比表、FAQ 的布局范式。

---

## 反模式提醒

❌ **避免**：
- 三张卡片完全一样的样式，只靠"推荐"标签区分
- 推荐卡片用花哨的渐变和阴影
- 功能列表用 emoji 或彩色图标
- 价格字号巨大且加粗到夸张

---

## 范式 1：三档定价卡片（标准版）

**适合**：SaaS 产品，有免费/专业/企业三档。

```
┌──────────┐  ┌──────────┐  ┌──────────┐
│  免费版    │  │ 专业版 ✨ │  │  企业版    │
│  ¥0/月    │  │ ¥99/月    │  │  定制     │
│          │  │          │  │          │
│ • 功能 A  │  │ • 功能 A  │  │ • 全部    │
│ • 功能 B  │  │ • 功能 B  │  │ • 专属    │
│          │  │ • 功能 C  │  │ • SLA    │
│ [按钮]    │  │ [按钮]    │  │ [联系]    │
└──────────┘  └──────────┘  └──────────┘
```

**要点**：
- 三档卡片，中间档为"推荐"
- 推荐卡的区分方式：**边框用强调色 + 顶部"推荐"标签**，不要用渐变背景
- 卡片之间间距 24px，垂直对齐
- 价格字号 36px，货币符号和周期字号 16px

**参考实现**：
```tsx
<section className="container mx-auto px-6 py-20">
  <div className="text-center mb-12">
    <h2 className="text-4xl font-bold">选择适合你的方案</h2>
    <p className="mt-4 text-text-secondary">随时升级或降级</p>
  </div>
  <div className="grid grid-cols-3 gap-6 max-w-5xl mx-auto">
    {/* 免费版 */}
    <div className="rounded-lg border border-border p-6">
      <h3 className="text-lg font-semibold">免费版</h3>
      <div className="mt-4">
        <span className="text-4xl font-bold">¥0</span>
        <span className="text-text-secondary">/月</span>
      </div>
      <ul className="mt-6 space-y-3 text-sm">
        <li>• 最多 3 个项目</li>
        <li>• 基础功能</li>
        <li>• 社区支持</li>
      </ul>
      <button className="mt-8 w-full h-10 rounded-sm border border-border font-medium">
        免费开始
      </button>
    </div>
    {/* 推荐版 */}
    <div className="rounded-lg border-2 border-accent p-6 relative">
      <span className="absolute -top-3 left-1/2 -translate-x-1/2 px-3 py-1 text-xs font-medium bg-accent text-accent-foreground rounded-full">
        推荐
      </span>
      <h3 className="text-lg font-semibold">专业版</h3>
      <div className="mt-4">
        <span className="text-4xl font-bold">¥99</span>
        <span className="text-text-secondary">/月</span>
      </div>
      <ul className="mt-6 space-y-3 text-sm">
        <li>• 无限项目</li>
        <li>• 全部功能</li>
        <li>• 优先支持</li>
      </ul>
      <button className="mt-8 w-full h-10 rounded-sm bg-accent text-accent-foreground font-medium">
        开始试用
      </button>
    </div>
    {/* 企业版 */}
    <div className="rounded-lg border border-border p-6">
      <h3 className="text-lg font-semibold">企业版</h3>
      <div className="mt-4">
        <span className="text-4xl font-bold">定制</span>
      </div>
      <ul className="mt-6 space-y-3 text-sm">
        <li>• 所有专业版功能</li>
        <li>• 专属客服</li>
        <li>• SLA 保障</li>
      </ul>
      <button className="mt-8 w-full h-10 rounded-sm border border-border font-medium">
        联系销售
      </button>
    </div>
  </div>
</section>
```

---

## 范式 2：功能对比表

**适合**：功能差异复杂，需要详细对比的产品。

**要点**：
- 第一列是功能名，其余列是各套餐
- 用 ✓ / ✗ 或文字标注，不要用彩色图标
- 表头吸顶（sticky）
- 斑马纹可选，用极浅的背景色区分行

```tsx
<div className="overflow-x-auto rounded-lg border border-border">
  <table className="w-full text-sm">
    <thead>
      <tr className="border-b border-border bg-bg-subtle">
        <th className="text-left p-4 font-medium">功能</th>
        <th className="p-4 font-medium">免费版</th>
        <th className="p-4 font-medium text-accent">专业版</th>
        <th className="p-4 font-medium">企业版</th>
      </tr>
    </thead>
    <tbody>
      <tr className="border-b border-border">
        <td className="p-4">项目数量</td>
        <td className="p-4 text-center">3 个</td>
        <td className="p-4 text-center text-accent">无限</td>
        <td className="p-4 text-center">无限</td>
      </tr>
      <tr>
        <td className="p-4">API 调用</td>
        <td className="p-4 text-center text-text-muted">—</td>
        <td className="p-4 text-center text-accent">10万/月</td>
        <td className="p-4 text-center">无限</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## 范式 3：FAQ 折叠面板

**适合**：定价页底部解答常见疑问。

**要点**：
- 手风琴式展开，一次只展开一个
- 展开/收起用 `+` / `−` 或 `▾` / `▴` 图标
- 过渡用 `max-height` 或 grid 动画，不要用 slideDown 飞入

```tsx
<div className="max-w-2xl mx-auto divide-y divide-border">
  {faqs.map((faq, i) => (
    <div key={i} className="py-4">
      <button className="flex w-full justify-between items-center text-left font-medium">
        {faq.question}
        <ChevronDown className={`w-5 h-5 transition-transform ${open ? 'rotate-180' : ''}`} />
      </button>
      {open && <p className="mt-3 text-text-secondary">{faq.answer}</p>}
    </div>
  ))}
</div>
```

---

## 定价页通用规则

1. **价格展示**：整数部分 36px 700，周期部分 16px 400，颜色 `--text-secondary`
2. **推荐标识**：用 `--accent` 边框 + 小标签，不用渐变背景
3. **功能列表**：用 `•` 或 `✓`，不用彩色图标
4. **按钮**：推荐卡的按钮是实心，其余是描边
5. **月付/年付切换**：放在定价上方，用 Segmented Control，不要用两个按钮
6. **信任元素**：底部可加"已有 XX 用户"、"30 天退款"等小字
