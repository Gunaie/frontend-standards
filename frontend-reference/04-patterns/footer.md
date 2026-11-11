# 场景范式：页脚

> 页脚是页面的收尾，作用是提供导航补充、法律信息和品牌收尾。
> 核心原则：**信息分层清晰、不抢主内容风头、与导航栏风格统一**。

---

## 反模式提醒

❌ **避免**：
- 页脚用深色背景 + 彩色图标（像 2010 年的网站）
- 链接列表用彩色图标当项目符号
- 版权信息字号过大或加粗
- 页脚高度超过首屏的 1/3
- 把所有页面链接都塞进页脚（信息过载）
- 社交媒体图标用各品牌原色（Facebook 蓝、Twitter 蓝等）

---

## 范式 1：多列链接式（最通用）

**适合**：官网、SaaS、内容站。

```
┌──────────────────────────────────────────────────┐
│  Logo + 一句话介绍                                  │
│                                                   │
│  产品        资源        公司        订阅           │
│  功能        博客        关于我们    [输入框] [按钮] │
│  定价        文档        加入我们                   │
│  下载        帮助中心     联系方式                   │
│                                                   │
│  ─────────────────────────────────────────────    │
│  © 2026 Brand.  隐私政策  服务条款  Cookie 设置      │
└──────────────────────────────────────────────────┘
```

**要点**：
- 列数：3~5 列，每列标题 `14px` 字重 600，链接 `14px` 颜色 `--text-secondary`
- 列间距：`32px`
- 顶部内边距：`64px`，底部：`32px`
- 底部版权栏：上边框 `1px solid var(--border)`，内边距 `24px 0`
- 版权文字：`14px`，颜色 `--text-muted`

**参考实现**：
```tsx
<footer className="border-t border-border bg-bg">
  <div className="container mx-auto px-6 py-16">
    <div className="grid grid-cols-2 md:grid-cols-5 gap-8">
      {/* 品牌区 */}
      <div className="col-span-2">
        <div className="flex items-center gap-2">
          <Logo className="w-8 h-8" />
          <span className="font-bold text-lg">Brand</span>
        </div>
        <p className="mt-4 text-sm text-text-secondary max-w-xs">
          一句话介绍你的产品，简洁有力。
        </p>
      </div>

      {/* 链接列 */}
      {footerLinks.map((col) => (
        <div key={col.title}>
          <h4 className="text-sm font-semibold mb-4">{col.title}</h4>
          <ul className="space-y-3">
            {col.links.map((link) => (
              <li key={link.label}>
                <a href={link.href} className="text-sm text-text-secondary hover:text-accent transition-colors">
                  {link.label}
                </a>
              </li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  </div>

  {/* 版权栏 */}
  <div className="border-t border-border">
    <div className="container mx-auto px-6 py-6 flex flex-col md:flex-row justify-between items-center gap-4">
      <p className="text-sm text-text-muted">© 2026 Brand. 保留所有权利。</p>
      <div className="flex gap-6">
        <a href="#" className="text-sm text-text-secondary hover:text-accent">隐私政策</a>
        <a href="#" className="text-sm text-text-secondary hover:text-accent">服务条款</a>
        <a href="#" className="text-sm text-text-secondary hover:text-accent">Cookie 设置</a>
      </div>
    </div>
  </div>
</footer>
```

---

## 范式 2：极简单列式

**适合**：极简产品、工具站、个人网站。

```
┌──────────────────────────────────────────────────┐
│                                                   │
│              Logo + 一句话                          │
│                                                   │
│        链接1 · 链接2 · 链接3 · 链接4                │
│                                                   │
│              © 2026 Brand                         │
│                                                   │
└──────────────────────────────────────────────────┘
```

**要点**：
- 居中布局，大量留白
- 链接一行排列，用 `·` 分隔
- 字号 `14px`，颜色 `--text-secondary`
- 总高度 `200px ~ 240px`

---

## 范式 3：带订阅表单式

**适合**：内容站、Newsletter、需要收集用户的产品。

```
┌──────────────────────────────────────────────────┐
│                                                   │
│   订阅我们的更新                      [输入框] [订阅] │
│                                                   │
│   链接列 ...                                       │
│                                                   │
│   © 2026 Brand.  隐私  条款                        │
└──────────────────────────────────────────────────┘
```

**要点**：
- 订阅区在页脚顶部，有独立背景（`--bg-subtle`）或上边框
- 输入框和按钮在同一行，高度 `40px`
- 输入框圆角 `--radius-sm`，按钮实心
- 下方是标准链接列 + 版权栏

---

## 页脚通用规则

### 配色
- 背景：`--bg` 或 `--bg-subtle`，不要用深色（除非全站深色）
- 链接：`--text-secondary`，hover 变 `--accent`
- 版权文字：`--text-muted`
- 社交媒体图标：统一 `--text-secondary`，hover 变 `--accent`（不要用品牌原色）

### 排版
- 列标题：`14px`，字重 600，颜色 `--text`
- 链接文字：`14px`，颜色 `--text-secondary`
- 版权信息：`13px ~ 14px`，颜色 `--text-muted`
- 行高：链接列表行高 `1.5`，行间距 `12px`

### 间距
- 页脚顶部内边距：`48px ~ 64px`
- 列之间间距：`32px`
- 列内链接间距：`12px`
- 版权栏与上方间距：上边框 + `24px` 内边距

### 社交媒体
- 图标统一单色（`--text-secondary`），hover 变 `--accent`
- 尺寸：`20px`
- 间距：`16px`
- 平台：最多放 4~5 个主流平台，不要全放

### 内容组织
- 链接列标题用名词（产品、资源、公司），不要用动词
- 每列链接不超过 5 个
- 法律链接（隐私、条款）放版权栏，不放链接列
- 不要在页脚放导航栏已有的主要菜单（重复）

### 响应式
- 桌面：多列网格
- 平板：2 列
- 移动端：单列堆叠，品牌区在上，链接列在下
- 版权栏：移动端居中堆叠
