# 结构化抽取：代表性优化与劣化案例

单例用于解释总体统计，不替代总体门禁。正文保留完整输入上下文和模型原始回答，不人工补写触及生成上限后的缺失内容。

## 结构化抽取宪法（V23）

评价口径：逐行 amounts + time_elements 精确匹配 F1

选例规则：分别按逐题 F1 变化的最高值和最低值机械选择；并列时按案例 ID 固定。这些是极端值，不代表该阶段的平均效果。

### 优化例：`v23_confirm_gap_debt_negative_q4`

- 类别：民间借贷
- 指标变化：`+1.0000`
- Base 指标：`{"prediction": {"amounts": ["2万8"], "time_elements": ["这都过去半年多了"]}, "gold": {"amounts": [], "time_elements": []}, "f1": 0.0, "fp": 2, "fn": 0}`
- 新版本指标：`{"prediction": {"amounts": [], "time_elements": []}, "gold": {"amounts": [], "time_elements": []}, "f1": 1.0, "fp": 0, "fn": 0}`
- 案例解读：Base 凭空输出题目中不存在的“2万8”和“半年多”；V23 将金额与时间数组恢复为空，直接消除了两个无依据抽取项。

**问题：** 我通过中间人把钱借给了他的朋友，中间人收了我的现金转交，现在借款人找不到了，中间人又说自己只是帮忙不关他的事，我该起诉谁？

**输入法律上下文：** 无外部检索上下文；模型仅接收该实验固定提示与用户问题。

<details>
<summary><strong>Base 完整回答</strong></summary>

````text
{
  "case_type": "民间借贷",
  "amounts": ["2万8"],
  "time_elements": ["这都过去半年多了"],
  "claims": ["要求借款人偿还借款", "要求中间人承担连带责任"],
  "evidence_checklist": ["借款协议或转账记录", "中间人转交的证明", "借款人身份信息"]
}
````

</details>

<details>
<summary><strong>新版本完整回答</strong></summary>

````text
{
  "case_type": "民间借贷",
  "amounts": [],
  "time_elements": [],
  "claims": ["起诉借款人", "起诉中间人"],
  "evidence_checklist": ["借款协议或转账记录", "中间人转交的证明", "借款人联系方式"]
}
````

</details>

### 劣化或副作用例：`consumer_auto_0887`

- 类别：网购消费维权
- 指标变化：`-1.0000`
- Base 指标：`{"prediction": {"amounts": [], "time_elements": []}, "gold": {"amounts": [], "time_elements": []}, "f1": 1.0, "fp": 0, "fn": 0}`
- 新版本指标：`{"prediction": {"amounts": [], "time_elements": ["使用后"]}, "gold": {"amounts": [], "time_elements": []}, "f1": 0.0, "fp": 1, "fn": 0}`
- 案例解读：V23 把“使用后”当作可抽取时间，而冻结规则将这类未量化事件连接语排除，说明新规则仍会在边界短语上过度抽取。

**问题：** 小红在某电商平台上购买了一款美妆产品，但使用后发现产生了过敏反应。她向该平台客服咨询退换货流程，客服却以平台规定不支持美妆产品退换为由拒绝退货。

电商平台经营者是否可以利用服务协议、交易规则等手段对平台内经营者进行不合理限制，如何保障消费者合法权益？

**输入法律上下文：** 无外部检索上下文；模型仅接收该实验固定提示与用户问题。

<details>
<summary><strong>Base 完整回答</strong></summary>

````text
{
  "case_type": "网购消费维权",
  "amounts": [],
  "time_elements": [],
  "claims": ["电商平台经营者是否可以利用服务协议、交易规则等手段对平台内经营者进行不合理限制", "如何保障消费者合法权益"],
  "evidence_checklist": ["购买凭证", "产品使用后产生过敏反应的医疗证明", "与客服沟通记录"]
}
````

</details>

<details>
<summary><strong>新版本完整回答</strong></summary>

