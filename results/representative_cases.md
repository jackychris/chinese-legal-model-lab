# 代表性版本优化与劣化案例

本报告为公开展示阶段各选一组优化与劣化/副作用案例。单例用于解释总体统计，不替代总体门禁。Markdown 与同名 JSON 均保留完整输入上下文和回答正文；页面使用可展开区块控制阅读长度，不截断内容。
部分正文会在原始输出末尾戛然而止，这是生成时触及 1024 token 上限的真实结果；此类案例已在元数据中标注，文档不补写模型未生成的内容。选例规则逐节披露：机械极值与人工固定的机制代表例均不等同于平均样本。

## 分阶段案例

| 文件 | 包含阶段 |
|---|---|
| [咨询 SFT](cases/01-consultation-sft.md) | 审计后 Gold SFT；引用约束 SFT（V17） |
| [偏好训练](cases/02-preference-training.md) | 引用偏好 DPO（V18）；rank-64 高剂量偏好训练 |
| [结构化抽取](cases/03-structured-extraction.md) | 结构化抽取宪法（V23）；时间精度修正（V24） |
| [量化与 Strict RAFT](cases/04-quantization-and-raft.md) | V24 BF16 到 AWQ 的量化干预；Strict RAFT |
| [GRPO](cases/05-grpo.md) | GRPO 200 步；GRPO 600 步 |

机器可读完整数据见 `metrics/representative_cases.json`。
