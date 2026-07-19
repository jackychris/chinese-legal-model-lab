# 引用约束 SFT / DPO 数据

- `sft_train.jsonl` / `sft_val.jsonl`：203/22 条模仿 SFT 数据。
- `dpo_pairs.jsonl`：119 条 `chosen/rejected` 偏好对。

对应结果位于 `results/instruct_quality.md` 与 `results/answers/instruct/`。模仿 SFT 未减少无据引用；原始 DPO 基本不动，rank-64 高剂量虽移动了行为，但仍未通过质量门。该目录用于复核失败实验，不是推荐训练配方。
