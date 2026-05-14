# Patent-Drafting Skill for Claude Code

<div align="center">

**AI-Powered Patent Mining & Technical Disclosure Drafting**

从 PRD 中识别专利创新点并撰写技术交底书的 Claude Code Skill。

[![Version](https://img.shields.io/badge/version-V1.0-blue)](./SKILL.md)
[![Author](https://img.shields.io/badge/author-许圣义-green)](https://github.com/jerryxu95)

</div>

---

## Table of Contents / 目录

- [What It Does / 功能概述](#what-it-does--功能概述)
- [Key Features / 核心特色](#key-features--核心特色)
- [Workflow / 工作流](#workflow--工作流)
- [Installation / 安装](#installation--安装)
- [Prerequisites / 前置条件](#prerequisites--前置条件)
- [Usage / 使用示例](#usage--使用示例)
- [Important Notes / 重要提示](#important-notes--重要提示)
- [License / 许可](#license--许可)

---

## What It Does / 功能概述

**English:**

A Claude Code Skill that transforms Product Requirement Documents (PRD) into patent-ready technical disclosures. It helps product managers identify patentable innovations, construct rigorous technical solutions, perform novelty/inventiveness/utility self-reviews, and generate compliant patent diagrams — all without requiring engineer involvement.

**中文：**

这是一个 Claude Code Skill，用于将产品需求文档（PRD）转化为可提交的专利级技术交底书。帮助产品经理从 PRD 中挖掘可专利的创新点，构建严谨的技术方案，进行新颖性/创造性/实用性自审查，并生成符合专利要求的附图 —— 全程无需技术人员参与。

**Core Capabilities / 核心能力：**

| Capability | Description |
|---|---|
| **Patent Point Mining** | 5-dimension scanning (architecture, algorithm, interaction, data, engineering) + 4-layer deep scanning to identify patentable innovations from PRD |
| **Technical Disclosure Drafting** | Structured 10-chapter patent template with incremental chapter-by-chapter writing and self-validation |
| **Novelty Pre-screening** | Generate retrieval prompts for external AI patent databases; parse results and perform differentiation analysis before drafting |
| **Self-Review (Mandatory)** | Spawn an independent sub-agent for novelty review + main agent tri-check (novelty, inventiveness, utility) + template completeness check |
| **Patent Diagram Generation** | Auto-detect Mermaid diagrams in disclosure → convert to Graphviz DOT → export SVG (Visio-editable) + PNG (300 DPI) |

---

## Key Features / 核心特色

### 1. 五维四层扫描体系 / 5D × 4L Scanning Framework

Unlike simple keyword extraction, this Skill performs systematic multi-dimensional scanning:

- **Five Dimensions:** Architecture · Algorithm · Interaction · Data · Engineering
- **Four Layers per Dimension:** Syllogism scan · Combination innovation scan · Alternative comparison scan · Boundary scenario scan

区别于简单的关键词提取，采用系统性多维度扫描：架构、算法、交互、数据、工程五个维度，每个维度执行三段论、组合创新、替代方案对比、边界场景四层扫描。

### 2. 章节化增量撰写 / Incremental Chapter-by-Chapter Writing

The Skill writes the technical disclosure one chapter at a time, saving to file after each chapter with `TodoWrite` progress tracking and self-validation. This prevents context compression from losing critical details across a 10-chapter document.

采用逐章增量撰写，每完成一章立即写入文件并用 TodoWrite 跟踪进度，避免长文档上下文压缩导致细节丢失。

### 3. 子 Agent 独立新颖性审查 / Independent Sub-Agent Novelty Review

A mandatory step where a **separate sub-agent** reviews only the technical solution section (without seeing the main agent's conclusions) to independently assess novelty risk. Results are cross-validated with the main agent's review.

强制流程：独立子 Agent 仅审查技术方案部分（看不到主 Agent 的结论），独立评估新颖性风险，与主 Agent 交叉验证。

### 4. Mermaid → 专利合规附图 / Mermaid to Patent-Compliant Diagrams

Auto-detects ` ```mermaid ` blocks in the disclosure, converts semantics to Graphviz DOT format (white background, black borders, no shadows, Chinese font support), and generates:

- **SVG** — import into Visio for further editing, then export PDF
- **PNG** — 300 DPI, paste directly into documents
- **DOT** — source file for manual adjustment

自动检测 Mermaid 流程图，转换为 Graphviz DOT 格式（白底、黑框、无阴影、中文字体），输出 SVG（Visio 可编辑）、PNG（300 DPI 直接粘贴）、DOT（源文件）。

### 5. 产品经理单人闭环 / Single PM Closed Loop

Designed for product managers to complete the entire patent mining and drafting workflow alone. Technical paths are constructed based on world knowledge — logically rigorous, commonsense-compliant, and provably feasible — without requiring actual implementation or engineer input.

专为产品经理设计，一人完成全流程。技术路径基于世界知识构建，逻辑严谨、符合常识、可做到即可，不要求已落地或技术人员参与。

---

## Workflow / 工作流

```
┌─────────────────────────────────────────────────────────────────┐
│  Scenario 1: PRD Patent Point Mining                            │
│  ├── Read PRD → 5D×4L Scan → Candidate Filter → Tri-check      │
│  └── Save results as local Markdown + Output decision table      │
│                          ↓ User selects a patent point            │
│  Scenario 1A: Novelty Pre-screening (Optional but Recommended)  │
│  ├── Ask if external retrieval available                        │
│  └── If yes: Generate prompts → Parse results → Differentiation │
│                          ↓                                       │
│  Scenario 2: Technical Disclosure Drafting                      │
│  ├── Present 3 building modes → User selects → Build tech path │
│  ├── Save tech path → Incremental chapter writing (10 chapters) │
│  └── User confirms no changes                                   │
│                          ↓                                       │
│  Scenario 3: Mandatory Self-Review                              │
│  ├── Sub-agent independent novelty review                       │
│  ├── Main agent tri-check (novelty / inventiveness / utility)  │
│  └── Template completeness check → Structured modification list │
│                          ↓ (if risks found)                      │
│  Scenario 3A: Supplementary Retrieval & Strengthening           │
│  └── Generate prompts → Parse → Update key points & claims     │
│                          ↓ Disclosure finalized                  │
│  Scenario 4: Patent Diagram Generation (Optional)               │
│  ├── Detect Mermaid → Convert to DOT → Generate SVG/PNG/DOT    │
│  └── Deliver with usage instructions                            │
└─────────────────────────────────────────────────────────────────┘
```

---

## Installation / 安装

In Claude Code, run either of the following:

```bash
# Method 1: Install from repository root (recommended)
claude /skill install https://github.com/jerryxu95/patent-drafting-skill

# Method 2: Install from SKILL.md directly
claude /skill install https://github.com/jerryxu95/patent-drafting-skill/blob/main/SKILL.md
```

---

## Prerequisites / 前置条件

| Requirement | Required? | Description |
|---|---|---|
| **Claude Code** | ✅ Mandatory | This is a Claude Code Skill |
| **WebSearch tool** | ✅ Mandatory | The Skill calls `WebSearch` for novelty assessment and patent retrieval |
| **External AI for patent retrieval** | ⚠️ Strongly Recommended | e.g., Kimi with patent database access. Significantly improves final document quality |
| **Graphviz (`dot`)** | ⚠️ Optional | Required only if you want to generate patent diagrams from Mermaid. Install via `brew install graphviz` (macOS) or winget (Windows) |
| **Chinese fonts** | ⚠️ Optional | For diagram generation. Prefer PingFang SC, fallback SimHei / Microsoft YaHei |

---

## Usage / 使用示例

**Trigger phrases / 触发方式：**

Simply mention any of the following to Claude Code:

> "帮我从这个 PRD 里挖一下专利点"  
> "写一份技术交底书"  
> "专利挖掘"  
> "审查一下这个交底书的新颖性"  
> "把这张流程图转成专利附图"  
> "patent drafting", "technical disclosure", "novelty check"

**Typical session / 典型会话：**

1. Provide your PRD (paste text or file path)
2. The Skill scans and outputs a **decision table** of candidate patent points
3. You select one point to pursue
4. (Optional) Provide external retrieval results for differentiation analysis
5. Choose a building mode (A: multi-candidate / B: single optimal / C: direction-first)
6. Confirm the technical path → Skill drafts the 10-chapter disclosure incrementally
7. After your confirmation, mandatory self-review runs automatically
8. (Optional) Generate patent-compliant diagrams from any Mermaid blocks

---

## Important Notes / 重要提示

### ⚠️ What This Skill Is NOT / 非本 Skill 的设计目标

1. **Not a patent agent** — It drafts technical disclosures, not legal patent applications. Claims language and legal formatting require a professional patent attorney.
2. **Not a replacement for professional novelty search** — The built-in WebSearch + world knowledge tri-check is a preliminary filter. For high-stakes filings, engage a professional patent search firm.
3. **Not guaranteed grant** — Patent grant depends on examiner judgment, prior art landscape, and claim drafting quality. This Skill improves your *input quality* to the patent system, not the outcome.

1. **不是专利代理人** — 本 Skill 产出的是技术交底书，不是法律意义上的专利申请文件。权利要求书的法律措辞和格式需要专业专利代理人处理。
2. **不能替代专业查新** — 内置的 WebSearch + 世界知识三性初判只是前置过滤。对于高价值申请，仍需委托专业查新机构。
3. **不保证授权** — 专利授权取决于审查员判断、现有技术格局和权利要求撰写质量。本 Skill 提升的是输入质量，而非结果确定性。

### 📋 Best Practices / 最佳实践

- **Always do external retrieval if possible** — The Skill explicitly asks whether you have access to an external AI with patent database (e.g., Kimi). Saying "yes" and providing results dramatically improves novelty analysis and differentiation positioning.
- **Review the technical path before chapter writing** — The Skill presents the full technical path for your confirmation before drafting. Catch architectural errors here, not after 10 chapters are written.
- **Iterate on the self-review output** — The mandatory self-review produces structured, machine-consumable modification instructions. Don't skip them.

- **尽可能使用外部查新** — Skill 会主动询问是否有带专利知识库的 AI 助手（如 Kimi）。回答"有"并提供查新结果，新颖性分析和差异化定位质量会大幅提升。
- **在技术路径确认环节仔细审查** — Skill 会在撰写前呈现完整技术路径供确认。架构层面的错误在此阶段修正，远优于写完 10 章后再改。
- **认真对待自审查输出** — 强制自审查会产出结构化的、机器可消费的修改指令，不要跳过。

### 🛠️ Troubleshooting / 常见问题

**Q: 推送后报错 `gh: Recv failure: Connection reset by peer`**  
A: 你的网络环境可能需要代理。在命令前加上代理环境变量：`export HTTP_PROXY=http://用户的代理端口`

**Q: 附图生成时报错 `dot: command not found`**  
A: 未安装 Graphviz。macOS: `brew install graphviz`；Windows: `winget install graphviz`；或从官网下载安装。

**Q: 生成的附图中文显示为方框**  
A: 系统缺少中文字体。macOS 通常自带 PingFang SC 无需处理；Windows/Linux 请安装 SimHei 或 Microsoft YaHei。

---

## Repository Structure / 仓库结构

```
patent-drafting-skill/
├── README.md                          # This file
├── SKILL.md                           # Skill definition (main entry)
├── evals/
│   └── evals.json                     # Evaluation configuration
└── references/
    ├── methodology.md                 # 5D×4L scanning methodology
    ├── retrieval-prompt-template.md   # Prompt template for external patent retrieval
    ├── template-guide.md              # Disclosure template guide & checklist
    └── diagram-generation.md          # Diagram generation spec & font troubleshooting
```

---

## License / 许可

MIT License — feel free to use, modify, and distribute. Attribution appreciated.

---

<div align="center">

Built with care for product managers who want to own their IP.

为希望掌握自己知识产权的产品经理精心打造。

</div>
