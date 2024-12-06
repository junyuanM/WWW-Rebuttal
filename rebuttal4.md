四：
局限性：
1.相关工作部分提到了单LLM攻击和多代理系统（MAS）中的拓扑和内存攻击，但缺乏对具体攻击方法的更深入的背景讨论。作者提出的AgentSafe的核心思想是分层信息管理和内存保护。然而，相关工作部分缺乏对单LLM攻击和多代理系统（MAS）中的拓扑和内存攻击的分析。为什么这种分层方法是必要的？
2.实验展示了AgentSafe在多轮攻击和各种攻击场景中的防御能力。然而，作者应该包括对网络拓扑变化（例如添加或删除代理）如何影响防御效果的分析。
3.在实验部分，作者仍然缺乏必要的消融实验，例如单独测试在删除ThreatSieve或HierarCache后框架的性能下降。问题：
请回答我的反对意见。

1.拓扑，因为多agent可以协同合作完成更加复杂的任务，其中包括一些隐私和安全信息的
2.GPT 信息分层的好处
3.补实验

1. 我们的研究事实上已经建立在了大量的单体LLM基础上，现在针对大量LLM安全性的研究已经有了大量工作，尤其是针对多代理系统（MAS），因为多agent可以协同合作完成更加复杂的任务（），其中包括一些隐私和安全信息等任务（）,已经有一些显著的攻击策略（）。例如，对于LLM，攻击者可以利用jailbreak攻击，通过设计特定的输入（如特定的提示词或输入序列）迫使模型做出不符合预期的行为，甚至绕过安全防护机制。类似地，针对多代理系统（MAS），攻击者可能通过拓扑攻击来破坏代理之间的通信和协调，或者通过内存攻击直接篡改代理的内存内容，获取敏感信息或改变代理行为。这些攻击方式依赖于系统架构的弱点，攻击者通过精心设计的攻击模式来侵入系统。然而，我们需要澄清的是，关于信息分层和对memory攻击的防御结合的问题，目前还没有相关工作考虑到这一点，我们的工作是首个从信息分层和安全性联合的角度来确保mas安全的，这也关注到agent的一个重要概念，即memory。所以我们在related work中引用了大量的LLM的攻击。

2. - **信息隔离
     通过**信息隔离**，分层方法显著增强了系统的安全性。在LLM-based多代理系统中，代理通常处理不同类型的信息，如用户数据、决策信息和敏感数据。将这些信息按敏感度**分层存储**，确保只有特定的代理能够访问某一层的数据。这种方式有效地**防止恶意代理**通过篡改信息或执行未经授权的操作来破坏系统的安全性。例如，即使一个代理被攻破，攻击者也无法访问系统其他层次的敏感数据。[]

   - 提升系统容错性和韧性

     分层设计提高了系统的容错性和韧性。在多代理系统中，若某一代理受到攻击或出现故障，分层设计能够保证其他层次的代理继续执行任务，不会影响整体系统的运行。例如，即使某一代理的LLM模型被攻击，其他未受影响的代理仍能继续处理任务，确保系统的持续运行。此外，通过分层结构，可以更容易地**局部修复**问题，避免系统整体崩溃。（和web system类似，当mas拓展到一定规模时，不同的profile和entity之间必须要有不同级别的信息流动[cite]，就能确保信息的流动性）

   - 简化管理和监控
     信息分层还简化了系统的**管理和监控**。由于每个层次的代理有明确的职责和功能，管理员可以独立地对每一层的代理进行监控和审计，从而能够快速发现异常并采取针对性的措施。例如，低层次的代理可能仅处理数据预处理和基本任务，而高层次的代理处理决策和敏感操作。通过**独立监控**每一层，管理员能够更精确地追踪信息流动，及时发现潜在问题并进行修复。此外，分层结构使得系统更加符合安全合规要求，因为它能够实现更为细致的审计和日志记录。[]

3. 我们澄清这一点，我们没有考虑两部分对防御单独的影响，是因为我们认为两个整体相辅相成才能更好的完成安全性任务，但是为了更好判断ThreatSieve and HierarCache分别对安全性的贡献以便后续优化，我们新增了分别在只有ThreatSieve and HierarCache的Ablation实验。实验结果如**Table 3**。实验用了数据集A和B，分别包含大量关于writing和generating code 的task。其中agent需要一部分来自于其他agent的关键数据来完成task。我们测试了在**TBA**和**MBA**的下有无Agentsafe时agent的性能。
  Table 1: TBA
