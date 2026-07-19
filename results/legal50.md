# Legal-50：语义质量与引用行为

Legal-50 包含 50 道中文情境式法律咨询题。它在项目开发中被多次使用，因此属于程序级标尺，不是未见盲测集。

## 历史阳性锚点

Audited Gold SFT 在历史成对评审场中将语义均分从 `6.81` 提升到 `7.33`，结果为 29 胜、11 平、10 负。该刻度包含当时的封顶规则，不能与后面的 Strict 或 uncapped 分数直接相减。

## 历史内部谱系统一重评

下表来自 2026-07-19 的同场统一重评，但答案生成于不同历史协议。分数均为 uncapped 六维组件总和，用于展示内部谱系的大尺度变化，不用于精确归因。

| 模型 | 语义均分 | unsafe 行 | 历史生成协议（temperature / top_p / max_tokens） |
|---|---:|---:|---|
| Qwen3-8B Raw Base | 5.946 | 10/50 | `0.2 / 0.9 / 2048` |
| Qwen3-8B Instruct | 6.784 | 7/50 | `0.2 / 0.9 / 1024` |
| Gold V2 | 7.430 | 5/50 | `0.2 / 0.9 / 2048` |
| Distill V3 | 8.122 | 2/50 | `0.2 / 0.9 / 2048` |

## 外部模型 Strict 语义榜

五个曾受短输出协议影响的模型使用 8192-token 自然停止重跑答案；四个模型使用经复核完整的历史存档。评分明确枚举六维组件、封顶原因和严重错误类别。

| 排名 | 模型 | Strict 均分 | 答案资产 | 实际封顶行 |
|---:|---|---:|---|---:|
| 1 | `z-ai/glm-5.2` | 9.428 | legacy_complete_archive | 2 |
| 2 | `anthropic/claude-fable-5` | 9.322 | legacy_complete_archive | 1 |
| 3 | `moonshotai/kimi-k2.6` | 9.282 | long_budget_retest_8192 | 1 |
| 4 | `bailian/qwen3.7-max` | 9.250 | long_budget_retest_8192 | 5 |
| 5 | `openai/gpt-5.5` | 9.090 | long_budget_retest_8192 | 5 |
| 6 | `deepseek/deepseek-v4-pro` | 9.066 | legacy_complete_archive | 6 |
| 7 | `anthropic/claude-opus-4.8` | 8.846 | legacy_complete_archive | 9 |
| 8 | `google/gemini-2.5-pro` | 8.700 | long_budget_retest_8192 | 7 |
| 9 | `anthropic/claude-sonnet-5` | 8.446 | long_budget_retest_8192 | 16 |

相邻约 `0.4` 分以内的差距不解释为可靠能力差异。评审来自 DeepSeek 模型家族，存在自族和风格偏好风险。本表是 Legal-50 协议内的描述性坐标，不是通用模型排行榜。

## 外部模型确定性引用普查

所有外部回答均在明确要求不要编造具体法条编号的 system prompt 下生成。排序依次按高精度无据法名+条号、无据条号、全部无据引用升序。

| 排名 | 模型 | 高精度无据法名+条号 | 无据条号 | 全部无据引用 |
|---:|---|---:|---:|---:|
| 1 | `openai/gpt-5.5` | 0/50 | 0/50 | 0/50 |
| 2 | `anthropic/claude-fable-5` | 0/50 | 0/50 | 2/50 |
| 3 | `anthropic/claude-opus-4.8` | 0/50 | 0/50 | 6/50 |
| 4 | `anthropic/claude-sonnet-5` | 0/50 | 1/50 | 6/50 |
| 5 | `bailian/qwen3.7-max` | 0/50 | 1/50 | 17/50 |
| 6 | `moonshotai/kimi-k2.6` | 0/50 | 2/50 | 19/50 |
| 7 | `google/gemini-2.5-pro` | 0/50 | 2/50 | 25/50 |
| 8 | `z-ai/glm-5.2` | 5/50 | 6/50 | 21/50 |
| 9 | `deepseek/deepseek-v4-pro` | 11/50 | 14/50 | 28/50 |

“无据”表示引用未由题面提供，不等于法条虚假、失效或适用错误。该表衡量题面 grounding 与指令服从，不替代法源核验。

## 内部模型 8192-token 剂量比较

四组内部答案均为 50/50 自然停止。由于本批 cap schema 欠定义，只报告 uncapped 六维组件总和，不与上面的 Strict 榜混排。

| 模型 | 语义均分 | 相对 Base | 配对 bootstrap 95% CI | unsafe 行 |
|---|---:|---:|---|---:|
| Base | 6.688 | - | - | 13 |
| GRPO-200 | 6.686 | -0.002 | [-0.512, 0.502] | 12 |
| GRPO-600 | 7.080 | +0.392 | [-0.036, 0.842] | 14 |
| Strict RAFT | 6.604 | -0.084 | [-0.502, 0.318] | 11 |

GRPO-200 与 Base 几乎相同；GRPO-600 点估计为 `+0.392`，但区间仍跨 0；Strict RAFT 在 Legal-50 上也没有可辨别差异。长预算修复了截断混杂，没有把阴性结果改写成阳性结果。

## Raw Base 长预算补测与早期模型解码鲁棒性

Raw Base 使用 `temperature=0.2, top_p=0.9, max_tokens=8192, seed=20260720` 重新生成，50/50 正常停止。独立统一评审的 uncapped 均分为 `5.148`，unsafe 为 `10/50`。它与历史 Raw Base 的 `5.946` 来自不同答案样本和评审批次，不把差值解释为模型退化。

确定性引用分析显示：Raw Base 有 `33/50` 行输出法律引用；这些引用均未由题面提供，其中 `23/50` 行包含无据法名+条号。“无据”不等于法条虚假或适用错误，但说明静态模型在无 RAG 题面下会主动生成无法由输入核验的法律引用。

同一轮还用贪心解码测试了早期 LoRA：Gold V2 为 `39/50 stop、11/50 length`；Distill V3 为 `1/50 stop、49/50 length`。截断样本出现重复词和模板循环，因此这两路只作为**模型与贪心解码交互退化**证据，不进入语义榜。

## 边界

- 外部 Strict、内部 uncapped 和历史 Gold 分别属于三种评分场，禁止合并排序。
- 历史内部谱系、Raw Base 长预算补测和贪心鲁棒性探针也属于不同生成场，不合并为严格同协议排行。
- 每个 arm 只有 50 题，语义评审只适合粗粒度比较和灾难检测。
- GPT-5.4 Pro 仅完成 21/50，不进入榜单。
