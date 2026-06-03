# DeepThink Style

> AI Output Style — 风格套件 · v1.0

---

## 风格定位

**DeepThink** 是一套以学术咨询报告为灵感的视觉风格。

设计基调：纸张质感、克制的墨色系、精准的信息层级、适合长文阅读。推荐用于分析报告、研究结论、深度调研等内容较多的文档，内容较少时同样可以使用。

---

## 使用方式

把以下内容复制给 Claude、ChatGPT、Gemini，附上你要美化的内容：

```
请使用 DeepThink 风格（读取下面的 README，按其中说明加载模板）：
https://raw.githubusercontent.com/nest85/ai-output-styles/main/styles/deepthink/README.md
 
把以下内容用这套风格渲染：
[你的 AI 输出内容]
```
---

## 模板列表

AI注意：
1. 存在多个模板，而用户未指定模板时，列出选项提醒用户选择。
2. 需加载文件先从本地目录找；
   找不到则用 https://raw.githubusercontent.com/nest85/ai-output-styles/main/styles/deepthink/ + 需加载文件 查找；
   只要有任一需加载文件最终找不到，就停止生成，并告诉用户缺哪个文件； 不要自行编造样式或用其它风格替代。

| 模板名称              | 适合场景                     | 需加载文件                   | 渲染依赖            |
|-------------------|--------------------------|-------------------------|-----------------|
| `report.html`     | 可视化 / 可打印 PDF / 浏览器分享    | `templates/report.html` | 浏览器             |
| `report-css.html` | 可视化 / 可打印 PDF / 浏览器分享    | `templates/report-css.html` | 浏览器             |
| `report.md`       | 文档站 / 邮件 / 纯文本归档 / PR 描述 | `templates/report.md`   | 任意 Markdown 渲染器 |

**默认模板：** `report.html`