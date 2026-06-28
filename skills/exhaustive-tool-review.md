---
name: exhaustive-tool-review
description: >
  Prevents the AI from concluding before exhaustively reviewing ALL available tools.
  Forces a systematic tool scan before delivering final answers. Addresses the
  "Knowing-Doing Gap" (Cheng et al., 2026) — agents often know they should use
  tools but fail to translate that into action. Trigger: any non-trivial task
  where additional tool calls could improve answer quality.
version: 1.0.0
---

# Exhaustive Tool Review

> 核心原则：**不要看完两三个工具就下结论。把所有工具扫一遍，多想一步"还有哪个工具能帮上忙"。**

## 触发条件

以下任一情况，必须执行本流程：
- 任务涉及事实信息（价格/时间/地址/数据/研究）
- 回答需要外部知识而非仅凭推理
- 用户问了"为什么""怎么回事""有没有研究""怎么解决"等开放性问题
- 任务 ≥3 步，可能遗漏中间环节
- 你发现自己只用了一两个工具就准备给出最终答案

## 三步扫描法

### Step 1 — 全量工具盘点（每次任务开始时）

在开始任务前，快速扫一遍所有可用工具，在脑中分类：

```
📂 搜索类: web_search, mcp_anysearch_search, mcp_anysearch_extract, arxiv, perplexity...
📂 文件类: read_file, write_file, search_files, patch...
📂 执行类: terminal, execute_code, delegate_task...
📂 媒体类: vision_analyze, text_to_speech...
📂 记忆类: memory, session_search, skill_view...
📂 协作类: send_message, clarify, cronjob...
📂 验证类: verify_claims, cross-verify-search skill...
```

对当前任务，**逐类问自己**："这一类里有工具能帮我做得更好吗？"

### Step 2 — 强制二次检查（回答前）

在准备输出最终答案之前，执行这个检查：

```
⛔ 停！先回答三个问题：

1. 我刚才用了几个工具？____个
   → 如果 ≤2 且任务不是 trivial → 很可能用少了

2. 有没有一个工具我看到了但没试？
   → 列出它：________
   → 为什么没试？如果理由是"应该不需要"→ 去试一下

3. 如果我多用一个工具，结果会变好吗？
   → 具体哪个工具？________
   → 如果不确定 → 去试一下
```

### Step 3 — 输出前确认

最终回答前，确认：
- [ ] 对所有相关工具类别至少考虑过
- [ ] 对"可能有用但没试"的工具有明确跳过理由（不是"应该不需要"）
- [ ] 事实性主张有 ≥2 个来源（如果适用）

## 何时可以跳过

以下情况不需要全量扫描：
- 纯闲聊/问候
- 用户明确说"不要搜/不要用工具"
- 任务只需要单一工具且结果已完备（如"读这个文件"）
- 紧急场景（路上点菜/找路/快没电）

## 常见遗漏模式（反模式）

| 模式 | 表现 | 修正 |
|:--|:--|:--|
| **搜索即止** | 搜了一次就回答，没提取全文 | 搜到链接 → extract 全文 → 对照检查 |
| **只看标题** | 看了搜索结果标题就下结论 | 必须点进去看内容 |
| **单源定论** | 一个来源说了就信 | ≥2 独立来源交叉验证 |
| **工具盲区** | 用 web_search 查学术问题，不知道有 arxiv | 盘点时扫全量工具 |
| **文件遗忘** | 有 read_file/write_file 但全程没用 | 问自己：有没有文件可以读/写？ |
| **验证缺失** | 给出了事实数据但没调用验证工具 | 用 verify_claims 或手动交叉搜索 |
| **记忆遗忘** | 之前存过相关信息但没查 memory/session_search | 先查历史再搜新东西 |
| **skill 遗忘** | 相关 skill 就在 available_skills 里但没加载 | 扫描 skill 列表，加载相关项 |

## 与现有 skill 的关系

- **fact-check**: 专注于事实主张的逐条验证。本 skill 是更上层的"工具使用纪律"——确保你不会连验证工具都忘了用。
- **cross-verify-search**: 专注于搜索结果的交叉验证。本 skill 确保你连搜索这一步都不会跳过。
- **hermes-agent**: 了解可用工具本身。本 skill 确保你实际使用它们。

## 学术背景

本 skill 基于以下研究发现：

1. **Knowing-Doing Gap** (Cheng et al., UMD, 2026, arXiv 2605.14038):
   LLM 的内部表征能线性解码出"该用工具"的认知，但认知向量和执行向量在末层几乎正交。
   模型知道该用工具，但无法把认知转化为行动。**26.5%-54.0% 的 necessity-action mismatch。**

2. **Temporal Blindness** (Cheng et al., UMD, 2026, arXiv 2510.23853):
   LLM agent 在多轮交互中忽略时间流逝，要么过度依赖过时上下文（不调用工具），
   要么冗余重复调用工具。**最佳对齐率不到 65%。**

3. **Tool Selection Depth** (Repantis et al., Meta, 2026, arXiv 2605.24660):
   展示工具太少 → 找不到正确工具；展示太多 → 选择准确率下降。
   自适应深度比固定 K 值提高下游选择准确率 6pp（93.1% vs 87.1%）。

4. **Tool Description Engineering** (Anthropic, 2025; Adaline Labs, 2026):
   工具描述是模型选择的第一信号，比 system prompt 重要得多。
   描述重叠是选择失败的主要原因。

5. **Context Engineering** (Anthropic, 2025):
   LLM 有"注意力预算"，上下文越长越容易丢失中间信息。
   工具列表本身消耗注意力——找到最少高信号 token 是关键。

## Pitfalls

- **不要只扫不看**：盘点工具不是为了走过场，是为了发现遗漏。扫到"可能有用"的就去试。
- **不要用"应该够了"自我说服**：如果你在说服自己"应该够了"，那就是不够。
- **不要把工具盘点当成一次性任务**：任务中途发现新方向时，重新盘点——新方向可能需要新工具。
- **本 skill 不替代专业 skill**：如果你发现相关专业 skill（如 fact-check、cross-verify-search），加载它们，不要用本 skill 的通用流程替代。
