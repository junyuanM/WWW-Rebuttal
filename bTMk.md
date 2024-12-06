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

### Message Legitimacy Evaluation

ThreatSieve also evaluates the legitimacy of each communication message. This is implemented through the following function:

$$
C(v_i, v_j, t) = 
\begin{cases}
\text{Valid}, & \text{if } A(v_i, v_j, t) = 1 \land Iv(v_i, v_j) = 1 \\
\text{Invalid}, & \text{otherwise}
\end{cases}
$$

where `Iv(vi, vj) = 1` indicates that the identity of `vi` has been verified. This ensures that unauthorized or unverified communications are filtered out before reaching the target agent.

[1]MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework

[2]CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society

Sinercely,

Authors
