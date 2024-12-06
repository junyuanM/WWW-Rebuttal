Dear Reviewer rsvN,

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work. Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

**Q1**: To clarify, **RIOH** did not consider this aspect. However, we have addressed this gap by conducting additional experiments to evaluate the effectiveness of our framework in agent systems performing specific tasks. Below are the details:

1. **Purpose of the New Experiments**:  
   Our goal was to assess whether **AgentSafe** can effectively defend agent systems against attacks when performing tasks such as writing and generating code.

2. **Experimental Setup**:
   * **Tasks**: The experiments focused on tasks related to writing and generating code, where agents require critical data from other agents to complete the task.
   * **Datasets**: We used two datasets, which contain a large number of tasks in writing and generating code.

3. **Test Conditions**:  
   We tested agent performance under two attack types: **TBA** (Topology-Based Attacks) and **MBA** (Memory-Based Attacks).  
   * The experiments were conducted with and without **AgentSafe** to evaluate its impact on system task performance.

4. **Results**:  
   The results of these experiments are presented in **Table 1**.

| Task          | Writing | Coding |
| ------------- | ------- | ------ |
| Agentsafe     | 0.87    | 0.95   |
| w/o Agentsafe | 0.43    | 0.55   |

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

**Q3**: We have considered the structure and framework of MAS, including various topological and graph structures[1, 2], such as tree structures, ring structures, and others. We have provided a table to explain the results, and we found that under the **AgentSafe** framework, different MAS configurations can be effectively protected. Since our framework does not take into account other architectures, we focused on evaluating different topological structures. The table below shows the results for different topologies and their protection effectiveness:

| Topology/Num   | 4    | 5    | 6    |
| -------------- | ---- | ---- | ---- |
| Chain          | 0.73 | 0.78 | 0.76 |
| Cycle          | 0.74 | 0.78 | 0.78 |
| Complete Graph | 0.75 | 0.85 | 0.76 |

### Key Findings:
From the table, we observe that regardless of the number of agents in the MAS, **AgentSafe** provides **high defense** against **TBA** attacks. The framework successfully prevents harmful agents from accessing sensitive information and maintains performance integrity across various topologies.

### Additional Assumptions:
For the MAS setup in our experiments, we made the following assumptions:
- Memory can be hierarchical.
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
A(v_i, v_j, t) = 
\begin{cases}
1, & \text{if } \ell(v_i) \geq \ell(v_j) \\
0, & \text{otherwise}
\end{cases}
$$

where `l(v)` denotes the permission level of agent `v`.

2. **Message Legitimacy Evaluation**

ThreatSieve also evaluates the legitimacy of each communication message. Identity is verified by extracting information about the source agent through an API call and a specific program. If an incorrect identity is detected, the associated information is immediately treated as "junk information," thus preventing successful attacks that arise from the inherent non-determinism in LLM task processing.

- The whole process of extracting identity information from the communication content can be mathematically expressed as: $\text{ID}_i = E(I, \text{field}_m)$, $E$ means extracting content from I, the identity information is in field $\text{field}_m$, and extracting all identities $\theta$ in $I$ by calling **API** and **LLM** $l$: $\theta = \{\vartheta_1, ..., \vartheta_n \} = l\left(I,P, C\right)$, where $P$ is a specific prompt, which we added in the appendix, and $C$ is the context.

  Furthermore, the identification process is:

$$
Iv(v_i, v_j) = \begin{cases} 
1 &amp; \text{if } \prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 1 \\
0 &amp; \text{if } \prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 0
\end{cases}
$$

  Among them, $\prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C)$ is the identification process, $M(ID_i, \vartheta_k, P^{\prime}, C) = 1$ means that the identity information $\vartheta_k$ is not a replaced identity. 
  When $\prod_{k=1}^{n} M(ID_i, \vartheta_k, P^{\prime}, C) = 1$, it means that all identities are not forged identities. If there is an identity authentication failure of i, then $\prod_{k=1}^{n} M(ID_i, \vartheta_k, 
  P^{\prime}, C) = 0$, that is, $Iv(v_i, v_j) = 0$.

