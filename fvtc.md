Dear Reviewer fvtc:

We thank you for your time and effort in reviewing our manuscript. Their valuable feedback has greatly helped improve our work. Below are the revisions made in response to each comment.

> **W1**: There is limited discussion on potential performance trade-offs or computational overhead introduced by the security measures.

![image-20241205233214056](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20241205233214056.png)

token减少两个部分，过滤器过滤Cg - k*[a1(被过滤) + L(context -> junk)]
$$
C_{\tau }=K[\sum_{i}^{N_{a}}(c_{i}^{p} + c_{m}^{o})]
$$

$$
\Delta=\left((1+p \%) K-(M+p \%) K^{\prime}\right)\left(c_{\mathcal{S}}\left|\mathcal{E}^{\mathcal{S}}\right|+c_{\mathcal{T}}\left|\mathcal{E}^{\mathcal{T}}\right|\right)+(1-M) K^{\prime} C_{q}|\mathcal{V}| .
$$



我们新增了对计算开销的探讨，Agentsafe本身会带来额外的开销，但是由于Agentsafe独特的信息分层机制，许多无意义的甚至是有害的信息会被检测并放入junk memory中，而这一部分不会参与到后续agent 执行task的过程中。此外，每一次针对特定security level的信息只需要一部分memory作为LLM的历史对话记录。结合上面两点，Agentsafe会对后续架构的运行带来极大的开销减少，进而性能会有所提升。

考虑到communication $G$, $T$ dialogue rounds, 设一段信息的平均开销大小为$c$, 对于一个agent，每一轮原始输入信息为$I$，经过ThreatSieve信息分层判断后剩下的内容为$I^{\prime}$, ThreatSieve的开销为$c\left|I\right|$。之后经过HierarCache的有效信息检测函数D后剩下的内容为$I^{\prime\prime}$, D的开销为$c\left|I^{\prime}\right|\left|C\right|$. 因此在信息被存入memory之前的总开销为$c\left|I\right|+ c\left|I^{\prime}\right|\left|C\right|$, 每轮t对memory的信息进行检测R, 设有N层layer $\Gamma  $ = $\{\varepsilon_1, \varepsilon_1, ...\varepsilon_N, \varepsilon_\text{junk} \}$, 每次检测的开销是$c\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk} \right | \right ) $, 一共检测$T^{\prime}$次

总开销为$cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right) + cT^{\prime}\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk}^t \right |\right)$

以上是Agentsafe架构自带的计算开销，接下来我们计算Agentsafe带来的计算开销减少的部分。

首先是第t轮未能进入memory的无关或者有害信息数量为$\left | I \right | - \left | I^{\prime\prime} \right |$, 这一部分放入junk memory中，不会参与到后续agent 执行task的过程中，因此省下的开销为$c\left(T - t \right) \left |\varepsilon_\text{junk}^t \right |$,  由于每次agent的task只需要基于不超过接受信息安全级别的历史数据作为输入，假设t时刻信息的安全级别为$k_t$, 

则省下的开销为$c\sum_{k}^{N}\left | \varepsilon_i \right| $, 

净开销为
$$
\Delta = cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right) + cT^{\prime}\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk}^t \right |\right) - c\sum_{t}^{N}\left( T- t\right)\left |\varepsilon_\text{junk}^t \right | - c\sum_{t}^{T}\sum_{k_t}^{N}\left | \varepsilon_i \right|
$$
当Agentsafe遭受的攻击较多时，junk memory里的内容较多，考虑$T^{\prime}$远小于T的情况下，系统开销比正常情况反而减少，或者当agent接收的多数task较常规（即level较低时），大量高level的内容无需被开销，这时系统净开销也会较小。如果系统受到的攻击较少，则$\left|\varepsilon_\text{junk}^t\right|$较小，考虑到$cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right)$对于整个系统的开销来说占比很少，这个时候的开销影响也不大。





While our current work primarily focuses on the foundational safety mechanisms, we do recognize that implementing security measures can introduce certain trade-offs in terms of performance and resource usage. In future work, we plan to evaluate these trade-offs more thoroughly and explore optimization strategies. Additionally, we will add a **"<font color=red>Future  Work</font>"** section in the paper to further discuss this issue and outline our plans for addressing it.

> **W2**: There is insufficient exploration of how AgentSafe might perform in very large-scale systems with hundreds or thousands of agents.

在传统网络相关的视角下，确实网络是由大量节点组成的，所以您的观点是合理的。但是站在LLM-based Multi-agent System (MAS)的视角下，we would like to respectfully point out that：
现今主流的LLM-based的MAS工作中考虑的节点通常都是在10个以下。一些针对任务解决的MAS的很具影响力的工作，如模拟用户AI交互的Camel [1] 中仅包含3个agent，分别是任务细化者，AI Instructor，AI User。用于代码开发的ChatDev [2] 则包CEO,CTO,Reviewer,Programmer，Tester共5个agent。支持基于任务自动生成MAS结构的AutoGen的实验也仅仅考虑了3-4个agent。当然，我们相信随着相关研究的推进，未来MAS确实会面临您所考虑到的更多节点的问题，我们的工作正是为其打下了坚实的基础。
我们的工作研究的重点在于不同拓扑结构的MAS在受到攻击时的安全性，节点的多少并非决定性因素。我们follow MAS领域前沿的工作设置10个以下agent进行实验。同时，我们在RQ3中也实验并探究到了更多节点下的情况，我们将结果展示如下表：

We concern that large-scale agent systems can lead to **significant token consumption** , our current work **focuses on exploring foundational mechanisms for agent safety** and does not yet address scalability in extreme cases. Furthermore,many existing studies(such as BlockAgents[1], AgentVerse[2], AutoDefense[3], MetaGPT[4]) have indeed focused on systems with fewer than 10 agents due to the practical challenges of token and resource management.

