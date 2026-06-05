---
name: as
description: AI 输出风格路由器——定位风格、加载模板、按指定风格渲染输出。不创造内容，只改变呈现方式。
---

# AI Styles (AS)

AS 是一个**输出风格路由器**。它不管内容，只管把内容送进正确的风格流水线。

---

## 1. 触发条件与参数解析

调用方式：

```
/as [style] [template1] [template2] ...
```

### 参数解析流程

```
/as
  → 无参数：扫描 styles/ 展示风格列表，询问用户选哪个

/as [style]
  → 有 style，无 template：
    → 读取 styles/[style]/README.md 获取模板清单
    → 只有1个模板：直接使用，开始渲染
    → 有多个模板：列出模板清单，询问用户选哪个（或选多个）
    → 没有模板：提醒用户该风格无可用模板，结束

/as [style] [template]
  → 有 style + template：直接加载 styles/[style]/templates/[template:需加载文件] 开始渲染

/as [style] [template1] [template2] ...
  → 有 style + 多个 template：依次加载各模板，同一份内容分别渲染
```

### style 匹配规则

| 用户输入               | 匹配方式                     |
|--------------------|--------------------------|
| `deepthink`        | 精确匹配 `styles/deepthink/` |
| `styles/deepthink` | 路径匹配，取最后一段               |
| URL                | 远程获取 README              |

匹配不到 style 时，提示可用的风格列表供用户选择。

---

## 2. 风格注册表与路由

### 注册表

扫描 `styles/` 目录，每个子目录即为一个风格：

```
styles/
├── base/          ← AI Styles 官方风格
├── deepthink/    ← DeepThink 深度分析风格
└── <style>/      ← 未来新增风格
```

### 路由优先级

| 优先级 | 信号   | 解析方式                                     |
|-----|------|------------------------------------------|
| 1   | 精确路径 | 直接读取 `styles/<style>/README.md`          |
| 2   | URL  | 远程获取 README.md                           |
| 3   | 风格名  | 在 `styles/` 下找 `styles/<name>/README.md` |

### 加载流程

```
路由到 styles/<style>/README.md
  → 读取模板清单 + 默认模板
  → 用户指定模板？加载对应 templates/<name>.html/.md
  → 用户未指定？加载 README 中声明的默认模板
  → 模板文件找不到？停止，告知缺哪个文件。不编造样式。
```

需加载文件优先从本地目录找；找不到再用远程 URL（
`https://raw.githubusercontent.com/nest85/ai-styles/main/styles/<style>/`），用到的远程风格下载保存到本地 /styles。

多模板时，同一份内容分别送入每个模板，独立渲染，互不依赖，不共享上下文。

---

## 3. 全局产物规则

### 落盘位置

产物一律写入 `styles/<style>/examples/`。

### 文件命名

```
<内容标题>@<模板名>.<ext>
```

同一内容多模板输出时，各自独立命名，不合并。

### 输出方式

风格化输出本身是文件。消息正文只回复文件路径。

---

## 4. 风格与模板的创建 / 更新

### 新增风格

在 `styles/` 下新建目录，最少需要：

```
styles/<new-style>/
├── README.md          ← 必须包含：风格定位、可用模板清单、默认模板声明
├── templates/         ← 至少一个模板文件
└── examples/          ← 产物落盘目录（建好即可）
```

README.md 中**必须**包含的字段（AI 路由依赖）：

- **可用模板清单**：表格列出模板名、适合场景、需加载文件、渲染依赖
- **默认模板声明**：用户未指定模板时加载哪个

无需修改 `skill/SKILL.md`——路由规则全局通用。

### 新增模板

在 `styles/<style>/templates/` 下新增模板文件。模板头部注释必须包含：

- 组件清单与用法示例
- 渲染机制说明
- ⚠ 保真硬约束
- ⚠ 组件禁忌对照表

然后在 `styles/<style>/README.md` 的模板清单中登记。

### 更新风格 / 模板

修改对应文件即可。注意：

- 同一条规则只在一个地方写完整，其他地方显式引用（"详见 templates/xxx"），不复述出不同版本