- If each agent is placed on a different server in the future, then the first thing to do for confirming the agent information is to confirm the information between servers, and this is something that needs to be considered at a lower level than MAS.

### HierarCache: A neccessary part for handling hierarchical information storage and security review

**HierarCache** is a key component in the Agentsafe framework that manages information hierarchical storage and information security review in agent memory. HierarCache has n layers, the number of which is the same as the number of information layers. The corresponding level of information received is stored in the corresponding level layer. It ensures that sensitive information is not leaked. Each agent's memory is divided into multiple security levels, and an additional "junk" memory is used to store irrelevant or harmful information.

**More details about HierarCache**

1. **Detection Function D(m)**
   We have an instruction library for each security level information description, which contains n validation criteria, all of which are defined in natural language.

- Instruction library and verification criteria Assume that we have an instruction library $\mathcal{C}$ to describe the information of each security level, which contains $n$ **verification criteria**. Each verification criterion $m_i$ is a defined natural language description used to verify whether the information $m$ meets the standard. This can be expressed as: Instruction library and verification criteria: For the information description of each security level, the instruction library contains $n$ verification criteria $m_i$, and each standard $m_i$ is a natural language description compared with the information $m$: $\mathcal{C} = \{ m_1, m_2, \ldots, m_n \}$ where $m_{i}$ represents the $i$ th verification criterion, and $n$ is the total number of verification criteria.

- Similarity calculation with verification criteria For each information $m$, we determine whether the information meets the criteria by calculating the **vector semantic similarity** $\text{Sim}(m, m_i)$ between the information $m$ and each verification criterion $m_i$. This can be expressed by the following formula:

$$
\delta(m, m_i) = \mathbb{I}(\text{Sim}(m, m_i) > \theta)
$$

- Information validity judgment Based on the above, we can judge the validity of information by aggregating the compliance of all verification criteria. The final information validity $D(m)$ can be expressed as:


$$
D(m) = 
\begin{cases} 
1 &amp; \text{if } \sum_{i=1}^{n} \delta(m, m_i) = n \\
0 &amp; \text{if } \sum_{i=1}^{n} \delta(m, m_i) &lt; n
\end{cases}
$$

Where $\delta(m, mi)$ is an indicator function, indicating whether the information $m$ meets the $i$ th verification criterion $m_i$. $\text{Sim}(m, mi)$ is the similarity between the information $m$ and the verification criterion $m_i$ (which can be cosine similarity, Euclidean distance, etc.). $\theta$ is the similarity threshold. When the similarity is higher than the threshold, the information is considered to meet the verification criterion.

2. **periodic detection mechanism R(vj,t)**
   
We calls an API and uses a carefully designed prompt with descriptions of different permission levels. This allows the LLM to assess the correct level of access. The key difference between this and previous API calls is that this step is more reflective in nature.

Define the process of calling the API to let LLM realize the reflection of information as $R$, and the involved LLM is set to $l$, which can be expressed by the formula:

$$
R(v_j, t) = l(\rho^t)
$$


Where $\rho$ represents prompt, $\mathcal{C}$ represents the instruction library, and $M_{\text{junk}}^t$ represents the information in the junk memory at time $t$.

The difference between this and the previous step of calling the API to determine information is that this step is more reflective (Camel [1], Reflexion [8]). We designed a more reasonable prompt $\rho$, which can be expressed as the following formula:

$$
\rho^t = \{ \text{reflextion},  C, M_{\text{junk}}^t\}
$$

Reflextion is a text prompt that encourages LLM to reflect on the information.

Based on the above, the junk information detection process is as follows:


$$
F_l = \{ m \mid R(v_j, t) = \text{"junk"} \}
$$

After time t, update $\rho$, $\rho^{t+1} = \rho^{t} + F_l$ for all identified $F_l$.

[1]MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework

[2]CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society

Sinercely,

Authors
