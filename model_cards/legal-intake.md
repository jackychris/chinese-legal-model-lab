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
- information-extraction
- structured-output
---

# Qwen3-8B LegalIntake

面向中文法律咨询 Intake 的 rank-64 LoRA，用于案件类型识别以及金额、时间要素的固定 JSON 抽取。

## 冻结评测结果

在新冻结的 120 题 confirmatory 集上：

| 指标 | Base | 本模型 |
|---|---:|---:|
| JSON 有效率 | 1.0000 | 1.0000 |
| 案件类型准确率 | 1.0000 | 1.0000 |
| 整体 micro-F1 | 0.6031 | 0.7018 |
| 金额 F1 | 0.6077 | 0.8936 |
| 时间 F1 | 0.6000 | 0.6159 |
| 无依据抽取项 | 14 | 2 |

模型最终判定为 `ARCHIVED_FINAL_FAIL`：时间字段和负样本精确匹配没有通过预注册门禁。该 checkpoint 被公开是因为它承载了明确的确定性任务阳性结果，而不是因为已经达到部署标准。

## 加载

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

adapter = "haofue2i1z3/Qwen3-8B-LegalIntake"
base = "Qwen/Qwen3-8B"

tokenizer = AutoTokenizer.from_pretrained(adapter, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(base, device_map="auto", torch_dtype="auto")
model = PeftModel.from_pretrained(model, adapter)
```

## 限制

- 输出必须经过 JSON Schema 和输入依据校验。
- 时间表达仍存在过度抽取问题。
- 仅供研究与作品展示，不提供法律服务。
