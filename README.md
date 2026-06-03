# AI Output Styles

> AI 输出风格库，让 Claude、ChatGPT、Gemini 等 AI 的输出拥有统一、专业、可复用的视觉风格。

## 项目简介

大模型已经很好地解决了内容生成问题。但在实际使用过程中，AI 输出的内容仍然存在：

* 结构不统一
* 视觉层次较弱
* 不适合直接汇报或交付
* 风格难以复用

AI Output Styles 希望解决的是 **AI 输出风格标准化与美化** 问题。

## 核心理念

AI Output Styles 不创造内容。 它只改变内容的呈现方式。你可以理解为：
> AI 输出领域的风格库（Style Library）。

* 保留原始事实
* 保留原始观点
* 保留原始结论
* 优化结构与展示效果

---

## 已收录风格

| 风格                | 适用场景      |
|-------------------|-----------|
| DeepThink         | 深度分析、研究报告 |
| McKinsey          | 咨询报告、商业分析 |
| Investment        | 投资分析      |
| Product PRD       | 产品需求文档    |
| Tech Architecture | 技术架构方案    |

后续将持续增加更多风格，欢迎提交新的风格。

建议目录结构：

```text
your-style/
├── README.md               # 风格说明
├── templates/              # 模板文件
│   ├── report.md
│   ├── report.html
│   ├── presentation.pptx
│   └── dashboard.html
├── examples/               # 输入输出示例
└── preview/                # 效果预览
```

---

## 使用方式

- **方式一：免安装，直接引用风格链接**

  将某个 Style 的 README.md 链接提供给 Claude、ChatGPT、Gemini。例如：

    ```text
    请使用 DeepThink 风格。
    
    读取：
    styles/deepthink/README.md
    
    然后将以上内容按照该风格输出：
    ...
    ```

- **方式二：Agent Skill 形式调用**

  安装为 Claude Code/OpenClaw/Codex Skill，在 Agent 中调用。例如：
    ```text
    /ai-output-styles 用deepthink风格输出以上内容
    ```

---

## License

MIT
