# 场景范式：模态框 / 抽屉

> 模态框和抽屉用于聚焦用户完成单一任务。
> 核心原则：**只放必要内容、操作清晰、可随时关闭**。

---

## 反模式提醒

❌ **避免**：
- 模态框里塞完整页面的内容（表单超过 6 个字段）
- 没有关闭按钮，只能点遮罩关闭
- 确认删除的弹窗用红色大背景
- 抽屉从顶部下滑（应该从右侧）
- 模态框宽度超过 `560px`
- 打开动画用缩放+旋转

---

## 范式 1：居中模态框

**适合**：确认操作、简单表单、提示信息。

```
        ┌─────────────────────┐
        │  标题          [×]   │
        ├─────────────────────┤
        │                     │
        │   内容区域            │
        │                     │
        ├─────────────────────┤
        │        [取消] [确认]  │
        └─────────────────────┘
```

**尺寸规范**：
- 宽度：`400px`（小）、`480px`（默认）、`560px`（大），最大不超过 `560px`
- 圆角：`--radius-lg`
- 内边距：`24px`
- 头部高度：`48px`，底部按钮栏高度：`64px`

**结构**：
- 头部：标题（左）+ 关闭按钮（右）
- 内容：可滚动（`max-height: 70vh`）
- 底部：操作按钮（右对齐，主按钮在最右）

**参考实现**：
```tsx
{/* 遮罩 */}
<div className="fixed inset-0 z-50 bg-black/40 backdrop-blur-sm flex items-center justify-center p-4" onClick={onClose}>
  {/* 模态框 */}
  <div className="w-full max-w-md rounded-lg bg-bg border border-border shadow-lg" onClick={(e) => e.stopPropagation()}>
    {/* 头部 */}
    <div className="flex items-center justify-between px-6 h-12 border-b border-border">
      <h3 className="font-semibold">标题</h3>
      <button onClick={onClose} className="p-1 rounded-sm text-text-secondary hover:text-text">
        <X size={18} />
      </button>
    </div>
    {/* 内容 */}
    <div className="p-6 max-h-[70vh] overflow-y-auto">
      内容...
    </div>
    {/* 底部 */}
    <div className="flex justify-end gap-3 px-6 h-16 items-center border-t border-border">
      <button onClick={onClose} className="h-9 px-4 rounded-sm border border-border text-sm font-medium">
        取消
      </button>
      <button className="h-9 px-4 rounded-sm bg-accent text-accent-foreground text-sm font-medium">
        确认
      </button>
    </div>
  </div>
</div>
```

### 确认删除弹窗

```
┌─────────────────────┐
│  ⚠ 删除确认          │
├─────────────────────┤
│  确定要删除吗？       │
│  此操作不可撤销。      │
├─────────────────────┤
│      [取消] [删除]   │
└─────────────────────┘
```

**要点**：
- 标题加警告图标（`--warning` 色）
- 删除按钮用 `--danger` 色实心
- 文案明确说明后果（"不可撤销"）

---

## 范式 2：右侧抽屉

**适合**：详情查看、编辑表单、侧边配置。

```
┌──────────────────────────┬──────────┐
│                          │          │
│      页面内容（遮罩变暗）   │  抽屉内容  │
│                          │          │
│                          │          │
└──────────────────────────┴──────────┘
```

**尺寸规范**：
- 宽度：`360px`（小）、`480px`（默认）、`640px`（大）
- 高度：全屏 `100vh`
- 圆角：左上、左下 `--radius-lg`，右侧无圆角
- 从右侧滑入，动画 `translate-x` 250ms

**要点**：
- 抽屉内布局同模态框（头/内容/底）
- 内容可滚动
- 遮罩点击关闭

```tsx
<div className="fixed inset-0 z-50 flex">
  <div className="flex-1 bg-black/40" onClick={onClose} />
  <div className="w-full max-w-md bg-bg border-l border-border shadow-lg flex flex-col">
    <div className="flex items-center justify-between px-6 h-14 border-b border-border">
      <h3 className="font-semibold">编辑</h3>
      <button onClick={onClose}><X size={18} /></button>
    </div>
    <div className="flex-1 overflow-y-auto p-6">内容...</div>
    <div className="flex justify-end gap-3 px-6 h-16 items-center border-t border-border">
      <button className="h-9 px-4 rounded-sm border border-border text-sm">取消</button>
      <button className="h-9 px-4 rounded-sm bg-accent text-accent-foreground text-sm font-medium">保存</button>
    </div>
  </div>
</div>
```

---

## 范式 3：轻量提示（Toast / Notification）

**适合**：操作成功、保存完成、短暂通知。

```
┌─────────────────────┐
│  ✓ 保存成功    [×]   │
└─────────────────────┘
```

**要点**：
- 位置：右上角或顶部居中
- 宽度：`320px ~ 380px`
- 圆角：`--radius-md`
- 背景：`--bg` + 边框 + 阴影
- 自动消失：`3s`（成功）、`5s`（错误），或手动关闭
- 成功用 `--success` 图标，错误用 `--danger`，警告用 `--warning`

```tsx
<div className="fixed top-4 right-4 z-50 flex items-center gap-3 w-80 rounded-md border border-border bg-bg p-4 shadow-md">
  <CheckCircle size={18} className="text-success" />
  <span className="text-sm">保存成功</span>
  <button className="ml-auto text-text-muted hover:text-text"><X size={16} /></button>
</div>
```

---

## 模态框通用规则

### 打开/关闭
- 打开：淡入 + 轻微上移（`translateY(8px)` → `0`），200ms
- 关闭：淡出 + 下移，150ms
- 不要用缩放、旋转等夸张动画
- `Esc` 键可关闭
- 点击遮罩可关闭（确认删除类弹窗可禁用）

### 内容
- 字段数 ≤ 6 个，超过用抽屉或独立页面
- 标题 ≤ 15 字
- 底部按钮最多 2 个（取消 + 主操作）
- 危险操作（删除）的主按钮用 `--danger`

### 可访问性
- 用 `role="dialog"` 和 `aria-modal="true"`
- 打开时焦点移入模态框，关闭时焦点回到触发元素
- `Tab` 键在模态框内循环
- 背景内容 `aria-hidden`
