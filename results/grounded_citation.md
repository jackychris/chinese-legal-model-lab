# 权威上下文与引用行为

训练与评测数据把引用分成 `no_context`、`rag_clean` 和 `rag_distractor`。RAG 行记录 required、allowed 和 forbidden 引用标记，使覆盖率与精度可以由确定性程序计算。

| 方法 | no-context 无据行 | RAG 精度 | required 行召回 | 正常停止 | 重复 | 语义变化 |
|---|---:|---:|---:|---:|---:|---:|
| Base | 40/42 | 0.7703 | 35/42 | 41/84 | 9/84 | - |
| Strict RAFT | 34/42 | 0.7330 | 36/42 | 57/84 | 11/84 | -0.4310 |
| GRPO-200 | 36/42 | 0.7874 | 39/42 | 49/84 | 12/84 | -0.0393 |
| GRPO-600 | 39/42 | 0.7736 | 37/42 | 46/84 | 14/84 | +0.0139 |

## 方法判读

- Strict RAFT 把 no-context 无据行从 40 降到 34、正常停止从 41 提到 57，但 RAG 精度从 0.7703 降到 0.7330，dev84 no-context 语义分下降 0.431。
- GRPO-200 把 required 行召回从 35 提到 39，并且是唯一让 RAG 精度上升的方法；整体语义变化仅 -0.039。
- GRPO-600 回吐了 200 步的大部分 no-context 和精度改善，重复内容继续增加。

没有任何 arm 通过全部预注册门禁。结论是“行为发生可测移动”，不是“模型已可部署”。[Strict RAFT 案例](cases/04-quantization-and-raft.md)及[GRPO 案例](cases/05-grpo.md)。
