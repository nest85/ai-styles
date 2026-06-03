---
name: ai-output-styles
description: AI 输出风格库，让 Claude、ChatGPT、Gemini 等 AI 的输出拥有统一、专业、可复用的视觉风格。
---

# AI Output Styles (AOS)

## Purpose

AI Output Styles（AOS）是一个输出风格路由器。

AOS 不负责：

* 内容生成
* 内容分析
* 内容推理
* 内容改写

AOS 只负责：

* 定位 Style
* 加载 Style
* 加载 Template
* 按指定风格输出

---

## Trigger Conditions

仅在以下场景触发：

### Style Name

例如：

```text
style:deepthink
```

```text
style:mckinsey
```

```text
使用 deepthink 风格
```

---

### README Path

例如：

```text
styles/deepthink
```

```text
styles/deepthink/README.md
```

---

### README URL

例如：

```text
https://github.com/xxx/styles/deepthink/README.md
```

---

未满足以上条件时，不主动触发 AOS。

---

## Style Resolution

### Priority 1

精确路径

```text
styles/deepthink/README.md
```

---

### Priority 2

README URL

```text
https://...
```

---

### Priority 3

Style Name

```text
deepthink
```

从本地 styles 目录查找：

```text
styles/{style}/README.md
```

---

## Style Loading

读取目标 Style 的：

```text
README.md
```

获取：

* Style 信息
* 可用模板
* 默认模板

---

## Template Loading

如果用户指定模板：

例如：

```text
style:deepthink

template:report
```

加载：

```text
templates/report.html
```

---

如果用户未指定模板：

加载 README 中声明的默认模板。

---

## Multiple Templates

允许同时指定多个模板。

例如：

```text
style:deepthink

templates:
- report
- summary
```

执行方式：

```text
同一份内容
↓
report.html
↓
输出结果 A

同一份内容
↓
summary.html
↓
输出结果 B
```

模板之间互不依赖。

不进行编排。

不共享上下文。

独立运行。

---

## Output Constraints

AOS 不创造内容。

AOS 不修改事实。

AOS 不修改观点。

AOS 不修改结论。

AOS 不删减原文。

AOS 不改写原文。

仅改变：

* 布局
* 结构
* 视觉表现
* 输出格式

### 保真原则（Fidelity Rule）

原文是不可变的。渲染前先把原文切分为段落数组 `P[]`，渲染后必须验证 `P[i]` 全部出现在输出中。

允许：

* 为原文段落添加结构容器（标签、卡片、表格行、block 组件等）
* 在原文之外添加结构性文字（章节标题、目录标签、表格表头等元信息）
* 调整原文的 HTML 标签（如 `<p>` → `<li>`、纯文本 → `<td>`）以适配组件

不允许：

* 删除原文的任何句子、段落
* 改写原文的任何措辞（哪怕"意思不变"）
* 压缩、摘要、精简原文
* 为适配组件尺寸而裁剪原文

如果原文不适合塞进某组件，应选择更宽松的组件（如普通 `<p>`、`<section>` 正文），而不是裁剪原文来迁就组件。

---

## Output Delivery

产物一律落盘到 `styles/<style>/examples/`。

风格化输出本身就是文件。

消息正文只回复路径。

---

## Repository Convention

```text
styles/
└── deepthink
    ├── README.md
    ├── templates
    ├── examples
    └── preview
```

其中：

README.md

负责描述：

* 风格
* 模板
* 默认模板

templates/

负责定义：

* HTML
* Markdown
* PPT
* Dashboard
* Infographic

等输出模板。