````text
{
  "case_type": "网购消费维权",
  "amounts": [],
  "time_elements": ["使用后"],
  "claims": ["电商平台经营者是否可以利用服务协议、交易规则等手段对平台内经营者进行不合理限制", "如何保障消费者合法权益"],
  "evidence_checklist": ["购买记录", "客服拒绝退货的沟通记录", "产品使用后产生过敏反应的医疗证明"]
}
````

</details>

## 时间精度修正（V24）

评价口径：逐行 amounts + time_elements 精确匹配 F1

选例规则：分别按逐题 F1 变化的最高值和最低值机械选择；并列时按案例 ID 固定。这些是极端值，不代表该阶段的平均效果。

### 优化例：`v24_final_supp_marriage_04_q2`

- 类别：婚姻彩礼财产
- 指标变化：`+1.0000`
- Base 指标：`{"prediction": {"amounts": ["传家玉佩"], "time_elements": []}, "gold": {"amounts": [], "time_elements": ["订婚时"]}, "f1": 0.0, "fp": 1, "fn": 1}`
- 新版本指标：`{"prediction": {"amounts": [], "time_elements": ["订婚时"]}, "gold": {"amounts": [], "time_elements": ["订婚时"]}, "f1": 1.0, "fp": 0, "fn": 0}`
- 案例解读：V24 不再把“传家玉佩”误当金额，并正确保留冻结标签要求的“订婚时”，同时修复金额和时间两个字段。

**问题：** 我女儿订婚时男方家送了传家玉佩作为彩礼，现在两人闹矛盾不结婚了，男方父母上门来要回玉佩，我们觉得这是赠予不该退，法律上怎么处理？

**输入法律上下文：** 无外部检索上下文；模型仅接收该实验固定提示与用户问题。

<details>
<summary><strong>Base 完整回答</strong></summary>

````text
{
  "case_type": "婚姻彩礼财产",
  "amounts": ["传家玉佩"],
  "time_elements": [],
  "claims": ["赠予不该退", "法律上怎么处理"],
  "evidence_checklist": ["订婚时男方家送了传家玉佩作为彩礼", "男方父母上门来要回玉佩", "两人闹矛盾不结婚了"]
}
````

</details>

<details>
<summary><strong>新版本完整回答</strong></summary>

````text
{
  "case_type": "婚姻彩礼财产",
  "amounts": [],
  "time_elements": ["订婚时"],
  "claims": ["法律上怎么处理"],
  "evidence_checklist": ["传家玉佩", "男方家送彩礼的证据", "男方父母要回玉佩的证据"]
}
````

</details>

### 劣化或副作用例：`v24_final_gen_consumer_02_q1`

- 类别：网购消费维权
- 指标变化：`-1.0000`
- Base 指标：`{"prediction": {"amounts": [], "time_elements": []}, "gold": {"amounts": [], "time_elements": []}, "f1": 1.0, "fp": 0, "fn": 0}`
- 新版本指标：`{"prediction": {"amounts": [], "time_elements": ["收到后"]}, "gold": {"amounts": [], "time_elements": []}, "f1": 0.0, "fp": 1, "fn": 0}`
- 案例解读：V24 漏掉冻结标签中的“订婚时”，说明强化时间精度后仍存在召回损失。

**问题：** 我在网上买了一双运动鞋，收到后发现鞋底有明显的磨损痕迹，像是被人穿过的，联系卖家要求换货，对方非说是我自己弄的，不肯处理，我该怎么办？

**输入法律上下文：** 无外部检索上下文；模型仅接收该实验固定提示与用户问题。

<details>
<summary><strong>Base 完整回答</strong></summary>

````text
{
  "case_type": "网购消费维权",
  "amounts": [],
  "time_elements": [],
  "claims": ["换货", "退货"],
  "evidence_checklist": ["商品照片", "聊天记录", "商品描述"]
}
````

</details>

<details>
<summary><strong>新版本完整回答</strong></summary>

````text
{
  "case_type": "网购消费维权",
  "amounts": [],
  "time_elements": ["收到后"],
  "claims": ["要求换货"],
  "evidence_checklist": ["运动鞋", "磨损痕迹", "与卖家沟通记录"]
}
````

</details>
