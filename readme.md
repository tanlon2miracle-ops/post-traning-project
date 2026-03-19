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
├── README.md
├── docs/                         # 📖 架构、决策、Roadmap（所有人可读，PM 维护）
│   ├── roadmap.md                #   进度追踪 & TODO
│   ├── decisions.md              #   技术决策记录
│   ├── label-taxonomy.md         #   标签体系设计
│   ├── data-synthesis.md         #   数据合成方案 & Prompt 模板
│   └── tech-stack.md             #   技术栈选型对比
│
├── tasks/                        # 📍 任务分发与追踪中心
│   ├── 01-ops-tasks/             #   运营/策略组任务打卡区
│   ├── 02-dev-tasks/             #   开发/工程组任务打卡区
│   └── 03-algo-tasks/            #   算法/大模型组任务打卡区
│
├── 01-ops-workspace/             # 🟢 运营/风控同学的主阵地
│   ├── rules/                    #   结构化规则库（JSON/YAML），供程序读取
│   ├── sample_data/              #   极少量严格脱敏样例数据（供开发调试）
│   └── taxonomy_updates/         #   标签体系变更提案区
│
├── 02-dev-workspace/             # 🔵 开发/工程同学的主阵地
│   ├── envs/                     #   H20 集群环境配置、Dockerfile、requirements
│   ├── data_pipeline/            #   数据清洗(Data-Juicer)、格式转换、标签映射脚本
│   └── deployment/               #   vLLM 推理部署脚本、压测工具
│
├── 03-algo-workspace/            # 🟣 算法同学的主阵地
│   ├── synthesis/                #   Distilabel 数据合成逻辑、Prompt 模板调试
│   ├── eval/                     #   自研风控评测脚本、lm-eval 适配代码
│   └── configs/                  #   LLaMA-Factory / veRL 的 yaml 训练参数
│
├── data/                         # 🔴 .gitignore 忽略！真实数据不入库
└── checkpoints/                  # 🔴 .gitignore 忽略！模型权重不入库
```

### 各工作区职责

| 工作区 | 角色 | 核心产出 |
|--------|------|---------|
| `docs/` | 全员 / PM | 架构文档、决策记录、进度追踪 |
| `tasks/` | 全员 | 任务分发、打卡、进度同步 |
| `01-ops-workspace/` | 运营/策略 | 结构化规则、脱敏样例、标签提案 |
| `02-dev-workspace/` | 开发/工程 | 环境配置、数据管道、部署脚本 |
| `03-algo-workspace/` | 算法 | 合成逻辑、评测脚本、训练配置 |

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
