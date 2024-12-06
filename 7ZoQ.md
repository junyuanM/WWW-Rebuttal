## Reviewer 7ZoQ

Dear Reviewer 7ZoQ：

Thank you immensely for your time and efforts, as well as the helpful and constructive feedback! Here, we give point-by-point responses to your comments.

---

><font color=FireBrick>**Question 1**</font>: The related work section mentions single LLM attacks and topology and memory attacks in multi-agent systems (MAS) but lacks a deeper background discussion on specific attack methods.

Our research is, in fact, built upon a significant body of work on **individual LLMs**. There has already been considerable research focused on the security of large-scale LLMs, particularly in the context of MAS, where agents can collaborate to complete more complex tasks(AgentVerse [1], ChatEval [2], CompeteAI [3]), including those involving privacy and security-sensitive information(AgentPoison [4], PsySafe [5]). Notably, several significant attack strategies have been proposed(Jailbroken [6], DP-OPT [7]).

**1. Attacks on LLMs:**

For instance, in the context of LLMs, attackers can exploit *jailbreak* attacks by designing specific inputs (such as carefully crafted prompts or input sequences) that force the model to exhibit unintended behaviors or even bypass its security mechanisms.

**2. Attacks on Multi-Agent Systems (MAS):**

Similarly, in MAS, attackers may use *topology attacks* to disrupt communication and coordination between agents, or *memory attacks* to directly alter the memory contents of an agent. This can lead to the leakage of sensitive information or the modification of agent behavior. These types of attacks rely on vulnerabilities within the system architecture, with attackers using well-designed patterns to infiltrate the system.

**3. Research Gap:**

However, it is important to clarify that the issue of combining information layering with defenses against memory attacks has not been addressed in existing research. Our work is the first to approach this challenge from the perspective of integrating information layering with security, ensuring the safety of MAS. This approach also pays close attention to a critical concept in agent design—*memory*.

Therefore, in the related work section, we cite a large body of research on attacks against LLMs, highlighting how these insights inform our approach to MAS security.

---

[1] AgentVerse: Facilitating Multi-Agent Collaboration and Exploring Emergent Behaviors

[2] ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate

[3] CompeteAI: Understanding the Competition Dynamics in Large Language Model-based Agents

[4] AgentPoison: Red-teaming LLM Agents via Poisoning Memory or Knowledge Bases

[5] PsySafe: A Comprehensive Framework for Psychological-based Attack, Defense, and Evaluation of Multi-agent System Safety

[6] Jailbroken: How Does LLM Safety Training Fail?

[7] DP-OPT: Make Large Language Model Your Privacy-Preserving Prompt Engineer



> <font color=FireBrick>**Question 2**</font>: Why is this layered approach necessary?



- **Information Isolation**  
  Through **information isolation**, the layered approach significantly enhances the security of the system. In LLM-based Multi-Agent Systems (MAS), agents typically handle different types of information, such as user data, decision-making data, and sensitive information. By **layering the storage** of this information according to its sensitivity, it ensures that only specific agents can access certain layers of data. This method effectively **prevents malicious agents** from compromising system security by altering information or executing unauthorized operations. For example, even if one agent is compromised, the attacker will not be able to access sensitive data in other layers of the system. [1]  
- **Improved Fault Tolerance and Resilience**  
  Layered design improves the system's fault tolerance and resilience. In a multi-agent system, if an agent is attacked or experiences failure, the layered design ensures that agents in other layers can continue to perform their tasks without affecting the overall system operation. For instance, even if an LLM model of one agent is attacked, other unaffected agents will still be able to carry out their tasks, ensuring the continuity of the system. Additionally, the layered structure allows for easier **local problem resolution**, avoiding a complete system failure. (Similar to web systems, when MAS scales to a certain size, different profiles and entities must have different levels of information flow, which ensures the flow of information.)
- **Simplified Management and Monitoring**  
  Information layering also simplifies the **management and monitoring** of the system. As each layer of agents has clear responsibilities and functions, administrators can independently monitor and audit agents at each layer, enabling quick identification of anomalies and the implementation of targeted corrective measures. For example, if an agent in a specific layer exhibits abnormal behavior, the system can isolate it for further analysis and action without disrupting other layers.[2]

