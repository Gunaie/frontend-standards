# 场景范式：复杂表单

> 登录注册以外的表单（创建、编辑、配置）。
> 核心原则：**分组清晰、校验即时、操作可达、不 overwhelm 用户**。

---

## 反模式提醒

❌ **避免**：
- 20 个字段平铺在一个长页面里
- 标签在输入框左边（除并排短字段）
- 所有字段都是必填，没有分级
- 提交按钮在表单最底部，需要滚动很久
- 必填标记用 `*` 但颜色不明显
- 校验只在提交时才触发

---

## 范式 1：分组表单

**适合**：创建/编辑实体，字段 6~15 个。

```
┌─────────────────────────────────────┐
│  基本信息                             │
│  ──────                              │
│  名称        [__________]            │
│  描述        [__________]            │
│                                     │
│  高级设置                             │
│  ──────                              │
│  可见性      [公开 ▾]                │
│  标签        [__________]            │
│                                     │
│              [取消]  [保存]           │
└─────────────────────────────────────┘
```

**要点**：
- 按逻辑分组，每组有标题（`18px` 600）+ 描述（`14px` --text-secondary）
- 组间距：`40px`
- 每组内字段垂直排列，间距 `20px`
- 提交按钮固定在底部或右下角

**参考实现**：
```tsx
<form className="max-w-2xl space-y-10">
  {/* 分组 1 */}
  <section>
    <h3 className="text-lg font-semibold">基本信息</h3>
    <p className="mt-1 text-sm text-text-secondary">填写项目的基本信息</p>
    <div className="mt-6 space-y-5">
      <div className="space-y-2">
        <label className="text-sm font-medium">名称 <span className="text-danger">*</span></label>
        <input className="w-full h-10 rounded-sm border border-border px-3" />
      </div>
      <div className="space-y-2">
        <label className="text-sm font-medium">描述</label>
        <textarea rows={3} className="w-full rounded-sm border border-border px-3 py-2" />
      </div>
    </div>
  </section>

  {/* 分组 2 */}
  <section>
    <h3 className="text-lg font-semibold">高级设置</h3>
    <div className="mt-6 space-y-5">
      {/* 并排字段 */}
      <div className="grid grid-cols-2 gap-4">
        <div className="space-y-2">
          <label className="text-sm font-medium">可见性</label>
          <select className="w-full h-10 rounded-sm border border-border px-3">
            <option>公开</option><option>私有</option>
          </select>
        </div>
        <div className="space-y-2">
          <label className="text-sm font-medium">过期时间</label>
          <input type="date" className="w-full h-10 rounded-sm border border-border px-3" />
        </div>
      </div>
    </div>
  </section>

  {/* 操作栏 */}
  <div className="flex justify-end gap-3 pt-4 border-t border-border">
    <button type="button" className="h-10 px-4 rounded-sm border border-border text-sm font-medium">取消</button>
    <button type="submit" className="h-10 px-4 rounded-sm bg-accent text-accent-foreground text-sm font-medium">保存</button>
  </div>
</form>
```

---

## 范式 2：多步骤表单

**适合**：字段超过 15 个，或有明确的流程顺序。

```
┌─────────────────────────────────────┐
│  ① 基本信息 → ② 配置 → ③ 确认         │
│  ─────────────                       │
│                                     │
│  当前步骤的字段...                     │
│                                     │
│       [上一步]  [下一步]              │
└─────────────────────────────────────┘
```

**要点**：
- 步骤指示器在顶部，已完成步骤打勾，当前步骤高亮
- 每步字段 ≤ 6 个
- 步骤间可前进/后退，数据保留
- 最后一步是确认页（只读展示所有信息）

**步骤指示器**：
```tsx
<div className="flex items-center gap-2 mb-8">
  {steps.map((step, i) => (
    <div key={i} className="flex items-center gap-2">
      <div className={`w-7 h-7 rounded-full flex items-center justify-center text-xs font-medium
        ${i < current ? 'bg-accent text-accent-foreground' :
          i === current ? 'border-2 border-accent text-accent' :
          'border border-border text-text-muted'}`}>
        {i < current ? <Check size={14} /> : i + 1}
      </div>
      <span className={`text-sm ${i === current ? 'font-medium' : 'text-text-muted'}`}>{step.label}</span>
      {i < steps.length - 1 && <div className="w-8 h-px bg-border" />}
    </div>
  ))}
</div>
```

---

## 范式 3：内联编辑

**适合**：表格行内编辑、配置项快速修改。

```
┌─────────────────────────────────────┐
│  名称    项目 A              [编辑]   │
│  状态    已启用                       │
└─────────────────────────────────────┘

点击编辑后：
┌─────────────────────────────────────┐
│  名称    [项目 A____]    [取消] [保存] │
│  状态    [已启用 ▾]                   │
└─────────────────────────────────────┘
```

**要点**：
- 点击编辑后，文字变输入框
- 操作按钮（取消/保存）出现在行内
- 保存后恢复只读状态
- 取消恢复原值

---

## 表单通用规则

### 字段布局
- 默认垂直排列（标签在上，输入框在下）
- 短字段（日期、数量、下拉）可并排 2~3 列
- 标签与输入框间距 `8px`
- 字段之间间距 `20px`

### 必填标记
- 用红色 `*` 紧跟标签文字
- 颜色 `--danger`
- 不要用"（必填）"文字

### 校验
- **实时校验**：输入时（失焦后）就提示，不要等提交
- **错误位置**：输入框下方 `8px`，红色 `12px` 文字
- **错误状态**：输入框边框 `--danger`
- **提交校验**：未填字段滚动到第一个错误处并聚焦

### 输入框类型
- 文本：`input`，高度 `40px`
- 长文本：`textarea`，最小 3 行
- 选择：`select` 或自定义下拉，高度 `40px`
- 开关：Toggle，不要用 checkbox（除非列表多选）
- 日期：date picker，不要让用户手输

### 按钮
- 主按钮（保存/提交）：`--accent` 实心
- 次按钮（取消）：描边
- 危险操作（删除）：`--danger` 文字按钮，不放主按钮位置
- 按钮组右对齐，主按钮在最右

### 辅助说明
- 字段下方可加 `12px` `--text-muted` 帮助文字
- 不要用 placeholder 当标签（placeholder 输入后消失）

### 加载与提交
- 提交时按钮变 loading 状态（spinner + 文字变"提交中"）
- 提交中禁用表单
- 成功后 Toast 提示 + 跳转或关闭
