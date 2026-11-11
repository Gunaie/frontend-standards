# 场景范式：登录 / 注册页

> 登录注册页是用户进入产品的第一步。简洁、可信、无障碍是核心。

---

## 反模式提醒

❌ **避免**：
- 背景用渐变或花哨插画
- 表单字段过多（注册页超过 4 个字段）
- "记住我"和"忘记密码"排版混乱
- 第三方登录按钮堆满一排
- 输入框标签在左边

---

## 范式 1：居中卡片式（最通用）

**适合**：大多数产品，简洁高效。

```
┌─────────────────────────────────────────┐
│                                         │
│            ┌──────────────┐             │
│            │  Logo / 品牌  │             │
│            │              │             │
│            │  邮箱 [____] │             │
│            │  密码 [____] │             │
│            │              │             │
│            │ [  登 录  ]   │             │
│            │              │             │
│            │ 第三方登录    │             │
│            │ 注册账号      │             │
│            └──────────────┘             │
│                                         │
└─────────────────────────────────────────┘
```

**要点**：
- 卡片宽度 380px ~ 420px
- 卡片用边框 + 淡阴影，不用大阴影
- 背景纯色 `--bg`，可加极淡的网格纹理
- 输入框高度 40px，圆角 `--radius-sm`
- 登录按钮宽度 100%，高度 40px

```tsx
<div className="min-h-screen flex items-center justify-center bg-bg px-4">
  <div className="w-full max-w-md">
    <div className="text-center mb-8">
      <Logo className="w-10 h-10 mx-auto" />
      <h1 className="mt-4 text-2xl font-bold">欢迎回来</h1>
      <p className="mt-2 text-sm text-text-secondary">登录你的账号</p>
    </div>
    <div className="rounded-lg border border-border bg-bg p-8 shadow-sm">
      <form className="space-y-4">
        <div className="space-y-2">
          <label className="text-sm font-medium">邮箱</label>
          <input type="email" className="w-full h-10 rounded-sm border border-border px-3 focus:outline-none focus:ring-2 focus:ring-accent/20 focus:border-accent" />
        </div>
        <div className="space-y-2">
          <div className="flex justify-between">
            <label className="text-sm font-medium">密码</label>
            <a href="#" className="text-sm text-accent hover:underline">忘记密码？</a>
          </div>
          <input type="password" className="w-full h-10 rounded-sm border border-border px-3" />
        </div>
        <button type="submit" className="w-full h-10 rounded-sm bg-accent text-accent-foreground font-medium">
          登录
        </button>
      </form>
      <div className="mt-6 flex items-center gap-4">
        <div className="flex-1 h-px bg-border" />
        <span className="text-xs text-text-muted">或</span>
        <div className="flex-1 h-px bg-border" />
      </div>
      <div className="mt-6 grid grid-cols-2 gap-3">
        <button className="h-10 rounded-sm border border-border text-sm font-medium">GitHub</button>
        <button className="h-10 rounded-sm border border-border text-sm font-medium">Google</button>
      </div>
    </div>
    <p className="mt-6 text-center text-sm text-text-secondary">
      还没有账号？<a href="#" className="text-accent hover:underline">注册</a>
    </p>
  </div>
</div>
```

---

## 范式 2：左右分栏式

**适合**：品牌展示需求强的产品，左侧放品牌内容。

```
┌──────────────────────┬──────────────────────┐
│                      │                      │
│   品牌视觉区           │   登录表单卡片         │
│   （截图/插画/文案）   │                      │
│                      │   邮箱 [____]         │
│                      │   密码 [____]         │
│                      │                      │
│                      │   [  登 录  ]          │
│                      │                      │
└──────────────────────┴──────────────────────┘
```

**要点**：
- 左右比例 1:1 或 6:4
- 左侧背景可用深色或品牌色，右侧白色
- 左侧内容：品牌名、一句话标语、1~2 个产品亮点
- 左侧不要放太多信息，保持简洁

---

## 范式 3：极简全屏式

**适合**：极简产品、内部工具。

```
┌─────────────────────────────────────────┐
│                                         │
│                                         │
│         邮箱                              │
│         [____________________]          │
│                                         │
│         密码                              │
│         [____________________]          │
│                                         │
│         [  登 录  ]                       │
│                                         │
│         还没有账号？注册                   │
│                                         │
└─────────────────────────────────────────┘
```

**要点**：
- 无卡片，纯文字 + 输入框
- 输入框无边框，只有底部下划线
- 大量留白，极简风格
- 适合预设 A（极简黑白）

---

## 登录页通用规则

### 表单字段
- **登录**：邮箱 + 密码（最多加"记住我"）
- **注册**：邮箱 + 密码 + 确认密码（用户名可选）
- 超过 4 个字段的注册，分步进行

### 按钮
- 登录按钮：实心 `--accent`，100% 宽度
- 第三方登录：描边按钮，最多 2~3 个
- 注册链接：文字链接，放在底部

### 错误处理
- 字段级错误：输入框下方红色小字
- 全局错误：表单顶部红色提示条
- 不要用 alert 弹窗

### 安全提示
- 密码输入框右侧加"显示/隐藏"切换
- "忘记密码"链接放在密码字段旁边
- 注册页加服务条款勾选

### 无障碍
- 所有输入框有 `<label>`
- 按钮有 `type="submit"`
- 错误信息关联到输入框（`aria-describedby`）
