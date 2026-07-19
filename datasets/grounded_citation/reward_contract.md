# 引用标记与确定性奖励合同

每道题属于三种模式之一：

- `no_context`：不提供外部法条，目标是避免自行给出具体法名和条号。
- `rag_clean`：提供相关权威上下文，奖励 required 引用覆盖和 grounded precision。
- `rag_distractor`：同时提供相关与干扰上下文，要求覆盖 required、允许 allowed、拒绝 forbidden。

required 引用必须同时匹配法名和条号。中文数字与阿拉伯数字条号按同一编号归一。完整 required 覆盖优先，但额外 allowed 引用按数量衰减，避免通过堆砌引用获得更高奖励。静态硬失败包括问题复读、重复段落、明显模板滥用、非正常终止以及无依据引用。

该合同评价的是输入 grounding 和引用行为，不判定法条本身是否现行、真实或适用于个案。
