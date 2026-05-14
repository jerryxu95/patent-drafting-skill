# 专利附图生成规范

## 专利附图合规要求

- 黑白线图，无彩色、无灰色填充、无阴影、无渐变
- 背景纯白，边框和文字纯黑
- 部件须标注名称或编号
- 流程图用标准框图（矩形、菱形、椭圆）
- 线型清晰，连接关系明确

## Mermaid → DOT 转换方法

Mermaid 代码块不能直接渲染为专利合规附图，需转换为 Graphviz DOT 后生成。

### 转换步骤

1. **读取 Mermaid**：从交底书中提取 ` ```mermaid ... ``` ` 代码块
2. **语义理解**：理解图表结构、节点关系、布局方向
3. **重写为 DOT**：用 DOT 语言重新描述相同结构的图
4. **样式设置**：确保符合专利附图规范
5. **生成图片**：调用 graphviz 生成 SVG 和 PNG

### DOT 样式模板

```dot
digraph 图标题 {
    graph [rankdir=LR, bgcolor=white, fontname="PingFang SC", fontsize=12, margin=0, nodesep=0.4, ranksep=0.5];
    node [shape=box, style="filled", fillcolor=white, color=black, fontcolor=black, fontname="PingFang SC", fontsize=10];
    edge [color=black, fontcolor=black, fontname="PingFang SC", fontsize=9];

    // 开始/结束节点用双椭圆
    Start [label="开始节点", shape=ellipse, peripheries=2];
    // 判断节点用菱形
    Decision [label="判断条件", shape=diamond];
    // 普通处理节点用矩形
    Process [label="处理步骤"];

    Start -> Process -> Decision;
}
```

### 关键参数

| 参数 | 作用 | 专利合规值 |
|---|---|---|
| `bgcolor=white` | 背景色 | 纯白 |
| `fillcolor=white` | 节点填充 | 纯白 |
| `color=black` | 边框色 | 纯黑 |
| `fontcolor=black` | 文字色 | 纯黑 |
| `shape=box` | 普通节点 | 矩形 |
| `shape=diamond` | 判断节点 | 菱形 |
| `shape=ellipse, peripheries=2` | 开始/结束 | 双椭圆 |
| `style=dashed` | 特殊数据流 | 可用于标注非主流程 |

## 环境适配

执行前检查 `dot` 命令是否可用。根据操作系统选择安装方式。

### macOS

**安装 Graphviz：**
```bash
brew install graphviz
```

**检测中文字体：**
```bash
fc-list :lang=zh | grep -i "pingfang\|heiti\|yahei\|simsun"
```

优先使用 PingFang SC。若 fc-list 未返回结果，尝试直接用字体名称（DOT 中 `fontname="PingFang SC"`），Graphviz 会自动查找系统字体。

**生成命令：**
```bash
dot -Tsvg -o 附图.svg 附图.dot
dot -Tpng -o 附图.png -Gdpi=300 附图.dot
```

### Windows

**安装 Graphviz：**
- 官网下载安装包：https://graphviz.org/download/
- 或使用 `winget install graphviz`
- 安装后将 `bin` 目录加入 PATH

**检测中文字体：**
```powershell
fc-list :lang=zh
```

优先使用 SimHei 或 Microsoft YaHei。DOT 中指定 `fontname="SimHei"` 或 `fontname="Microsoft YaHei"`。

**生成命令与 macOS 相同。**

### Linux

**安装 Graphviz：**
```bash
apt-get install graphviz   # Debian/Ubuntu
yum install graphviz       # RHEL/CentOS
```

中文字体需确保系统已安装（如文泉驿、Noto Sans CJK）。

## 输出文件组织

与交底书同目录创建 `附图/` 文件夹：

```
技术交底书.md
附图/
├── 附图1-系统架构图.svg
├── 附图1-系统架构图.png
├── 附图1-系统架构图.dot
├── 附图2-数据流程图.svg
├── 附图2-数据流程图.png
└── 附图2-数据流程图.dot
```

## 交付说明

向用户说明三种文件的用途：

- **DOT**：源文件，可随时重新生成
- **SVG**：矢量格式，可导入 Visio / Adobe Illustrator 编辑，可导出 PDF
- **PNG**：位图格式，300 DPI，可直接粘贴到 Word / Markdown / 在线文档中

## 常见问题

**Graphviz 未安装**
- 提示用户按上述操作系统对应方式安装，不自动执行安装命令

**中文字体渲染异常**
- 检查系统是否安装中文字体
- 尝试更换字体名称（如 PingFang SC → SimHei → Microsoft YaHei）
- 若所有中文字体均不可用，用英文标注节点，在图注中补充中文说明

**Mermaid 复杂图（如时序图、类图）**
- 时序图：用 DOT 的 rank 机制模拟时间轴，节点按时间顺序排列
- 类图：用矩形节点表示类，用边和标签表示继承/关联关系
- 核心原则：保持语义一致，布局可适当调整以符合专利附图规范
