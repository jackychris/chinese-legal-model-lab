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

# Qwen3-8B LegalCitation GRPO 600

从 GRPO-200 checkpoint 按原配方继续训练至全局 600 步的剂量响应终点。该模型用于检验残余问题是否仅由训练剂量不足导致。

## 结论

dev84 的冻结抽取器 required 行召回率由 Base 的 `83.33%` 变为 `88.10%`，低于 200 步的 `92.86%`；该指标对 Markdown、法名与条号间隔及嵌套书名号敏感，不解释为纯内容召回。grounded citation precision 为 `77.36%`，仍远低于 `95%` 门禁。Legal-50 统一场均分为 Base `6.908`、200 步 `6.924`、600 步 `6.944`，差异远低于约 `0.4` 的可辨别尺度。

最终判定为 `FAIL`。600 步没有证明额外剂量能够解决剩余引用、停止和重复问题，因此该模型只作为剂量响应研究终点发布。

## 加载

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

adapter = "haofue2i1z3/Qwen3-8B-LegalCitation-GRPO-600"
base = "Qwen/Qwen3-8B"

tokenizer = AutoTokenizer.from_pretrained(adapter, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(base, device_map="auto", torch_dtype="auto")
model = PeftModel.from_pretrained(model, adapter)
```

## 限制

- 不是 GRPO-200 的全面升级，部分指标出现回落。
- 不应通过选择 200 或 600 步的单项高点宣称已通过门禁。
- 仅供研究与作品展示，不提供法律服务。
