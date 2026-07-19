# 公开数据集

| 数据集 | 主要内容 | 行数 |
|---|---|---:|
| `legal50/` | 中文法律咨询题与评分规则 | 50 |
| `audited_gold_sft/` | 审计后咨询 SFT | 86 |
| `structured_intake/` | 结构化 SFT 全集 / confirm120 | 594 / 120 |
| `grounded_citation/` | train900 / dev84 / 权威法条 | 900 / 84 / 2249 |
| `citation_sft_dpo/` | 引用约束 SFT / DPO | 203 / 119 |
| `raft_strict/` | Strict RAFT 入选集 / 候选分数 | 639 / 7374 |
| `quality_rm_failed/` | 质量 RM / 绝对评分教师训练样本 | 597 / 500（完整版 5760） |
| `judge_probes/` | 成对 / 绝对评分探针 | 896 / 480 |

公开文件保留问题、训练正文、权威上下文和冻结标签，不包含服务器路径、来源哈希、API 用量、内部审计回包或历史运行归档。模型回答与统计数据位于仓库 `results/`。
