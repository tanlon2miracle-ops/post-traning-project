# 风控领域 LLM 后训练项目

> 基于开源多模态 LLM，通过后训练注入内容安全 & 欺诈风控领域知识

## 项目目标

| 优先级 | 使用场景 | 说明 |
|--------|---------|------|
| P0 | 辅助风控规则生成 / 策略编写 | 理解规则库，辅助生成新规则 |
| P0 | 自动审核优化 | 图文内容安全判定（分类+推理） |
| P1 | Agent 基座 | 后期目标，风控领域智能体 |

## 技术方案概览

```
┌─────────────────────────────────────────────────────────────┐
│                     数据层 (Phase 0)                        │
│                                                             │
│  人审数据(图文+标签)  ──→  清洗 & 标签映射                    │
│  规则库(参数+规则)    ──→  结构化 & 种子提取                   │
│  内部API(GLM/Qwen3)  ──→  数据合成(5套模板)                   │
│  开源数据集           ──→  适配 & 混合(通用40%)                │
│                            ↓                                │
│                   Data-Juicer 清洗 & 去重                    │
│                            ↓                                │
│                   格式化 → SFT jsonl / DPO pairs             │
├─────────────────────────────────────────────────────────────┤
│                     训练层 (Phase 1-2)                       │
│                                                             │
│  SFT  → LLaMA-Factory (Qwen2.5-VL-7B)                      │
│  DPO  → LLaMA-Factory (可选，视SFT效果)                      │
│  RL   → veRL (可选)                                         │
├─────────────────────────────────────────────────────────────┤
│                     部署层 (Phase 3)                         │
│                                                             │
│  推理 → vLLM                                                │
│  评测 → 自研风控评测 + lm-evaluation-harness                  │
└─────────────────────────────────────────────────────────────┘
```

## 项目结构

```
post-traning-project/
├── README.md                 # 本文件
├── docs/
│   ├── roadmap.md            # Roadmap & 进度追踪
│   ├── decisions.md          # 已确定的技术决策
│   ├── label-taxonomy.md     # 标签体系设计
│   ├── data-synthesis.md     # 数据合成方案 & prompt 模板
│   └── tech-stack.md         # 技术栈选型 & 对比
├── scripts/
│   ├── synthesize/           # 数据合成脚本
│   ├── clean/                # 数据清洗脚本
│   ├── format/               # 格式转换脚本
│   └── evaluate/             # 评测脚本
├── configs/
│   ├── sft_qwen25vl_7b.yaml  # SFT 训练配置
│   ├── dpo_qwen25vl_7b.yaml  # DPO 训练配置
│   └── data_juicer.yaml      # Data-Juicer 清洗配置
├── data/                     # .gitignore，不入库
│   ├── raw/                  # 原始人审数据
│   ├── cleaned/              # 清洗后数据
│   ├── synthetic/            # 合成数据
│   ├── opensource/            # 开源数据集
│   └── formatted/            # 最终训练格式
└── eval/
    └── test_set/             # 评测集
```

## 快速导航

- **项目进度** → [docs/roadmap.md](docs/roadmap.md)
- **技术决策** → [docs/decisions.md](docs/decisions.md)
- **标签体系** → [docs/label-taxonomy.md](docs/label-taxonomy.md)
- **数据合成方案** → [docs/data-synthesis.md](docs/data-synthesis.md)
- **技术栈** → [docs/tech-stack.md](docs/tech-stack.md)

## 硬件资源

- **GPU**: 几十张 NVIDIA H20 (96GB HBM)
- 足够支撑: 7B 全参微调 / 32B LoRA+DeepSpeed / 72B 多卡并行

## 参考资料

- [Llama-Nemotron Post-Training Dataset](https://huggingface.co/datasets/nvidia/Llama-Nemotron-Post-Training-Dataset) — NVIDIA 3300万样本后训练方法论
- [Nemotron 笔记](https://github.com/tanlon2miracle-ops/post-training-notes/blob/main/07-llama-nemotron-post-training-dataset.md)
- [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory)
- [veRL](https://github.com/volcengine/verl)
- [Data-Juicer](https://github.com/modelscope/data-juicer)
- [Distilabel](https://github.com/argilla-io/distilabel)
