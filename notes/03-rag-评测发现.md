# RAG 评测：三个维度拆解与短板分析

## 测试背景
- 测试对象：NextGen 银行客服 RAG bot
- 测试用例数：17
- 评测维度：Context Relevance / Answer Faithfulness / Answer Relevance
- 模型：llama3.1:8b（硬件原因，bot 和 judge 相同）

## 关键数据
| 维度 | 分数 | 含义 |
|---|---|---|
| Overall | 75% | 综合 |
| Context Relevance | 64% | 检索到的材料对不对 |
| Answer Faithfulness | 97% | 回答有没有超出材料 |
| Answer Relevance | 65% | 回答有没有解决用户问题 |

## 核心发现

### 发现 1：强在「不编造」，弱在「检索」
- Faithfulness 97%，说明模型几乎不编造
- Context Relevance 只有 64%，检索环节是短板
- 很多问题的检索结果不完整，模型即使想答也答不好

### 发现 2：Unanswerable 类得分最低（41%）
- 模型正确地说了「我不知道」，但评测标准认为「没回答就是失败」
- 这暴露了评测标准的盲区：**拒绝也是一种正确“答案”**

### 发现 3：Cross-Document 检索最难
- Cross-Document 类 Context Relevance 只有 50%
- 跨文档问题需要从多个文档中检索和整合，检索难度大

## 分类得分
| 类别 | 平均分 |
|---|---|
| Single Document — Numbers | 100% |
| Adversarial / Confused | 87% |
| Retrieval Confusion | 84% |
| Single Document — Clean | 78% |
| Cross-Document | 75% |
| Negative / Boundary / Edge | 61% |
| Unanswerable | 41% |

## 我的结论
1. 评测 RAG 系统要拆开看检索、生成、回答三个环节
2. 每个环节的评测方法要匹配它的目标
3. 无法回答的问题需要单独的评测标准，不能和普通问题混在一起

## 对应岗位要求
- 熟悉评测集、指标体系、Rubric 设计及自动化评测流程
- 能够围绕复杂任务设计合理评测方法