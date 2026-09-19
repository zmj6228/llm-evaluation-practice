# 大模型评测方法论实践

本项目基于开源仓库 [amitbad/llm-evaluation](https://github.com/amitbad/llm-evaluation) 进行学习实践，记录了我在完成 Phase 0-5 评测链路过程中跑出的结果、发现的问题和总结的方法论。

原作者代码：https://github.com/amitbad/llm-evaluation
我的实践记录：见 `notes/` 和 `reports/`

在此完整流程的基础上，我发现，在无大算力的情况下，数据的质量高低就成了一项重要的指标。不同的模型在不同功能上的分布特征有着很明显的区别，尤其是当进行LLM-as-a-Judge时，数据的筛选就显得尤为重要。故因此，我认为，当前大环境下，不仅要对大模型进行高强度训练，对于像Llama-3-8B这样的小模型，更能凸显出数据在整个测试评估流程中的重要性。

## 我做了什么

- 完整跑通 Phase 0-5 评测链路：关键词检查 → BLEU/ROUGE → LLM-as-judge → 语义相似度 → Groundedness → 温度敏感度 → RAG 评测 → Agent 轨迹评测
- 修复了原作者脚本在 Windows 中文系统下的 GBK 编码问题，涉及 `healthcare_bot.py`、`groundedness_evaluator.py`、`rag_bot.py` 等多个脚本。
- 记录了 5 个关键发现（见 `notes/`）

## 关键发现

1. **LLM-as-judge 人机一致率仅 43%**（49 用例）：分歧分三类——裁判更严格、裁判更宽松、对预期行为理解不同。
2. **Groundedness 裁判会误判**：9 条全部被判 GROUNDED，但逐条核对发现裁判放过了编造信息的回答。
3. **RAG 评测的短板在检索**：Context Relevance 只有 64%，而 Answer Faithfulness 高达 97%。
4. **温度敏感度因模型而异**：llama3.1:8b 四个温度稳定 87%，deepseek-r1:7b 从 93% 掉到 67%。
5. **轨迹评测能发现过程缺失**：Agent 最终答案看起来对，但轨迹评测发现它少了一步关键操作。

## 目录结构
```text
llm-evaluation-practice/
├── README.md
├── notes/
│   ├── 01-llm-as-judge-人机分歧分析.md
│   ├── 02-groundedness-裁判误判案例.md
│   ├── 03-rag-评测发现.md
│   ├── 04-温度敏感度对比.md
│   └── 05-轨迹评测发现.md
└── reports/
    ├── groundedness_report.html
    ├── rag_evaluation_report.html
    ├── temperature_sensitivity_report.html
    ├── trajectory_evaluation_report.html
    ├── trajectory_evaluation_results.json
    └── banking_bot_test_results.xlsx
```

## 致谢

评测框架和原始代码来自 [amitbad/llm-evaluation](https://github.com/amitbad/llm-evaluation)。