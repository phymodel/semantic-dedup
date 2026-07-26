# Semantic Dedup Subagent

基于语义的近似去重子代理。对比文本片段之间的**语义近似度**（非哈希精确匹配），输出重复分组、置信度和保留/删除建议。

## 配置

| 项 | 值 |
|----|-----|
| 预设模型 | `deepseek:v4-flash` |
| 可用工具 | `read_file`, `grep_workspace`, `glob_files` |
| 权限 | 只读 |

## 使用方式

在 Agent 面板启用此 subagent 后，向主 Agent 下发去重任务，主 Agent 会通过 `run_subagent("semantic-dedup", task)` 委派执行。

## 输入

- 一组文本片段或文件路径，要求判断其中是否存在语义近似重复。

## 输出

JSON 友好的结构化结果：

```json
{
  "groups": [
    {
      "members": ["snippet_a", "snippet_b"],
      "confidence": 0.95,
      "reason": "语义高度重叠，仅表述不同"
    }
  ],
  "keep_remove_rationale": [
    { "keep": "snippet_a", "remove": "snippet_b", "reason": "snippet_a 更完整" }
  ]
}
```

## 适用场景

- 数据集清洗前的语义去重
- 文档 / 对话语料中的近似冗余检测
- 多来源文本合并时的去重审查
