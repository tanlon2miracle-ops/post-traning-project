# 🟣 算法工作区

算法和大模型同学的主阵地。

## 目录说明

| 目录 | 用途 | 说明 |
|------|------|------|
| `synthesis/` | 数据合成 | Distilabel 合成逻辑、Prompt 模板调试、合成质量分析 |
| `eval/` | 评测 | 自研风控评测脚本、lm-evaluation-harness 适配、评测报告 |
| `configs/` | 训练配置 | LLaMA-Factory SFT/DPO yaml、veRL RL 配置 |

## 核心交付物

1. **合成数据** — 5-10万条高质量 SFT 数据（输出到 `data/synthetic/`）
2. **评测体系** — 风控评测集 + 评测脚本 + baseline 报告
3. **训练配置** — 调优后的最佳训练参数
4. **模型** — SFT/DPO 后的模型 checkpoint（输出到 `checkpoints/`）
