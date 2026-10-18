# make-markdown-mindmap

把文章整理成一张可复习、可归档的 Markdown 思维导图。输入粘贴文本或本地 Markdown/TXT 文件，输出单根节点、缩进清晰的层级列表。

## 1. 项目解决什么问题

长文章里，核心观点、支撑证据、背景信息、重复说明和例子往往混在一起。人工整理思维导图耗时；普通摘要又常常只剩段落大意，丢掉层级关系，甚至加入原文没有的信息。

本项目把“读文章 → 找核心主题 → 分主次 → 去重 → 输出层级”固化成一套可复用的 Skill 规则：

- 从原文提炼核心主题和支撑关系；
- 按重要性压缩，而不是逐句改写；
- 每个节点都能追溯到原文；
- 不补充原文之外的事实、定义、案例或结论；
- 输出可直接复制、保存的纯 Markdown 文本。

它适合学习笔记、文章拆解、汇报提纲和知识归档，不负责网页抓取、PDF 解析或图形绘制。

## 2. 主要功能

### 输入

- 直接粘贴的文章正文；
- 本地 `.md`、`.markdown`、`.txt` 文件。

### 输出

- 一个顶层根节点，代表文章核心主题；
- 使用 `- ` 无序列表和每层 2 个空格缩进；
- 通常为 3–7 层；
- 默认直接返回思维导图正文，不加分析过程或客套话；
- 输出语言默认跟随原文，专有名词和技术术语保留原样。

### 内容处理

- **提炼重点**：根节点概括核心主题，子节点补充机制、证据、条件、例子或结论。
- **保持层级逻辑**：父节点负责概括，子节点补充细节；同级节点保持同一分类逻辑。
- **合并重复内容**：同一观点只保留一次，放在信息最完整的位置。
- **忠于原文**：保留数字、日期、条件、例外、归因和不确定性；不把“可能”写成“一定”。
- **重点概念简短解释**：只使用原文提供的解释；原文没有解释时，不自行补写外部定义。
- **信息不足不凑层级**：如果原文确实支撑不了 3 层，会保留真实层级，并在图后注明实际层数。

### 暂不支持

- 网页抓取、URL 读取；
- PDF、Office 文档、图片或扫描件；
- 图形界面；
- 图片、Mermaid 或其他图形格式导出。

遇到不支持的格式，Skill 会提示改用粘贴文本或 Markdown/TXT 文件。

## 3. 安装方法

本项目是一个 Codex Skill，使用它不需要额外脚本、第三方运行依赖或构建步骤。核心文件只有：

```text
make-markdown-mindmap/
├── SKILL.md
└── agents/
    └── openai.yaml
```

完整规则见 [SKILL.md](skills/make-markdown-mindmap/SKILL.md)，界面元数据见 [openai.yaml](skills/make-markdown-mindmap/agents/openai.yaml)。

### 安装到 Codex

先获取本项目目录，再把 `skills/make-markdown-mindmap` 整个文件夹复制到 Codex 的 skills 目录。以下命令均在项目根目录执行。

macOS / Linux：

```bash
mkdir -p ~/.codex/skills
cp -R skills/make-markdown-mindmap ~/.codex/skills/
```

Windows PowerShell：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
Copy-Item -Recurse -Force ".\skills\make-markdown-mindmap" "$env:USERPROFILE\.codex\skills\"
```

如果设置了 `CODEX_HOME`，请改为复制到 `$CODEX_HOME/skills/`。

安装完成后，目录应类似：

```text
~/.codex/skills/make-markdown-mindmap/
├── SKILL.md
└── agents/
    └── openai.yaml
```

重新打开会话或刷新技能列表，让客户端重新发现该 Skill。

> 使用其他支持 `SKILL.md` 技能的客户端时，将 `make-markdown-mindmap` 文件夹放入该客户端约定的 skills 目录即可。`agents/openai.yaml` 只影响读取该配置的客户端。

## 4. 使用方法

### 转换粘贴的文章

```text
使用 $make-markdown-mindmap 将下面文章整理成 Markdown 思维导图：

<粘贴文章正文>
```

### 转换本地文件

```text
使用 $make-markdown-mindmap 转换 D:\notes\article.md
```

本地文件路径需要对运行环境可读。支持 `.md`、`.markdown` 和 `.txt`。

### 直接保存结果

```text
使用 $make-markdown-mindmap 转换 D:\notes\article.md，并保存到 D:\notes\article.mindmap.md
```

Skill 会把思维导图写入指定的 `.md` 或 `.txt` 文件，并报告保存路径。

### 使用说明

- 默认一次处理一篇文章；提供多篇材料时，会先确认处理方式。
- 默认直接返回思维导图列表；如果希望放进代码块方便复制，可以明确要求“用代码块输出”。
- 如果客户端不支持 `$skill-name` 语法，可以直接说“把下面文章整理成 Markdown 思维导图”，再粘贴正文。

## 5. 输入输出示例

### 输入

```text
社区花园不仅提供休闲空间，也能缓解城市热岛。植物通过蒸腾消耗热量，树冠还能遮挡阳光。研究显示，连续分布的绿地比零散绿地降温更明显，但效果受面积、灌溉和周边建筑密度影响。因此，规划时应优先连接现有绿地，并为灌溉和维护预留资源。
```

### 输出

下面的代码块仅用于展示 Markdown 文本；实际使用时 Skill 默认直接返回列表，不套代码围栏。

```markdown
- 社区花园兼顾休闲与缓解城市热岛
  - 降温机制
    - 植物蒸腾消耗热量
    - 树冠遮挡阳光
  - 效果与条件
    - 研究显示：连续分布的绿地比零散绿地降温更明显
    - 效果受面积、灌溉和周边建筑密度影响
  - 规划启示
    - 优先连接现有绿地
    - 为灌溉和维护预留资源
```

这个示例共 3 层：根节点给出核心判断，第二层分为机制、条件、规划启示，第三层补充具体信息。典型文章会按内容自然展开到 3–7 层；原文信息确实不足时，不会为了达到 3 层而编造内容。

## 验证

已通过官方 `skill-creator` 的 `quick_validate.py` 结构校验，并使用科普说明文、新闻评论、英文操作指南和短通知完成本地测试。

## License

本项目采用 [MIT License](LICENSE) 授权。
