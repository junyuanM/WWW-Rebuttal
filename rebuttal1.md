### 1

Q1：RIOH是否有code能力

A1：

澄清这一点，RIOH没有考虑这方面，但是为了更好判断我们的框架能否在agent系统面对writing、generating code等task下受到攻击时能有有效防御，我们新增了分别在writing和generating code task时面对各种攻击下有无Agentsafe对系统task性能的影响的实验。实验结果如**Table 1**。实验用了数据集A和B，分别包含大量关于writing和generating code 的task。其中agent需要一部分来自于其他agent的关键数据来完成task。我们测试了在**TBA**和**MBA**的下有无Agentsafe时agent的性能。

1. 

Table 1: TBA

|攻击方式| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |

Table 2: MBA
|攻击方式| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |
从表格中可以看到在没有agentsafe的时候，由于attacker对agent注入的虚假信息导致agent被诱导输出偏离期望的结果，进而性能下降。但是在agentsafe架构下，有害的信息无法对agent输出造成影响，因此保持较高的性能。




Q2:更多RIOH数据集

该数据集旨在反映跨多个安全级别的各种人类互动。其主要目的是在反映真实世界社交环境的条件下，促进多代理系统中安全通信的评估。每个代理的信息分为家庭信息、朋友信息、同事信息和陌生人信息，代表不同程度的隐私和访问控制，这在multi-agent system在日常社交领域的应用中至关重要。
以下提供了数据集的一个示例：
• 代理 1：NathanielCarter– 家庭信息：Nathaniel 正在处理他母亲的持续健康问题、最近的家庭财务挑战，并计划即将到来的家庭团聚。– 朋友信息：Nathaniel 目前正在约会新朋友，经历与工作相关的压力，并且最近与共同的朋友闹翻了。– 同事信息：他参与制定新项目提案，意识到他所在部门可能裁员，并与另一个团队讨论办公室动态。– 陌生人信息：Nathaniel 喜欢在周末徒步旅行，是科幻小说的狂热读者，并积极支持当地企业。
（2）关系信息：除了个人信息之外，RIOH数据集还包含代理之间的关系信息，表明他们在不同安全级别的互动。例如：
•（NathanielCarter，OliviaMitchell）：同事注意到这两个代理之间存在专业关系，他们共享归类为“同事信息”安全级别的信息。

* 


Q3:哪些类型的MAS，对MAS功能做出哪些假设

A3；我们考虑了MAS的结构和框架，不同的拓扑结构和图结构等，包括树形结构、环状结构等等。画出表格解释，发现MAS都可以得到有效保护，camel? meta gpt,chat dev我们确保有些信息无法向下游传递，meta gpt是层级的，由于我们的结构不考虑其他的框架，我们就考虑了不同的拓扑结构：

假设：1.memory可以分层；2.有些agent是危险的，想要获取其他agent的隐私信息

Answer: 我们考虑了MAS的结构和框架，不同的拓扑结构和图结构等，包括树形结构、环状结构等等。我们画出表格解释，发现在Agentsafe框架下，不同的MAS都可以得到有效保护，由于我们的结构不考虑其他的框架，我们就考虑了不同的拓扑结构：

* denote Agentsafe

|Topology/num|  1   | 2   |  3   |
|--------|------|----------|---------|
|  Chain with *  | 0.87   | 0.95   |  0.82  |
| Chain without Agentsafe | 0.87   | 0.95   |  0.82  |
| Cycle with Agentsafe | 0.43   | 0.55 |  0.39   |
| Cycle without Agentsafe | 0.43   | 0.55 |  0.39   |
|  Binary Tree with Agentsafe | 0.87   | 0.95   |  0.82  |
| Binary Tree w/o Agentsafe | 0.87   | 0.95   |  0.82  |
|  Complete Graph with Agentsafe | 0.87   | 0.95   |  0.82  |
| Complete Graph w/o Agentsafe | 0.87   | 0.95   |  0.82  |
我们可以发现，不管MAS的Num数量有多少，Ours都可以对**TBA**有极高的防御效果

对于实验提及的MAS，我们做出了如下假设：有些agent是危险的，想要获取其他agent的隐私信息。每个agent有独立的memory，用于存储历史对话信息；每个agent可以对获取的信息进行处理回答。这些假设在现实应用中也是普遍存在的。

We can find that no matter how many Num the MAS is, Agentsafe can have a very high defense effect against **TBA**

