# Base — AI Styles 官方风格 · v1.0

> **Base** 是一套以组件化为核心的通用文档风格，适合将任意 AI 输出渲染为结构清晰、排版精良的 HTML页面，及其他格式。

---
## 执行步骤（AI 必须严格按步骤执行，禁止省略）

### 第一步：加载文件
根据所选的模板加载所有需加载文件，完成后打印文件列表；

### 第二步：模板解析
解析模板，充分理解 @as-protocol 渲染规范 和 模板元素，完成后打印出以下内容：
1. 完整展示 @as-protocol 渲染规范；
2. 表格形式展示被标记的元素渲染信息：
   - data-as-lock（列：序号、标记类型、元素名称、渲染规则）；
   - data-as-slot（列：序号、标记类型、元素描述、填充、渲染规则）；
   - data-as-component（列：序号、标记类型、元素描述、布局、适用、禁止、渲染规则）；

### 第三步：渲染内容
严格按照 @as-protocol 渲染规范执行渲染，完成后打印渲染结果；

### 第四步：审计自检
全部渲染完成后，调用 as-check skill 进行自检审计，输出审计报告，完成后打印审计结果；

### 第五步：修复问题
根据第四步审计报告修复问题，如没问题则流程结束，完成后打印任务结果；

---

## 模板列表
| 模板名称    | 适合场景                          | 需加载文件                   | 状态 |
|---------|-------------------------------|-------------------------|----|
| html    |          | templates/as.html       | 可用 |
| html2   |          | templates/as-v2.html    | 可用 |
| html3   |          | templates/as-v3.html    | 可用 |
| html4   | 学术文章                          | templates/as-v4.html    | 可用 |
| aoshtml |          | templates/aos-template.html | 过时 |
| md      |       | templates/as.md         | 废弃 |
| ppt     | 网页 PPT / 演示 / 翻页幻灯片 / 可打印 PDF | templates/ppt.html      | 可用 |

**默认模板：** html

AI注意：
1. 如存在多个模板，而用户未指定模板时，列出选项提醒用户选择。
2. 需加载文件先从本地目录找；
   找不到则用 https://raw.githubusercontent.com/nest85/ai-styles/main/styles/base/ + 需加载文件 查找；
   只要有任一需加载文件最终找不到，就停止生成，并告诉用户缺哪个文件； 不要自行编造样式或用其它风格替代。



## 人类使用方式（AI忽略）

把以下内容复制给 Claude、ChatGPT、Gemini、豆包、千问、Kimi，附上你要美化的内容：

```
读取这套模板 https://raw.githubusercontent.com/nest85/ai-styles/main/styles/base/README.md，如果不能读取或读取失败则停止任务。
 
读取成功后，根据模板提示把以下内容用这套风格渲染：
[你的 AI 输出内容]
```