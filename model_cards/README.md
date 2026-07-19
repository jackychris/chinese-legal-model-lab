# 模型卡

本目录保存已发布模型在 Hugging Face 上使用的模型卡副本。模型权重不重复存入本仓库，加载地址以各模型卡中的个人 Hugging Face 仓库为准。

| 模型 | 用途 | 实验状态 |
|---|---|---|
| `Qwen3-8B-LegalConsult-Gold` | 自由文本法律咨询 | 阶段阳性 |
| `Qwen3-8B-LegalIntake` | 结构化案件信息抽取 | 未通过全部门禁 |
| `Qwen3-8B-LegalCitation-GRPO-200` | 有依据的法条引用 | 未通过全部门禁 |
| `Qwen3-8B-LegalCitation-GRPO-600` | GRPO 剂量响应终点 | 未通过全部门禁 |
| `Qwen3-8B-Legal-AWQ` | V24 融合后 W4A16 量化 | 后验量化资产，未通过全部门禁 |

未通过门禁的 checkpoint 被保留，是因为它们承载了可复核的实验结论，不表示适合直接部署。
