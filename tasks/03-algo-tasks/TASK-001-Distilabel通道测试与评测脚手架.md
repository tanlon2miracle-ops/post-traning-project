# TASK-001: Distilabel 合成链路验证与自研评测骨架

> **状态 (Status)**: 📝 待认领 (Open)
> **认领人 (Assignee)**: @[请在此处@你的名字]
> **关联文档**: `docs/data-synthesis.md`

## 🎯 任务背景
Phase 0 规划了 5 套数据合成的 Prompt 模板，我们需要尽快在代码层面跑通 API 调用链路，验证其生成质量；同时，评测代码需要前置准备。

## 📋 任务清单

- [ ] **行动 1：Distilabel 合成最小可用测试 (MVP)**
  - **动作**：基于 `docs/data-synthesis.md` 中的【模板 2：规则正反样本对生成】。
  - **开发**：在 Distilabel 框架下编写 Pipeline 代码，接入内部的 GLM 或 Qwen3 API。读取 Ops 组提供的 `rules_example.json`，试跑 10 条左右的合成数据。
  - **检查**：肉眼 Review 生成的 JSONL，确认其格式是否符合设定的图文对话结构 (`messages` 数组)。
  - **交付动作**：在 `03-algo-workspace/synthesis/` 下提交测试脚本 `test_synthesis_pipeline.py`。

- [ ] **行动 2：自研风控评测脚本脚手架**
  - **动作**：无需等待真实的测试集，先写好代码骨架。
  - **开发**：输入为“模型推理结果(json)”和“真实人工标签(json)”，能够自动化计算并输出：
    1. 整体准确率 (Accuracy)
    2. 误杀率 (False Positive Rate) - *极度重要指标*
    3. 漏杀率 (False Negative Rate)
  - **交付动作**：在 `03-algo-workspace/eval/` 提交 `risk_eval_metrics.py`。

## 📤 验收标准
- 合成脚本能成功调用内部大模型并返回格式合规的 JSONL，评测脚本能在 mock 数据上正确计算出混淆矩阵指标。
