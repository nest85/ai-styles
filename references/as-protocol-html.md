# AS HTML 渲染协议 v1

## 1. AS HTML 协议是什么

AS HTML 是一套指导 AI 在人工设计的 HTML 模板内填充内容的渲染协议。

模板由人编写，决定结构、CSS、JS 和视觉效果。AI 只能在被 data-as-element="name" 标记的 HTML Element（AS Element）内，按照同名
@as-element name 注释声明的权限边界修改内容。

data-as-element 是本协议唯一的 HTML 标记。

安全原生 HTML Element = 标准 HTML 中不涉及脚本执行、外部嵌入、表单提交、复杂交互和安全风险的基础内容标签。

---

## 2. 协议总原则

AS 协议采用白名单原则：

> 未声明允许 = 不允许

AI 只能在 AS Element 明确开放的范围内进行修改，其余行为全部禁止。

---

## 3. 注释声明

每个 AS Element 由三部分组成：

* 允许：说明 AI 能做什么（权限边界）
* 适用：说明什么内容应该使用该 Element（正向条件）
* 禁止：说明什么内容不能使用该 Element（反向条件）

示例：

```html
<!-- @as-element callout

允许:
  positions: [as-article, as-section]
  elements
  attributes: [type:[note, warning]]

适用:
  独立成段、需要视觉突出的提示或注意事项。

禁止:
  普通正文；
  真实引用；
  标题。

-->
<div class="callout" type="note" data-as-element="callout">
    <p>这里是一段提示内容。</p>
</div>
```

根据该声明，AI 可知：

* callout 允许放入 as-article 或 as-section
* 内部结构允许修改
* type 仅允许 note 或 warning
* 其他属性不可修改

---

## 4. 允许字段

### 4.1 字段规则

| 写法        | 含义             |
|-----------|----------------|
| 不写字段      | 空集，能力关闭        |
| 只写字段名     | 受控全集，等价于 `[*]` |
| 字段: [...] | 限定集            |

---

### 4.2 集合表达

| 表达              | 含义        |
|-----------------|-----------|
| `[]`            | 空集        |
| `[*]`           | 当前字段的受控全集 |
| `[a, b]`        | 限定集       |
| `[a:[x, y], b]` | 二级限定      |

集合按白名单原则表达，最多两级嵌套。 需要三级时，应拆分为新的 AS Element。

---

### 4.3 通配符

| 通配符      | 含义                  |
|----------|---------------------|
| `as-*`   | 所有已声明 AS Element    |
| `html-*` | 所有安全原生 HTML Element |
| `*`      | 当前字段的受控全集           |

说明：

`*` 不表示任意字符串、任意属性或任意 HTML，仅表示当前字段允许范围内的全集。

---

### 4.4 四个能力字段

| 字段         | 控制                                       |
|------------|------------------------------------------|
| positions  | 该 Element 允许放置的父容器，在合法的父容器内可自由移动、复制、重排数量 |
| elements   | 该 Element 内部允许放置哪些子Element               |
| attributes | 该 Element 可修改的属性及属性值                     |
| text       | 该 Element 是否可修改其纯文本内容                    |

AS Element 的 positions 和父容器的 elements 控制条件必须同时成立才能放置。 HTML Element 作为父容器时只需满足 AS Element 的
positions 控制条件即可。

示例：

```text
positions: [as-article, as-section]
elements: [as-*, html-*]
attributes: [src, alt]
attributes: [type:[note, warning]] 
text
```

---

## 5. Element 选择原则

当多个 AS Element 同时满足适用条件时：

1. 优先满足禁止条件更少的 Element
2. 优先选择语义更具体的 Element

例如：

```text
内容：提示信息，应优先选择：callout，而非：section
```

---

## 6. 渲染流程

```text
INPUT:

- 用户内容
- AS 协议
- HTML 模板

PROCESS:

1. 收集模板中的所有 AS Element。

2. 对每个 AS Element 读取：

   - 允许
   - 适用
   - 禁止

3. 分析用户内容结构。

4. 筛选满足：

   允许 AND 适用 AND NOT 禁止 条件的候选 AS Element。

5. IF 存在候选 AS Element：

   - 按 Element 选择原则排序；
   - 选择优先级最高的 AS Element；
   - 根据允许字段生成 HTML。

6. ELSE：

   - 选择最合适的安全原生 HTML Element；
   - 生成 HTML。

OUTPUT:

- 最终 HTML
```

---

## 7. 渲染规则

1. 未带 data-as-element 的 Element 必须原样保留，不得修改位置、属性、内容或标签。
2. 带 data-as-element 的 Element 只能按允许字段修改。
3. 禁止新增、删除、修改 CSS。
4. 禁止新增、删除、修改 JS。
5. 禁止新增、删除、修改未被 attributes 包含的 Element 的：
    * class
    * style
    * data-*
    * data-as-element
    * on*
6. 禁止使用未声明的 AS Element。
7. 当没有合适 AS Element 时，允许使用安全原生 HTML Element 作为兜底。