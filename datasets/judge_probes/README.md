# LLM 评审分辨率探针

- `pairwise_pairs.jsonl`：896 对近邻候选及完整答案。
- `pairwise_judgments.jsonl`：21504 个原子判断，保留位置、重复轮次和选择概率。
- `absolute_requests.jsonl` / `absolute_answers.jsonl`：480 个绝对评分请求及 1012 条包含重试的原始评分结果。

成对自评出现 93.75% 平局率和 94.09% 位置翻转率，未通过 CQ1；绝对评分重测较稳定，但其分辨率仍需针对实际 K=8 近邻候选单独验证。统计与方法论分析见 `results/calibration.md`。
