# Strict RAFT 数据

- `train.jsonl`：639 条最终入选问答。
- `skipped.jsonl`：261 条未获得合格候选的问题及原因。
- `candidate_scores.jsonl`：7374 条候选的确定性引用分、质量分和最终选择分。

对应模型输出和分析位于 `results/grounded_citation.md`、`results/legal50.md`、`results/representative_cases.md` 与 `results/answers/grounded/dev84_raft_strict.jsonl`。Strict RAFT 改善了 no-context 引用和停止率，但牺牲了 RAG 精度，并在 dev84 no-context 切片产生明显语义退化，因此未晋级。