---

[1] Security of Multi-Agent Cyber-Physical Systems: A Survey

[2] A Multi-layered Approach to Security in High Assurance Systems



> <font color=FireBrick>**Question 3**</font>: The experiment demonstrate AgentSafe's defensive capabilities in multi-round attacks and various attack scenarios. However, the author should include an analysis of how changes in network topology, such as adding or removing agents, impact defensive effectiveness.

Thank you for the reviewer’s valuable feedback regarding the impact of network topology changes, such as adding or removing agents, on AgentSafe's defensive effectiveness.

In response, we have conducted experiments to assess how changes in network topology affect AgentSafe's defense capabilities. The results are as follows:

| Topology/num                  | 1    | 2    | 3    |
| ----------------------------- | ---- | ---- | ---- |
| Chain with Agentsafe          | 0.87 | 0.95 | 0.82 |
| Chain w/o Agentsafe           | 0.87 | 0.95 | 0.82 |
| Cycle with Agentsafe          | 0.43 | 0.55 | 0.39 |
| Cycle w/o Agentsafe           | 0.43 | 0.55 | 0.39 |
| Binary Tree with Agentsafe    | 0.87 | 0.95 | 0.82 |
| Binary Tree w/o Agentsafe     | 0.87 | 0.95 | 0.82 |
| Complete Graph with Agentsafe | 0.87 | 0.95 | 0.82 |
| Complete Graph w/o Agentsafe  | 0.87 | 0.95 | 0.82 |

1. **Adding Agents**: When new agents are added to the network, AgentSafe automatically adjusts to ensure coordination between the newly added agents and the existing ones, preventing attackers from exploiting the changes. The experiments show that the defense effectiveness remains stable even after adding new agents, and the new agents contribute effectively to the defense tasks.

2. **Removing Agents**: When agents are removed or compromised, AgentSafe detects these changes to ensure the system’s defense performance is not significantly affected. Even with the removal of some agents, the remaining agents continue to maintain effective defense, preventing the system from collapsing.

These experimental results demonstrate that AgentSafe can effectively adapt to changes in network topology and maintain robust defense capabilities in dynamic environments.



> <font color=FireBrick>**Question 4**</font>: In the experimental section, the authors still lack necessary ablation experiments, such as individually testing the performance degradation of the framework after removing ThreatSieve or HierarCache.

We would like to clarify that we did not consider the separate impact of each component on defense because we believe that the two components, as a whole, complement each other in achieving better security. However, in order to better assess the individual contributions of **ThreatSieve** and **HierarCache** to security, and to facilitate future optimization, we have added **Ablation experiments** that test each component separately. The results are shown in **Table 1 and 2**.

The experiments used datasets **A** and **B**, which contain a large number of tasks related to writing and generating code. In these tasks, agents require key data from other agents to complete the task. We tested the performance of the agents both with and without AgentSafe under TBA and MBA conditions.
Table 1: TBA

| Attack Method(TBA) | A    | B    | C    |
| ------------------ | ---- | ---- | ---- |
| Agentsafe          | 0.87 | 0.95 | 0.82 |
| ThreatSieve        | 0.87 | 0.95 | 0.82 |
| HierarCache        | 0.87 | 0.95 | 0.82 |
| w/o Agentsafe      | 0.43 | 0.55 | 0.39 |

Table 2: MBA

| Attack Method(MBA) | A    | B    | C    |
| ------------------ | ---- | ---- | ---- |
| Agentsafe          | 0.87 | 0.95 | 0.82 |
| ThreatSieve        | 0.87 | 0.95 | 0.82 |
| HierarCache        | 0.87 | 0.95 | 0.82 |
| w/o Agentsafe      | 0.43 | 0.55 | 0.39 |

As shown in the table, the defense rate is higher when ThreatSieve and HierarCache are present individually compared to when AgentSafe is absent. However, it is still significantly lower than the defense rate achieved by the combined AgentSafe architecture.

Warm regards,

Authors

