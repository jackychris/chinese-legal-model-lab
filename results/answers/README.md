# 回答正文

这里保存支撑公开结果的模型回答正文。JSONL 每行只保留问题标识、类别、问题、法律上下文、回答、公开模型名和停止原因；回答文本不做人工润色或补全。

- `legal50/`：Gold 对照、九个外部模型 Strict 榜、Base/GRPO/RAFT 的 8192-token 比较，以及 Raw Base 采样补测和 Gold/Distill 贪心鲁棒性探针。
- `instruct/`：引用约束 SFT、DPO 和 rank-64 DPO 的 dev60 输出。
- `structured/`：结构化 confirm120 的 Base/模型 JSON 输出。
- `quantization/`：BF16/AWQ 的自由文本和结构化输出。
- `grounded/`：dev84 的 Base、Strict RAFT、GRPO-200 和 GRPO-600 输出，RAG 行附完整法律上下文。
