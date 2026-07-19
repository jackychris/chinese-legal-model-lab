---
language:
- zh
license: apache-2.0
base_model: Qwen/Qwen3-8B
library_name: peft
pipeline_tag: text-generation
tags:
- legal
- chinese
- lora
- grpo
- rag
---

# Qwen3-8B LegalCitation GRPO 200

基于 `Qwen/Qwen3-8B` 的 rank-64 GRPO LoRA。训练奖励只使用可确定验证的法条引用信号，覆盖 `no_context`、`rag_clean` 和 `rag_distractor` 三种模式。

## dev84 结果

| 指标 | Base | GRPO-200 |
|---|---:|---:|
| no-context 无依据引用行 | 40/42 | 36/42 |
| RAG grounded citation precision | 0.7703 | 0.7874 |
| RAG required 行召回率 | 0.8333 | 0.9286 |
| RAG required marker coverage | 0.6765 | 0.7794 |
| 正常停止行 | 41/84 | 49/84 |
| 语义均分变化 | - | -0.039 |

模型最终判定为 `FAIL`：引用精度、no-context 无依据引用、停止与重复等硬门禁仍未全部通过。它是行为移动研究 checkpoint，不是部署就绪模型。

## 加载

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

adapter = "haofue2i1z3/Qwen3-8B-LegalCitation-GRPO-200"
base = "Qwen/Qwen3-8B"

tokenizer = AutoTokenizer.from_pretrained(adapter, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(base, device_map="auto", torch_dtype="auto")
model = PeftModel.from_pretrained(model, adapter)
```

## 限制

- 无 RAG 上下文时仍可能生成无依据法条。
- 强化学习移动的是输出分布期望，不能替代运行时引用校验。
- 仅供研究与作品展示，不提供法律服务。
