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

这是将 V24 time-precision rank-64 结构化 Intake LoRA 合并进 Qwen3-8B Instruct BF16 后，再进行 W4A16 非对称量化得到的完整权重。权重采用 4 bit、group size 128，`lm_head` 保持未量化，加载时不需要额外 LoRA。

## 量化结果

- V24-AWQ 产物实测约 `6.11 GB`。`16.40 GB` 的 BF16 参照值来自同架构 Base-BF16 checkpoint；LoRA 合并不改变模型参数规模，因此可用于体积量级对照，但不是一份单独归档的 V24-BF16 文件体积测量。
- V24 confirm120 后验 2x2 实验中，相对融合后的 BF16 模型，整体抽取 micro-F1 为 `0.6900→0.7111`，金额 F1 为 `0.8133→0.7771`，时间 F1 为 `0.6299→0.6758`。
- 无依据抽取项由 `4` 增至 `7`，说明量化同时移动了抽取边界，并非无损升级。

这些结果是事后描述性证据，不能改变 V24 的归档 FAIL 判定。项目中的 RTX 4090D 吞吐和自由文本引用基准测量的是未适配 Base-AWQ，不是本模型，不能直接归因于这份权重。

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
- 原 V24 结构化模型未通过全部门禁，本量化版本也没有晋级。
- 仅供研究与作品展示，不提供法律服务。