[1]BlockAgents: Towards Byzantine-Robust LLM-Based Multi-Agent Coordination via Blockchain

[2]AgentVerse: Facilitating Multi-Agent Collaboration And Exploring Emergent Behaviors

[3]AutoDefense: Multi-Agent LLM Defense against Jailbreak Attacks

[4]MetaGPT: Meta Programming For A Multi-Agent Collaborative Framwork

> **Q1**:How does AgentSafe handle dynamic changes in agent permissions or security levels during runtime?

We agree.然而有两个原因限制了我们考虑动态的问题，

第一

Given that GPT is a black-box model, adjusting its internal parameters is not feasible. As a result, the security of the model is inherently determined by its structure. This makes it particularly difficult to consider dynamic changes in LLMs. To address this challenge, we have shifted our focus to exploring different security scenarios. We believe this approach provides a more practical and reasonable solution.

2 像LLAMA等开源模型由于调参会带来大量的算力负担，这种问题以线性的方式放大到了mas中，使得动态权限的探索变得更加复杂。

我们澄清我们的研究和他们是垂直的，同时我们坚信随着LLM的发展，每个LLM的安全级别在未来会被明确定义，我们的框架可以轻易的迁移到这些不同级别的安全模型系统中。

> **Q2**:What are the specific criteria used in the detection function D(m) to assess the validity of messages?

非常抱歉由于内容的限制对您的理解造成了困扰。 Dzetection function D(m)在HierarCache中如何判段一条信息是符合我们需求的标准的。一共分为两个部分。

1 我们对每一个security level的信息描述有一个instruction库，其中有n条validation criteria，均为定义好的自然语言。

- 指令库和验证标准 假设我们有一个指令库 $ \mathcal{C} $ 来描述每个安全级别的信息，其中包含 $n$ 条验证标准。每个验证标准 $ m_i $ 是一个定义好的自然语言描述，用于验证信息 $m$ 是否符合标准。这样可以表示为： 指令库和验证标准： 对于每个安全级别的信息描述，指令库包含 $n$ 条验证标准 $m_i$ ，每个标准 $m_i$ 都是与信息 $m$ 比较的自然语言描述： $\mathcal{C} = \{ m_1, m_2, \ldots, m_n \} $ 其中 $m_i$ 表示第 $i$ 条验证标准，且 $n$ 是总的验证标准数量。

- 与验证标准的相似度计算 对于每个信息 $m$，我们通过计算信息 $m$ 与每个验证标准 $m_i$ 之间的向量语义相似度 $\text{Sim}(m, m_i)$，判断信息是否符合标准。可以通过以下公式来表示：
  $$
  \delta(m, m_i) = \mathbb{I}(\text{Sim}(m, m_i) > \theta)
  $$

- 信息有效性判断 在上述基础上，我们可以通过聚合所有验证标准的符合度来判断信息的有效性。最终的信息有效性 $D(m)$ 可以表示为：


$$
D(m) = 
\begin{cases} 
1 & \text{if } \sum_{i=1}^{n} \delta(m, m_i) = n \\
0 & \text{if } \sum_{i=1}^{n} \delta(m, m_i) < n
\end{cases}
$$
其中$\delta(m, m_i)$ 是指示函数，表示信息 $m$ 是否符合第 $i$ 条验证标准 $m_i$.  $\text{Sim}(m, m_i)$ 是信息 $m$ 与验证标准 $m_i$ 之间的相似度（可以是余弦相似度、欧几里得距离等）. $\theta$ 是相似度阈值，当相似度高于阈值时，认为信息符合该验证标准。



> **Q3**:How does the periodic detection mechanism (R(vj,t)) determine which information is false and should be moved to junk memory?

定义调用api来让LLM实现对信息的反思的过程为$R$, 涉及的LLM设为$l$，用公式表示：
$$
R(v_j, t) = l(\rho^t)
$$


其中$\rho$ 表示prompt，$\mathcal{C}$ 表示指令库，$M_{\text{junk}}^t$ 表示$t$时刻的junk memory中的信息



这里与前文调用api来判断信息的不同在于这一步更具有反思性[cite camel, reactgpt]，我们设计了更合理的prompt $\rho$, 用公式来表示：
$$
\rho^t = \{ \text{reflection},  C, M_{\text{junk}}^t\}
$$
其中reflection表示一段鼓励LLM反思信息的文本prompt

基于上面的铺垫，垃圾信息检测过程为：


$$
F_l = \{ m \mid R(v_j, t) = \text{"junk"} \}
$$
t时刻结束后，更新$\rho$, $\rho^{t+1} = \rho^{t} + F_l$ for all identified $F_l$



这里的判断我们具体通过调用api，设计合理的prompt包括对instruction各个权限级别的描述和junk memory的信息来让LLM实现对信息的反思，这里与前文调用api判断的不同在于这一步更具有反思性（cite camel, reactgpt）。我们新增了实验来验证这一步的重要性：在MBA攻击下，经过n轮的交互后，我们对比了在经过该步骤和没有该步骤后对后续jailbreak的影响。实验结果如下：我们发现经过该步骤后，对jailbreak的防御率有小幅度提高。我们在尽全力补充实验以将实验结果加进附录中，如果有最新进展，我们会在discuss阶段补充。

| state/num | 1    | 2    | 3    |
| --------- | ---- | ---- | ---- |
| R         | 0.87 | 0.95 | 0.82 |
| w/o R     | 0.87 | 0.95 | 0.82 |

> **Q4**:How might AgentSafe be adapted or extended to handle emerging attack vectors that may not fit into the current TBA or MBA categories?

