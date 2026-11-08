# LLM Preference Evaluation

**面向 LLM-as-a-Judge 偏好预测的研究型评价项目。**

版本：`v0.1.0` · 状态：基于 Notebook 的研究原型

本项目研究不同 Prompting Strategies
与多智能体评价方式，如何影响大语言模型判断两条候选回答中哪一条更符合人类偏好的能力。项目最初源于课程作业，目前正在整理为可复现、可扩展的研究项目。

> **本版本范围：** 当前版本以实验 Notebook 为核心，尚未实现可复用的
> Python 包或命令行工具。

## 当前版本包含的内容

- 五种评价方法：Zero-shot
  Baseline、Few-shot、Chain-of-Thought（CoT）、Self-Consistency、Multi-Agent。
- 答案顺序随机化及标签映射。
- 五种方法共用的 96 条测试样本。
- 配对错误分析与统计显著性检验。
- 延迟、Token 成本、Pareto 效率分析和 Multi-Agent 消融实验。
- 答案顺序偏差分析、分类指标、代表性案例及可视化。
- 运行 Notebook 后导出逐方法 CSV 结果和实验配置。

## 当前实验结果

实验使用 `langchain-deepseek` 调用 `deepseek-chat`。课程提供的数据集包含
100 组问答对，其中 4 条平衡抽样作为 Few-shot 示例，不参与最终测试；其余
96 条由五种方法共同评估。

| 方法               | 准确率 | 有效输出率 | 平均延迟/样本 | 平均 Token/样本 | Macro-F1 |
|--------------------|-------:|-----------:|--------------:|----------------:|---------:|
| Zero-shot Baseline | 61.46% |    100.00% |      0.989 秒 |             246 |   59.89% |
| Few-shot           | 61.46% |    100.00% |      1.019 秒 |           1,073 |   61.12% |
| Chain-of-Thought   | 66.67% |     98.96% |      2.545 秒 |             506 |   66.09% |
| Self-Consistency   | 63.54% |    100.00% |      8.103 秒 |           1,545 |   62.86% |
| Multi-Agent        | 61.46% |    100.00% |      5.337 秒 |           1,143 |   61.12% |

以上是本次运行记录，并不意味着某一种方法在所有任务中都更优。总体准确率差异未达到统计显著水平（Cochran’s
Q = 1.92，p = 0.7505）。CoT 相比 Baseline 的观察准确率高 5.21
个百分点，但配对检验经校正后并不显著。详细结果和限制见
[`docs/RESULTS_SUMMARY.md`](docs/RESULTS_SUMMARY.md)。

## 项目结构

``` text
llm-preference-evaluation-v0.1.0/
├── README.md
├── README_zh.md
├── CHANGELOG.md
├── requirements.txt
├── .gitignore
├── configs/
│   └── experiment_config.json
├── data/
│   └── README.md
├── docs/
│   ├── EXPERIMENT_DESIGN.md
│   ├── PROJECT_ROADMAP.md
│   ├── REPRODUCIBILITY.md
│   └── RESULTS_SUMMARY.md
├── notebooks/
│   ├── 01_preference_evaluation.ipynb
│   └── archive/
│       ├── 00_original_coursework.ipynb
│       └── 01_randomized_fixed_intermediate.ipynb
├── reports/
│   ├── README.md
│   └── preference_evaluation_report.tex
├── results/
│   ├── README.md
│   ├── method_comparison.csv
│   ├── overall_statistical_tests.csv
│   ├── answer_order_bias.csv
│   └── multi_agent_ablation.csv
└── tests/
    └── README.md
```

## 快速开始

当前主 Notebook 面向 **Google Colab**。

1.  将 `notebooks/01_preference_evaluation.ipynb` 上传或打开到 Google
    Colab。
2.  在 Colab Secrets 中新增名为 `DEEPSEEK_API_KEY` 的 Secret。
3.  从头到尾运行 Notebook。
4.  Notebook 会从代码中记录的课程数据地址下载 JSONL 数据，校验 100
    条样本，并创建随机化测试集。
5.  运行结束后，详细表格和实验配置会导出到 `task2_results/`，并生成
    `task2_results.zip`。

如在兼容的 Notebook 环境中运行，可安装依赖：

``` bash
pip install -r requirements.txt
```

当前 Notebook 使用 `google.colab.userdata` 读取密钥，因此在 Colab
以外的本地环境运行仍需要后续重构。不要把 API Key 写进 Notebook 或提交到
Git。

## 实验设置

- 模型：`deepseek-chat`
- Baseline / Few-shot / CoT / Agent / Coordinator 温度：`0`
- Self-Consistency 温度：`0.7`
- Self-Consistency 每条样本采样次数：`5`
- 答案顺序随机化种子：`2026`
- Few-shot 抽样种子：`42`
- Few-shot 示例：`4` 条（展示标签两类各 2 条）
- 测试集：`96` 条
- 测试集答案顺序：49 条原顺序、47 条交换顺序

四条 Few-shot
示例不会进入共同测试集。模型预测会先映射回原始答案身份，再与人类偏好标签比较。

## 重要说明

- **仓库不包含原始数据集。** Notebook 会从 `data/README.md`
  中说明的课程数据地址下载 JSONL
  文件。公开再分发前，请核实课程及数据集许可。
- **API 调用会产生费用。** Self-Consistency 和 Multi-Agent
  方法需要更多模型调用。
- **结果仅代表本次实验快照。**
  模型版本、服务状态和网络状况变化都可能影响后续运行结果。
- **当前初始版本尚未选择开源许可证。** GitHub
  公开仓库并不自动意味着开源。邀请他人复用或贡献之前，需要先确定并添加许可证。
- 含有学生编号的课程提交 PDF 未放入仓库。`reports/`
  中保留的是去身份化后的报告源文件。

## 开发路线

五阶段开发计划见
[`docs/PROJECT_ROADMAP.md`](docs/PROJECT_ROADMAP.md)，从当前作业实验逐步发展为模块化开源评价工具。

## 当前局限

- 仅使用一个 100 条样本的数据集和一次 96 条样本的留出划分。
- 仅测试一个模型/API 家族及一组参数。
- 当前实现以 Notebook 为主，尚未形成可复用的软件包。
- 与人类偏好标签的一致性不等于事实正确性。
- 当前准确率排名仅为描述性结果，统计检验未证明总体准确率存在显著差异。
