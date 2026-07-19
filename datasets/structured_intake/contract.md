# 结构化抽取合同

模型只能输出一个 JSON 对象，并且只能包含：

- `case_type`
- `amounts`
- `time_elements`
- `claims`
- `evidence_checklist`

`case_type` 必须属于六个固定类别。`amounts` 和 `time_elements` 必须是原问题中的逐字子串，遵循最小完整语义值，不推断、不换算、不补单位。未量化的“一笔钱”“现在”“当时”“最近”等不进入对应数组。

主要评价指标为 JSON 有效率、案件类型准确率、金额/时间精确匹配、整体 micro-F1、无依据抽取项和 negative 精确匹配率。