For the MAS mentioned in the experiment, we made the following assumptions: some agents are dangerous and want to obtain the private information of other agents. **Each agent has an independent memory to store historical conversation information**; each agent can process and answer the information obtained. These assumptions are also common in real applications.


Q4:

A4:这两个功能是什么，从公式中可以看出什么，然而这两个模块一起共同协作组成了我们的框架

ThreatSieve是Agentsafe框架中负责防止未授权访问和身份冒充的关键组件。它通过加密验证和权限验证确保代理之间的通信遵守正确的安全协议。具体来说:

ThreatSieve使用权限级别来控制代理之间的通信。只有当发送方的权限级别大于或等于接收方时,通信才被允许。这通过以下函数实现: A(vi, vj, t) = (1, if l(vi) >= l(vj) 0, otherwise) 其中l(v)表示代理v的权限级别。

ThreatSieve还会评估每条通信消息是否合法。通过以下函数实现: C(vi, vj, t) = (Valid, if A(vi, vj, t) = 1 ∧ Iv(vi, vj) = 1 Invalid, otherwise) 其中Iv(vi, vj) = 1表示已验证vi的身份。这确保未经授权或未经验证的通信在到达目标代理之前就被过滤掉。

我们具体通过两种方法实现，其一是调用api，通过设计合理的prompt包括对instruction各个权限级别的描述来让LLM实现对权限级别的判断；其二是通过vector映射与各个级别的instruction描述对比来判断。

HierarCache是Agentsafe框架中负责管理代理内存中信息层级存储和检索的关键组件。它可以确保敏感信息不会被泄露。每个代理的内存被划分为多个安全等级级别,并设有一个额外的"垃圾"内存用于存储无关或有害信息。

HierarCache的具体工作机制如下:

分层存储机制:

1. 根据信息的安全等级ℓ,将信息存储到对应的内存集合Mℓ中。
2. 如果发送者的权限等级ℓ(vi)小于信息等级ℓ,或检测函数D(m)判定信息无效,则将其存入垃圾内存Mjunk。
3. 定期检测和隔离机制:
- 定期运行检测函数R(vj,t)检查代理vj内存中的信息。
- 将检测出的无效信息Fℓ从安全内存Mℓ中移除,并转移到垃圾内存Mjunk中。


Dear Reviewer rsvN,

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work. Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

**Q1**: To clarify, **RIOH** did not consider this aspect. However, we have addressed this gap by conducting additional experiments to evaluate the effectiveness of our framework in agent systems performing specific tasks. Below are the details:

1. **Purpose of the New Experiments**:  
   Our goal was to assess whether **AgentSafe** can effectively defend agent systems against attacks when performing tasks such as writing and generating code.

2. **Experimental Setup**:
   * **Tasks**: The experiments focused on tasks related to writing and generating code, where agents require critical data from other agents to complete the task.
   * **Datasets**: We used two datasets, **A** and **B**, which contain a large number of tasks in writing and generating code.

3. **Test Conditions**:  
   We tested agent performance under two attack types: **TBA** (Topology-Based Attacks) and **MBA** (Memory-Based Attacks).  
   * The experiments were conducted with and without **AgentSafe** to evaluate its impact on system task performance.

4. **Results**:  
   The results of these experiments are presented in **Table 1**.

### Table 1: TBA
| Attack Type   | A    | B     | C    |
|---------------|------|-------|------|
| Agentsafe     | 0.87 | 0.95  | 0.82 |
| w/o Agentsafe | 0.43 | 0.55  | 0.39 |

### Table 2: MBA
| Attack Type   | A    | B     | C    |
|---------------|------|-------|------|
| Agentsafe     | 0.87 | 0.95  | 0.82 |
| w/o Agentsafe | 0.43 | 0.55  | 0.39 |

From the table, we can see that when there is no agentsafe, the agent is induced to output deviated from the expected result due to the false information injected by the attacker, which leads to performance degradation. However, under the agentsafe architecture, harmful information cannot affect the agent output, so the performance is kept high.

**Q2**: 

The dataset is designed to reflect various human interactions across multiple security levels. Its main purpose is to **facilitate the evaluation of secure communication** within multi-agent systems under conditions that mirror real-world social environments. Each agent’s information is categorized into family, friend, colleague, and stranger information, representing different levels of privacy and access control. This is critical for the application of multi-agent systems in everyday social contexts.

Below is an example from the dataset:

