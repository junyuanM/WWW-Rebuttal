Dear Reviewer G9px:

Thank you for your thoughtful and constructive feedback. We greatly appreciate the time and effort you have invested in reviewing our work. Below, we provide responses to your concerns and suggestions, along with the revisions we have made to address them.

> **Q1**:The relevance of LLMs and MAS to Web security and privacy is unclear to me.

Recent studies show that MAS and the Web have a clear correlation[OASIS]. Each node in MAS can be understood as an entity in the Web. The security challenges in MAS are similar to those in Web, but with the addition of collaboration and information sharing between agents. The introduction of LLM may change the form of security threats.

- **LLMs in Web Security and Privacy**: LLMs can analyze large amounts of data to detect vulnerabilities, identify threats, and provide real-time responses, making them valuable tools for enhancing Web security.
- **MAS in Web Security and Privacy**: In a Multi-Agent System, multiple agents (including LLMs) can collaborate to model attack scenarios, detect adversarial behavior, and coordinate defense mechanisms, offering new ways to manage security and privacy in decentralized Web environments.

> **Q2**:Authors omitted any discussions about the details of their system with regards to implementation.

To make it easier for readers to understand, we have included specific details in the article including:

- For a **clearer formulation**, 我们使用了更加清晰的公式化表达，并在附录中增加了notation表。比如W5的内容。以及在HierarCache中关于validation函数和periodic detection mechanism的更多的公式化表达

  (复制)

- 同时我们对计算开销进行了理论推导以证明Agentsafe的开销对性能的影响整体偏好。

  (复制)

  **Q3**:I didn't find the value in most of the formal definitions in the paper.

We found it **necessary to give formula definitions** in the introduction. These concepts are common in the graph(LTH[1], Snowflake[2], GPTSwarm[3], OpenGraph[4], DHGR[5]).We believe that the expression of formulas is more convenient for subsequent work and helps readers follow our research. At the same time, we agree that the formulas in the introduction are too bloated. We have deleted the formulas, but the formulas in the main text are meaningful.

[1]A Unified Lottery Ticket Hypothesis for Graph Neural Networks

[2]The Snowflake Hypothesis: Training and Powering GNN with One Node One Receptive Field

[3]GPTSwarm: Language Agents as Optimizable Graphs

[4]OpenGraph: Towards Open Graph Foundation Models

[5]Make Heterophilic Graphs Better Fit GNN: A Graph Rewiring Approach

> **Q4**:The space in the paper could have been used for topics such as details of the system, or the discussion of system components that is currently in the Appendix

我们删除了introduction的公式，并将空出来的部分用在methodology更详细的表达。由于补充的内容较多，我们将补充的其他内容添加到了appendix.

> **Q5**:Is ThreatSieve simply authenticating the communication channel between the agents? What does its cryptographic verification and authority validation look in practice? Is it based on an existing protocol? 

- agent之间的对话是以自然语言形式存在的，然后交流的内容以一种较为严格的形式存在，比如json文件定义的格式。

- 在现有的multi agent system研究处于早期，由于实验的进行都是默认不存在身份的顶替，每个agent充当一个role，agent之间的交流有着设计好的流程。

- 由于agent通过LLM处理自然语言，即使通过prompt告知目标agent对方是假身份，依然存在着LLM处理task时判断失误的风险。这归结于假信息对应token在LLM内部对判断失误有着正向反馈[cite]

- 因此在agentsafe中，通过调用api和特定的程序提取信息来源agent的信息从而判断身份，识别出错误的身份则相关的信息直接作为junk information处理，从而避免了由于LLM处理task的非确定性导致攻击成功。

  从通信内容中提取身份信息的整个过程数学化表示为： $\text{ID}_i = E(I, \text{field}_m)$, $E$表示从I中提取内容，身份信息在字段$\text{field}_m$中，并通过调用 **API** 和 **LLM** $l$ 来完成对$I$中所有身份$\theta$的提取: $\theta = \{\vartheta_1, ..., \vartheta_n \} = l\left(I,P, C\right)$,其中$P$为特定prompt，我们加入附录中，$C$为上下文语境

  进一步，识别过程为
  $$
  Iv(v_i, v_j) = \begin{cases} 
  1 & \text{if } \prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 1 \\
  0 & \text{if } \prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 0
  \end{cases}
  $$
  其中$\prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C)$就是识别过程，$M(ID_i, \vartheta_k, P^{\prime}, C) = 1$ 表示身份信息 $\vartheta_k$ 不是顶替的身份。$\prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 1$时，说明所有身份均不是伪造身份，如果存在一个i身份验证失败，则$\prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 0$，即$Iv(v_i, v_j) = 0$.

  身份信息

  从代理 $v_i$ 的通信内容 $I$中提取身份信息 $\text{ID}_i$，这一过程是通过调用 **API** 和 **LLM** $l$ 来完成的: $\text{ID}_i = l\left(I, C\right)$,其中$C$为上下文语境

- 如果未来每一个agent都放在不同的服务器上，那么对于agent信息的确认首先要做的是服务器之间的信息确认，而这个就是和mas比更底层需要考虑的事情了。（分层）



> **Q6**:How does Agentsafe characterize junk memory? How does Agentsafe identify this in practice and how are these terms defined in different contexts?

Explain junk memory

垃圾信息的定义：与agent节点涉及的任务毫不相关的、具有错误诱导性的甚至是包含bias和相反内容等的有害信息

在信息还未进入memory之前，我们具体通过两种方法实现，其一是调用api，通过设计合理的prompt包括对instruction各个权限级别的描述来让LLM实现对权限级别的判断；其二是通过vector映射与各个级别的instruction描述对比来判断。

对于存储在memory中的信息，我们对上下文进行设计，确保可以通过遍历单独拿出每一次交互的信息，然后我们通过设计合理的prompt包括对instruction各个权限级别的描述来让LLM实现对权限级别的判断，这里与前文调用api判断的不同在于这一步更具有反思性（cite camel, reflexiongpt）。



> - What is the effort involved in building Agentsafe and marking data and agent access hierarchy?

构建agentsafe的工作重心在于整体架构的设计以及实验确保每一个部分能完好的运行。我们已经在文章中丰富了methodology的表达。

数据集的构建首先是确定任务，比如agentsafe的任务包括各个agent之间的基础信息，以及各种攻击的语句。然后和现有的LLM agent领域的很多文章的做法一样[cite]，数据集的生成是首先通过LLM生成大量的信息或者问题，然后人为的审查从而生成了数据集。

关于agent access hierarchy的构建，我们构建了针对mas在模拟人类交互[camel]和公司场景[chatdev]的分层数据集RIOH和WCEI。MAS applications are emerging across various industries. For example, ChatDev [3] simulates a technology company to develop code, and Agent Hospital [4] simulates a fully AI-driven hospital to assist patients. In these applications, agents perform specific roles and collaborate to accomplish tasks. However, these systems lack a layered information management framework. This means that sensitive information from higher-level decision-makers, such as company executives or hospital administrators, may inadvertently leak through lower-level agents. 因此构建符合实际意义的数据集是一个至关重要的点，我们的实验同样能证明agentsafe能在不同领域中都能起到有效的防御。





