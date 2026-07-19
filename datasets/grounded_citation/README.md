# 权威上下文与引用学习数据

- `train900.jsonl`：900 条训练问题。
- `dev84.jsonl`：84 条统一开发集，其中 42 条 no-context、30 条 rag-clean、12 条 rag-distractor。
- `authority_articles.jsonl`：2249 条按法条切分的权威上下文。
- `authority_documents.jsonl`：13 份法源文档元数据。
- `reward_contract.md`：required/allowed/forbidden 与确定性奖励边界。

对应 Base/RAFT/GRPO 回答与统计位于 `results/answers/grounded/`、`results/grounded_citation.md` 和 `results/grpo.md`。