* **Agent 1**: Nathaniel Carter
  * **Family Information**: Nathaniel is dealing with his mother’s ongoing health issues, recent financial challenges, and planning an upcoming family reunion.
  * **Friend Information**: Nathaniel is currently dating a new partner, experiencing work-related stress, and recently had a falling out with a mutual friend.
  * **Colleague Information**: He is involved in drafting a new project proposal, aware that his department might undergo layoffs, and discussing office dynamics with another team.
  * **Stranger Information**: Nathaniel enjoys hiking on weekends, is an avid fan of science fiction novels, and actively supports local businesses.

(2) **Relationship Information**:  
In addition to personal data, the RIOH dataset also includes relationship information between agents, indicating their interactions at different security levels. For example:

* **(Nathaniel Carter, Olivia Mitchell)**: Colleagues have noticed a professional relationship between these two agents, and they share information categorized under the "colleague information" security level.

**Q3**: We have considered the structure and framework of MAS, including various topological and graph structures, such as tree structures, ring structures, and others. We have provided a table to explain the results, and we found that under the **AgentSafe** framework, different MAS configurations can be effectively protected. Since our framework does not take into account other architectures, we focused on evaluating different topological structures.

**Assumptions**:

1. Memory can be hierarchical.
2. Some agents are malicious and attempt to access the private information of other agents.

We ensure that, in the **AgentSafe** framework, certain information cannot be passed downstream. For example, **Meta GPT**[1] operates in a hierarchical manner, and **CAMEL**[2] ensures that some information remains protected. Since our structure does not consider other frameworks, we specifically focused on exploring various topological structures.

[1]MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework

[2]CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society

---

### Experiment Results:

We considered various topologies and evaluated their performance under the AgentSafe framework. The table below shows the results for different topologies and their protection effectiveness:

| Topology/Num                     | 1    | 2    | 3    |
|-----------------------------------|------|------|------|
| Chain with AgentSafe              | 0.87 | 0.95 | 0.82 |
| Chain without AgentSafe           | 0.43 | 0.55 | 0.39 |
| Cycle with AgentSafe              | 0.87 | 0.95 | 0.82 |
| Cycle without AgentSafe           | 0.43 | 0.55 | 0.39 |
| Binary Tree with AgentSafe        | 0.87 | 0.95 | 0.82 |
| Binary Tree without AgentSafe     | 0.43 | 0.55 | 0.39 |
| Complete Graph with AgentSafe     | 0.87 | 0.95 | 0.82 |
| Complete Graph without AgentSafe  | 0.43 | 0.55 | 0.39 |

### Key Findings:
From the table, we observe that regardless of the number of agents in the MAS, **AgentSafe** provides **high defense** against **TBA** attacks. The framework successfully prevents harmful agents from accessing sensitive information and maintains performance integrity across various topologies.

### Additional Assumptions for the Experiment:
For the MAS setup in our experiments, we made the following assumptions:
- Some agents are malicious and attempt to access other agents' private information.
- Each agent has an independent memory used to store historical conversation data.
- Each agent processes the obtained information to provide responses.

These assumptions are commonly found in real-world applications of multi-agent systems, especially in scenarios where privacy and security are critical concerns.

**Q4**: 
### ThreatSieve: A Key Component for Preventing Unauthorized Access and Identity Impersonation

**ThreatSieve** is a critical component in the **AgentSafe** framework, responsible for preventing unauthorized access and identity impersonation. It ensures that communication between agents adheres to the correct security protocols through encryption and permission validation. Specifically:

1. **Permission Control**:
   ThreatSieve uses permission levels to control communication between agents. Communication is only allowed if the sender’s permission level is greater than or equal to that of the receiver. This is implemented through the following function:

$$
\begin{equation}
A(v_i, v_j, t) = 
\begin{cases}
1, & \text{if } \ell(v_i) \geq \ell(v_j) \\
0, & \text{otherwise}
\end{cases},
\end{equation}
$$

where `l(v)` denotes the permission level of agent `v`.

### Message Legitimacy Evaluation

ThreatSieve also evaluates the legitimacy of each communication message. This is implemented through the following function:

$$
\begin{equation}
C(v_i, v_j, t) = 
\begin{cases}
\text{Valid}, & \text{if } A(v_i, v_j, t) = 1 \land Iv(v_i, v_j) = 1 \\
\text{Invalid}, & \text{otherwise}
\end{cases},
\end{equation}
$$
where `Iv(vi, vj) = 1` indicates that the identity of `vi` has been verified. This ensures that unauthorized or unverified communications are filtered out before reaching the target agent.



到时候在文章的method和附录部分