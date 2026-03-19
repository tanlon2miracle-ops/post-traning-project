# 技术栈选型 & 对比

## 总览

```
数据合成（文本）  → Distilabel（接内部 API）
数据清洗          → Data-Juicer（多模态，中文友好）
标注质检          → Argilla（可选）
格式转换          → 自研（图文对话格式，薄层胶水）
SFT / DPO        → LLaMA-Factory
RL（可选）        → veRL
通用评测          → lm-evaluation-harness
风控评测          → 自研
推理部署          → vLLM
```

## 数据合成

| 工具 | 特点 | 结论 |
|------|------|------|
| **Distilabel** (Argilla) | 专做 LLM 数据合成，支持自定义 API，内置 SFT/DPO pipeline | ✅ 采用 |
| NeMo-Curator | NVIDIA，GPU 加速，偏大规模文本清洗 | ❌ 合成不是强项 |
| Self-Instruct | 经典自指令 | ❌ 太老 |
| Magpie | 从 LLM 直接提取对话 | ❌ 可控性差 |

## 数据清洗

| 工具 | 特点 | 结论 |
|------|------|------|
| **Data-Juicer** (阿里) | 中文友好，80+ 算子，支持多模态，yaml 配置 | ✅ 采用 |
| NeMo-Curator | GPU 加速，语义去重强 | ❌ 量级不需要 |
| DataTrove (HF) | 轻量 | ❌ 偏预训练 |

## 训练框架

| 工具 | 特点 | 结论 |
|------|------|------|
| **LLaMA-Factory** | Qwen 官方推荐，支持 VL，全参/LoRA/QLoRA，SFT+DPO+PPO | ✅ SFT/DPO |
| **veRL** (字节) | 主流 RL 框架，分布式友好 | ✅ RL（如需要） |
| NeMo Framework | NVIDIA 全家桶 | ❌ 过重 |
| NeMo-Skills | 偏数学/代码 | ❌ 场景不匹配 |

## 推理部署

| 工具 | 特点 | 结论 |
|------|------|------|
| **vLLM** | 开箱即用，支持 VL，PagedAttention | ✅ 采用 |
| SGLang | 结构化生成优化 | 备选 |
| TensorRT-LLM | NVIDIA 优化，最高吞吐 | 后期考虑 |

## 自研部分

只在业务特殊的地方自研：

| 模块 | 原因 |
|------|------|
| 标签映射逻辑 | 业务标签体系特殊 |
| 格式转换 | 图文对话格式，薄层胶水 |
| 风控评测脚本 | 业务指标（准确率/误杀率/漏杀率） |
