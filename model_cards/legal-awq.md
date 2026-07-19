---
language:
- zh
license: apache-2.0
base_model: Qwen/Qwen3-8B
pipeline_tag: text-generation
tags:
- legal
- chinese
- awq
- compressed-tensors
- quantized
---

# Qwen3-8B Legal AWQ W4A16

这是结构化 Intake LoRA 合并后的 Qwen3-8B 模型的 W4A16 非对称量化版本，权重采用 4 bit、group size 128，`lm_head` 保持未量化。仓库包含完整量化权重，不需要额外 LoRA。

## 部署结果

- 权重体积由约 `16.40 GB` 降至约 `6.11 GB`。
- RTX 4090D、vLLM 基准中，输出吞吐在并发 1/4/8/16/32 下分别由约 `75.62/285.58/555.80/1003.89/1777.38 tok/s` 提升到 `194.99/740.06/1387.01/2315.97/3793.90 tok/s`。
- 自由文本 dev60 上，无输入依据引用行由 BF16 的 `38` 增至 AWQ 的 `49`，质量无回退门禁失败。

量化是部署折中，不是无损升级。实际使用必须配套结构校验和引用依据校验。

## 加载

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

repo = "haofue2i1z3/Qwen3-8B-Legal-AWQ"
tokenizer = AutoTokenizer.from_pretrained(repo, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(repo, device_map="auto", torch_dtype="auto")
```

需要支持 `compressed-tensors` 量化格式的较新 Transformers/vLLM 环境。

## 限制

- 量化会移动不同任务的 precision/recall 与引用行为边界。
- 2x2 量化实验是事后描述性证据，不能改变原结构化模型的归档判定。
- 仅供研究与作品展示，不提供法律服务。
