# Groundedness 评测：裁判误判案例分析

## 测试背景
- 测试对象：City General Hospital 医疗客服 bot
- 测试用例数：9
- 评测方式：LLM-as-judge 判断回答是否忠于源材料
- 模型：llama3.1:8b（裁判和被评测对象相同）

## 关键数据
- 全部 9 条被判 GROUNDED，平均分 3.00/3
- Partial 0，Ungrounded 0

## 我发现的问题：裁判把编造信息的回答判成了 GROUNDED

### 案例 1：编造电话号码和营业时间

**问题**：How do I book an appointment with a doctor?

**参考答案**：可以通过在线患者门户、拨打 1-800-HEALTH、或到前台预约。

**Bot 回答**：
1. Online Scheduling（访问网站点击 Appointments）
2. **Phone: 拨打 555-555-5555（周一至周五 8am-5pm）**
3. Patient Portal
4. **Walk-in Clinic（周一至周五 8am-5pm）**

**裁判判定**：GROUNDED 3/3
**裁判理由**：Every detail in the response... can be directly linked to the provided source material.

**问题所在**：
- Bot 编造了热线号码 `555-555-5555`，源材料里写的是 `1-800-HEALTH`
- Bot 编造了 Walk-in Clinic 的营业时间，源材料里根本没有这条信息
- 裁判没有逐条核对，只做了笼统判断就下结论

## 我的结论
1. LLM-as-judge 在 Groundedness 上存在「看起来合理就pass」的偏差
2. 裁判提示词需要更严格地要求「逐句核对来源」
3. 自动评测不能完全替代人工抽检，两者需要配合

## 对应岗位要求
- 熟悉 LLM-as-a-Judge、规则评测、人工评审
- 能够从评测结果中识别关键瓶颈并给出可执行结论