---
language:
- zh
license: apache-2.0
base_model: Qwen/Qwen3-8B-Base
library_name: peft
pipeline_tag: text-generation
tags:
- legal
- chinese
- lora
- qlora
---

# Qwen3-8B LegalConsult Gold

基于 `Qwen/Qwen3-8B-Base` 的中文法律咨询 LoRA。模型使用 86 条经过审核的咨询样本进行 2 epoch、all-linear rank-64 QLoRA 微调。

## 结果

在项目历史 Legal-50 语义口径下，均分由 `6.81` 提升至 `7.33`，成对结果为 29 胜、11 平、10 负。这是研究阳性结果，不代表生产级法律可靠性；模型仍可能给出错误、过时或无依据的法律判断与法条引用。

## 加载

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

adapter = "haofue2i1z3/Qwen3-8B-LegalConsult-Gold"
base = "Qwen/Qwen3-8B-Base"

tokenizer = AutoTokenizer.from_pretrained(adapter, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(base, device_map="auto", torch_dtype="auto")
model = PeftModel.from_pretrained(model, adapter)
```

## 限制

- Legal-50 在历史开发中被多次使用，不是干净盲测集。
- 训练标签经过模型审核，不等同于律师认证。
- 仅供研究与作品展示，不提供法律服务。
