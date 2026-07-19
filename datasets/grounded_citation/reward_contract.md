# 引用标记与确定性奖励合同

奖励由程序计算，不调用任何模型。每条候选回答得到一个 `[0, 1]` 的标量；GRPO 在同一提示的 8 条候选内做组相对标准化，得到 advantage。

## 三种引用模式

| 模式 | 提供的材料 | 目标 |
|---|---|---|
| `no_context` | 无 | 不自行给出具体法名与条号 |
| `rag_clean` | 相关权威上下文 | 覆盖 required 标记，保持 grounded precision |
| `rag_distractor` | 相关 + 干扰上下文 | 覆盖 required，可用 allowed，拒绝 forbidden |

required 标记必须同时匹配法名与条号；中文数字与阿拉伯数字条号归一为同一编号。

## 计算顺序

```text
1. 静态与 grounding 检查，收集错误标签
2. 若命中任一硬失败标签           → 奖励 0.0
3. 若模式为 no_context 且无硬失败 → 奖励 1.0
4. 否则按 RAG 分级公式计算
```

### 硬失败标签（命中即 0）

`non_eos_or_truncated_completion`、`unsupported_citation`、`invalid_citation_mode`、
`answer_too_short`、`answer_too_long`、`repeated_paragraph`、`question_echo`、
`prompt_or_role_leakage`

`missing_required_citation` 与 `excessive_heading_template` 会被记录，但不置零：前者由下面的公式落到低分区间，后者仅作诊断。

### RAG 分级公式

```text
matched_required  = 命中的 required 标记（要求法名+条号同现）
matched_allowed   = matched_required ∪ 命中的 allowed 标记
extra             = |matched_allowed| − |matched_required|

若 matched_required 非空：
    coverage = |matched_required| / |required|
    core     = 0.5 + 0.5 × coverage
    reward   = core / (1 + 0.15 × extra)

否则若 matched_allowed 非空：
    reward   = 0.2 / (1 + 0.15 × (|matched_allowed| − 1))

否则：
    reward   = 0.0
```

常数：`REQUIRED_COVERAGE_WEIGHT = 0.5`、`EXTRA_ALLOWED_DECAY = 0.15`、`ALLOWED_ONLY_MAX_REWARD = 0.2`。

设计意图：命中任一 required 即进入 `[0.5, 1.0]` 区间，覆盖率决定区间内位置；额外 allowed 引用按数量衰减，使堆砌引用不能提高奖励；只命中 allowed 而未命中 required 的回答封顶 0.2。

### 取值集合

在 K=8 × 900 提示的候选池中，该公式产生的取值为
`0.0, 0.17, 0.20, 0.58, 0.65, 0.69, 0.75, 0.77, 0.87, 1.00`。
可用于核对实现：例如 `0.87 = 1.0 / 1.15`（满覆盖 + 1 个额外 allowed），
`0.65 ≈ 0.75 / 1.15`（覆盖 1/2 + 1 个额外 allowed）。

## 两种模式的奖励形态不同

`no_context` 是二元的（0 或 1），`rag_clean` / `rag_distractor` 是分级的。组相对优势要求组内奖励有方差：当一个提示的 8 条候选在 `no_context` 下全部命中硬失败时，组内奖励全为 0，advantage 为 0，该提示在该步不产生梯度。训练集中约 60% 的 `no_context` 提示组处于这一状态，这是本项目 no-context 指标改善有限的直接原因。

## 边界

该合同评价输入 grounding 与引用行为，不判断法条本身是否现行、真实或适用于个案。
`unsupported` 指引用未由题面或提供的上下文支持。

匹配器对文本表面形式敏感：法名与条号之间插入 Markdown 强调符号、或使用 `〈〉` 嵌套书名号，会导致本应命中的引用未被识别。逐行复核见
[`results/calibration.md`](../../results/calibration.md)。