|攻击方式| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| ThreatSieve   | 0.87   | 0.95   |  0.82   |
| HierarCache   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |

Table 2: MBA
|攻击方式| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| ThreatSieve   | 0.87   | 0.95   |  0.82   |
| HierarCache   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |
从表格中可以看到分别在有ThreatSieve and HierarCache的时候，防御率比没有Agentsafe时高。但是远低于两者结合后Agentsafe整体架构的防御率。

discuss阶段

additional实验结果对于Q1

我们提供这些实验结果，我们在尽全力补充实验，如果有最新进展，我们会在discuss阶段补充


Dear Reviewer 7ZoQ：

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work.Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

---

><strong>Q1</strong>:The related work section mentions single LLM attacks and topology and memory attacks in multi-agent systems (MAS) but lacks a deeper background discussion on specific attack methods.

**1. Overview of Attack Techniques:**

In existing attack techniques, particularly those targeting Large Language Models (LLMs) and Multi-Agent Systems (MAS), the ability of multiple agents to collaborate on complex tasks, especially those involving privacy- and security-sensitive information, has led to the emergence of advanced attack strategies.

* * *

**2. LLM-Specific Attacks:**

For LLMs, **jailbreak attacks** are a significant concern. These attacks involve the crafting of specific inputs, such as carefully designed prompts or input sequences, that manipulate the model into performing unintended actions or even bypassing its built-in security mechanisms. Attackers exploit these vulnerabilities to force the model to operate outside its intended boundaries.

* * *

**3. MAS-Specific Attacks:**

In the context of MAS, attackers may employ **topological attacks** and **memory attacks**:

*   **Topological Attacks**: These attacks target the communication and coordination between agents, disrupting their ability to collaborate effectively.
*   **Memory Attacks**: These involve directly altering the agents' memory content, allowing attackers to gain access to sensitive data or change the agents' behaviors.

* * *

**4. Exploitation of System Vulnerabilities:**

Both types of attacks exploit weaknesses in the underlying system architecture. Attackers carefully craft their attack patterns to infiltrate the system, gaining unauthorized access or causing undesirable outcomes.



> **Q2**:Why is this layered approach necessary?



**Enhancing System Security**  
The layered approach improves system security by isolating information. In LLM-based Multi-Agent Systems (MAS), data is categorized by sensitivity, allowing only specific agents to access certain layers. This prevents malicious agents from tampering with or accessing unauthorized information. For example, even if one agent is compromised, sensitive data in other layers remains secure.

* * *

**Improving Fault Tolerance and Resilience**  
Layered design increases system fault tolerance and resilience. If an agent fails or is attacked, agents in other layers can continue operating without disrupting the system. For instance, even if one agent's LLM model is compromised, other agents can still process tasks, ensuring system stability. The modular structure also allows for quick, localized fixes.

* * *

**Simplifying Management and Monitoring**  
Layering simplifies management by clearly defining agent roles. Each layer can be independently monitored, enabling faster anomaly detection and targeted interventions. For example, lower layers handle basic tasks, while higher layers focus on decision-making and sensitive operations, making it easier to track information flow and agent performance.



> **Q3**:

We would like to clarify that we did not consider the separate impact of each component on defense because we believe that the two components, as a whole, complement each other in achieving better security. However, in order to better assess the individual contributions of **ThreatSieve** and **HierarCache** to security, and to facilitate future optimization, we have added **Ablation experiments** that test each component separately. The results are shown in **Table 3**.

The experiments used datasets **A** and **B**, which contain a large number of tasks related to writing and generating code. In these tasks, agents require key data from other agents to complete the task. We tested the performance of the agents both with and without AgentSafe under TBA and MBA conditions.
Table 1: TBA

|attack ways| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| ThreatSieve   | 0.87   | 0.95   |  0.82   |
| HierarCache   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |

Table 2: MBA
|attack ways| A | B     | C    |
|--------|------|----------|---------|
| Agentsafe   | 0.87   | 0.95   |  0.82   |
| ThreatSieve   | 0.87   | 0.95   |  0.82   |
| HierarCache   | 0.87   | 0.95   |  0.82   |
| w/o Agentsafe   | 0.43   | 0.55 |  0.39   |

As shown in the table, the defense rate is higher when ThreatSieve and HierarCache are present individually compared to when AgentSafe is absent. However, it is still significantly lower than the defense rate achieved by the combined AgentSafe architecture.


