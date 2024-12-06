## Reviewer G9px

Dear Reviewer G9px:

Thank you for your thoughtful and constructive feedback. We greatly appreciate the time and effort you have invested in reviewing our work. Below, we provide responses to your concerns and suggestions, along with the revisions we have made to address them.

> <font color=FireBrick>**Question 1**</font>: The relevance of LLMs and MAS to Web security and privacy is unclear to me.

We believe that MAS and the Web have a clear correlation, because each node in MAS can be understood as an entity in the Web. The security challenges in multi-agent systems are similar to those in Web systems, but with the addition of collaboration and information sharing between agents. The introduction of LLM may change the form of security threats.

- **LLMs in Web Security and Privacy**: LLMs can analyze large amounts of data to detect vulnerabilities, identify threats, and provide real-time responses, making them valuable tools for enhancing Web security.
- **MAS in Web Security and Privacy**: In a Multi-Agent System, multiple agents (including LLMs) can collaborate to model attack scenarios, detect adversarial behavior, and coordinate defense mechanisms, offering new ways to manage security and privacy in decentralized Web environments.

> <font color=FireBrick>**Question 2**</font>: Authors omitted any discussions about the details of their system with regards to implementation. 

To make it easier for readers to understand, we have included specific details in the article including:

- For a **clearer formulation**, we use the term threatsieve and hiarcache in this article to represent... At the same time, we optimized the... (formula) and added the... **notation table** in the appendix.
- The **technical details are clearer** (the first work to consider MAS layering; the first systematic consideration of memory security issues and security frameworks; threatsieve uses..., junk memory...)

> <font color=FireBrick>**Question 3**</font>: I didn't find the value in most of the formal definitions in the paper.

We found it **necessary to give formula definitions** in the introduction. These concepts are common in the graph(LTH [1], Snowflake [2], GPTSwarm [3], OpenGraph [4], DHGR [5]).We believe that the expression of formulas is more convenient for subsequent work and helps readers follow our research. At the same time, we agree that the formulas in the introduction are too bloated. We have deleted the formulas, but the formulas in the main text are meaningful.

[1] A Unified Lottery Ticket Hypothesis for Graph Neural Networks

[2] The Snowflake Hypothesis: Training and Powering GNN with One Node One Receptive Field

[3] GPTSwarm: Language Agents as Optimizable Graphs

[4] OpenGraph: Towards Open Graph Foundation Models

[5] Make Heterophilic Graphs Better Fit GNN: A Graph Rewiring Approach

> <font color=FireBrick>**Question 4**</font>: The space in the paper could have been used for topics such as details of the system, or the discussion of system components that is currently in the Appendix

We will add ... in the appendix

> <font color=FireBrick>**Question 5**</font>: Is ThreatSieve simply authenticating the communication channel between the agents? What does its cryptographic verification and authority validation look in practice? Is it based on an existing protocol? 

1. **Identity Verification in MAS**  
   In our framework, agents communicate in natural language, and the exchanged content is structured, such as in JSON files. Currently, research in Multi-Agent Systems (MAS) is still in its early stages, and experiments typically assume that identity spoofing does not occur. Each agent takes on a specific role, and communication follows a pre-designed process.

2. **Identity Verification Process**  
   To verify identity, we call APIs and extract information from the source agent to determine its identity. However, because the content is in natural language, even if we inform the target agent of an impostor, there remains a risk of misjudgment during task processing by the LLM.

3. **Handling Incorrect Identity**  
   In **AgentSafe**, if an incorrect identity is detected, the related information is immediately treated as junk information. This helps prevent attacks that may succeed due to the non-deterministic behavior of LLMs when processing tasks.

4. **Future Considerations for Server-Based Agents**  
   If each agent is placed on a different server in the future, then the first thing to do for agent information confirmation is to confirm the information between servers, and this is something that needs to be considered at a lower level than MAS.

> <font color=FireBrick>**Question 6**</font>: How does Agentsafe characterize junk memory? How does Agentsafe identify this in practice and how are these terms defined in different contexts?

We appreciate your feedback and would like to clarify the process:

1. **Definition of Junk Information**:  
   Junk information refers to harmful content that is irrelevant to the agent’s tasks, including misleading, erroneous, biased, or contradictory information.

2. **Before Information Enters Memory**:  
   We use two methods to assess information before it is stored in memory:

   * **API Call**: A designed prompt, which includes descriptions of different permission levels, helps the LLM determine the correct level of access.
   * **Vector Comparison**: We compare the information against predefined permission levels using vector mappings.

3. **For Information Stored in Memory**:  
   For stored information, we ensure that each interaction is contextually designed for retrieval. Then, we apply the same method of using a prompt to assess the permission level. The main difference here is that this step is more reflective, considering the context of the information (see Camel [1], Reflexion [2]).

[1] CAMEL: Communicative Agents for “Mind” Exploration of Large Language Model Society

[2] Reflexion: Language Agents with Verbal Reinforcement Learning

Warm regards,

Authors





## Reviewer rsvN

Dear Reviewer rsvN：

We sincerely appreciate the time and effort that the reviewers have put into evaluating our manuscript. Their insightful comments and suggestions have greatly helped to improve the quality of our work.Below, we provide detailed responses to each comment, highlighting the revisions we have made to address the reviewers' concerns.

---

><font color=FireBrick>**Question 1**</font>: The overall quality of the manuscript is suboptimal, due to the numerous typos and weak conceptual clarity. A thorough revision is recommended to refine its structure, coherence, and professionalism.

- **Typos**: We have carefully reviewed and corrected all spelling errors to ensure accuracy throughout the manuscript.
- **Conceptual Clarity**: We have strengthened the explanation of key concepts, simplifying any ambiguous sections to make the arguments clearer and more coherent.
- **Structure and Coherence**: We have reorganized parts of the manuscript to improve the overall structure, ensuring better logical flow and readability.

> <font color=FireBrick>**Question 2**</font>: The authors formalize several typical interaction topologies, however, what are the actual corresponding scenarios for these interactions? Moreover, the Agent interaction scenarios proposed by the authors are not supported by concrete related work. i.e., are the authors' abstract Agent topologies representative of existing applications?

1. **Early Stage of MAS Development**  
   The development of Multi-Agent Systems (MAS) is still in its early stages. Currently, most practical interaction scenarios are centered around large-scale AI projects, such as CAMEL [1], Multi-Agent Debate [2], and LangChain, among others.

2. **Emerging Applications of MAS**  
   In addition, MAS applications are emerging across various industries. For example, ChatDev [3] simulates a technology company to develop code, and Agent Hospital [4] simulates a fully AI-driven hospital to assist patients. In these applications, agents perform specific roles and collaborate to accomplish tasks.

3. **Lack of Layered Information Management**  
   However, these systems lack a layered information management framework. This means that sensitive information from higher-level decision-makers, such as company executives or hospital administrators, may inadvertently leak through lower-level agents.

4. **Security Vulnerabilities**  
   Attackers could exploit this vulnerability, injecting harmful information into critical agents, such as those in charge of diagnosing patients in a hospital, leading to serious errors in diagnosis.

> <font color=FireBrick>**Question 3**</font>: The network topology proposed by the authors appears overly simplistic and abstract, as reflected in the relationship diagram presented in the introduction and the methodology framework(Figure 1), which involves at most a system with three interacting agents. However, in their experiments for RQ3, the authors set up systems with up to 10 agents. What was the specific MAS configuration used in these experiments?

As research on multi-agent systems (MAS) is still in its early stages, most high-citation papers involve fewer than 10 agents (cite). This is primarily due to two reasons:  

1. **Practical Requirements**: In existing MAS applications, fewer than 10 agents are typically sufficient to complete the desired tasks effectively.  
2. **Token Consumption**: Increasing the number of agents significantly results in a sharp rise in token consumption, leading to high computational overhead.

**Agent Connectivity and Topology**. In our experiments, the connectivity between agents is random, which includes various topologies such as chains, rings, and complete graphs. We hypothesize that our defense mechanism should perform consistently across different topologies, so we did not differentiate between them in our experiments. Instead, we used a random structure to represent the connections.

**Impact of Topology on Defense Performance**. We address your concern about topology in a follow-up experiment, which will show that topology does not have a significant impact on defense performance.

**Diversity in Agent Connectivity**. The diversity in agent connectivity accurately reflects the defense performance across different MAS architectures, such as Came [1]l and AutoGPT. The agents in our experiments use APIs like Llama, GPT-4, and others.

In fact, the existing Chain and Complete Graph topologies are a realistic exploration of the security of the structures used in influential MAS research, such as Camel [1] and Multi-agent Debate [2]. The only difference lies in the system prompt used for the agents.

> <font color=FireBrick>**Question 4**</font>: The abstracts are quite lengthy (especially the section introducing the TBA and MBA), could the language be further optimized to further emphasize the core motivation and core contribution of the study?

We acknowledge that the abstracts could be more concise. In response to your comment, we will revise the language to streamline the introduction of **TBA** and **MBA**, ensuring that the **core motivation** and **key contributions** of the study are highlighted more effectively and succinctly.

> <font color=FireBrick>**Question 5**</font>: There are problems with the formatting of section headings and space allocation in the author's manuscript, where the author presents extensively on Attacks in MAS, but lacks space on AgentSafe.

Thank you for your feedback.

Proposing attacks in MAS is one of our main contributions. However, as you suggested, we will revise the structure to allocate more space to the **AgentSafe** section, including more implementation details of **ThreatSieve** and **HierarCache**.

> <font color=FireBrick>**Question 6**</font>: In Table 1, the table caption refers to ‘over 10 interaction turns’, but the table content shows data from Turn 5 to Turn 50, not "over 10 interaction turns". Therefore, there is a real contradiction between the description of the title and the table content.

We appreciate your attention to this detail.The caption for **Table 1** has been revised to more accurately reflect the data presented. Instead of stating “over 10 interaction turns,” we now specify “from Turn 5 to Turn 50,” which aligns with the data shown in the table.

> <font color=FireBrick>**Question 7**</font>: The authors have designed possible attack scenarios against the four topologies (IABT, AM, II, IM), rather than attack methods, and the authors should have listed the corresponding attack methods in these four scenarios.
> <font color=FireBrick>**Question 8**</font>: The authors use abbreviations to represent attack scenarios without first defining them (IABT, AM, II, IM).

Thank you for your valuable feedback. We apologize for the confusion regarding the terminology. To clarify, the terms "IABT," "AM," "II," and "IM" **indeed refer to specific attack methods**, not attack scenarios. We mistakenly used these terms without providing their full definitions. In the revised manuscript, we will explicitly define each of these attack methods at their first occurrence, as follows:

- **IABT** stands for Information Acquisition Based on Topology attack,
- **AM** refers to Authorization Mixup attack,
- **II** denotes Information Interference attack,
- **IM** is Identity Manipulation attack.

Furthermore, we will ensure that the relationship between the attack methods and their corresponding scenarios is clearly explained, in order to avoid any misunderstandings. Thank you for bringing this to our attention.

> <font color=FireBrick>**Question 9**</font>: In Figure 4, why did the authors choose to use a radar chart instead of a line chart to represent the relationship between the Cosine Similarity Rate (CSR) and the number of agents? A line chart would be more suitable for illustrating the continuous nature of these relations.

Thank you for your suggestion.

1. **Choice of Radar Chart**: We initially chose the radar chart to represent CSR and the number of agents because it effectively highlights variations across multiple conditions. It allows us to capture different perspectives in a multi-agent system.
2. **Line Chart Consideration**: We agree that a line chart would better show the continuous nature of the relationship between CSR and the number of agents. It would clearly illustrate the trend.
3. **Revision Plan**: Based on your feedback, we will consider adding a line chart or adjusting the figure to better reflect the continuous trend in the revised manuscript.

Thank you for helping improve the clarity of our presentation.

> <font color=FireBrick>**Question 10**</font>: In Figure 5, the author did not provide the main caption for the picture.
>
> <font color=FireBrick>**Question 11**</font>: The author did not handle the image link correctly in Obs①, citing ‘Figure ??’ (page 7).

Thank you for pointing this out. We apologize for the missing main caption in Figure 5 and the incorrect reference to "Figure ??" in Obs①. We will **add the correct caption and fix the figure reference** in the revised version to ensure accuracy.We appreciate your valuable feedback and will make the necessary corrections.

> <font color=FireBrick>**Question 12**</font>: The **Case Study** is somewhat confusing and lacks representativeness. The statement, *“Compared to systems without the AgentSafe, the token count in the hierarchical memory under AgentSafe is significantly reduced,”* raises the question: Is there any direct comparison being made here?

We apologize for any confusion caused by this section. Below is a clarification:

1. **Figure Explanation**:
   The left and right figures represent token consumption under TBA and MBA attacks, respectively, both with and without AgentSafe. The comparisons are conducted within each figure.
2. **Token Consumption Reduction**:
   In the left figure, we observe that with AgentSafe, token consumption at each level is significantly reduced. This occurs because junk memory data is excluded from the conversation history provided to the API, thereby lowering the overall token usage.
3. **Improved LLM Performance**:
   By removing irrelevant or harmful information from the conversation history, the accuracy and performance of the LLM’s responses are also enhanced.

---

[1] CamelL: Communicative Agents for "Mind" Exploration of Large Language Model Society

[2] ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate

[3] ChatDev: Communicative Agents for Software Development

[4] Agent Hospital: A Simulacrum of Hospital with Evolvable Medical Agents

Sincerely,

Authors





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





## Reviewer fvtc

Dear Reviewer fvtc:

We thank you for your time and effort in reviewing our manuscript. Their valuable feedback has greatly helped improve our work. Below are the revisions made in response to each comment.

---

> <font color=FireBrick>**Weakness 1**</font>: There is limited discussion on potential performance trade-offs or computational overhead introduced by the security measures.

We have added a discussion on computational overhead. Agentsafe itself will bring additional overhead, but due to Agentsafe's unique information layering mechanism, many meaningless or even harmful information will be detected and put into junk memory, **and this part will not participate in the subsequent agent execution of tasks**. In addition, each time the information for a specific security level is sent, only a part of the memory is required as the historical conversation record of the LLM. Combining the above two points, **Agentsafe will greatly reduce the overhead of the subsequent architecture operation**, and thus improve the performance.

Considering communication $G$,  $T$ dialogue rounds, let the average cost of a piece of information be $c$. For an agent, the original input information in each round is $I$. After the **ThreatSieve** information hierarchical judgment, the remaining content is $I^{\prime}$. The cost of **ThreatSieve** is $c\left|I\right|$.

After passing through the **HierarCache** valid information detection function $D$, the remaining content is $I^{\prime}$, and the cost of $D$ is $c\left|I^{\prime}\right|\left|C\right|$. Therefore, the total cost before the information is stored in the memory is:
$$
c\left|I\right|+ c\left|I^{\prime}\right|\left|C\right|
$$
 Each round t detects the information in the memory $R$. 

There are N layers $\Gamma $ = $\{\varepsilon_1, \varepsilon_1, ...\varepsilon_N, \varepsilon_\text{junk} \}$. The cost of each detection is:
$$
c\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk} \right | \right )
$$
A total of $T^{\prime}$ detections are performed. Therefore, the total cost is:
$$
cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right) + cT^{\prime}\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk}^t \right |\right)
$$
The above is the computational overhead of the Agentsafe architecture. Next, we will calculate the computational overhead reduction brought by Agentsafe.

---

First, the amount of irrelevant or harmful information that failed to enter the memory in round $t$ is $\left | I \right | - \left | I^{\prime\prime} \right |$. This part is stored in the junk memory and will not participate in the subsequent agent task execution process. Therefore, the cost saved is:
$$
c\left(T - t \right) \left |\varepsilon_\text{junk}^t \right |
$$
Since each agent task only needs to be based on historical data that does not exceed the accepted information security level, assuming that the security level of the information at time $t$ is $k_t$, the cost saved is:
$$
c\sum_{k}^{N}\left | \varepsilon_i \right|
$$
The net cost is:
$$
\Delta = cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right) + cT^{\prime}\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk}^t \right |\right) - c\sum_{t}^{N}\left( T- t\right)\left |\varepsilon_\text{junk}^t \right | - c\sum_{t}^{T}\sum_{k_t}^{N}\left | \varepsilon_i \right|
$$
When **AgentSafe** is attacked more, there will be more contents in junk memory. Considering that $T^{\prime}$ is much smaller than $T$, the system overhead is reduced compared to normal conditions. Alternatively, when most tasks received by the agent are relatively regular (i.e., at a lower security level),a smaller amount of information enters the junk memory, which leads to more efficient memory utilization and lower system costs. Therefore, the net cost effectively balances between the increased junk memory content during attacks and the efficiency of the task execution based on the security levels of the information.



> <font color=FireBrick>**Weakness 2**</font>: There is insufficient exploration of how AgentSafe might perform in very large-scale systems with hundreds or thousands of agents.

From the perspective of traditional network research, it is indeed reasonable to consider networks as composed of a large number of nodes. Therefore, your point is valid. However, from the viewpoint of LLM-based Multi-Agent Systems (MAS), we would like to respectfully point out that:

- In current mainstream LLM-based MAS research, the number of nodes typically considered is fewer than 10.  
- Some influential works on task-solving MAS, such as:
  - **Camel [1]**, which simulates user-AI interactions, involves only 3 agents: a task refiner, an AI Instructor, and an AI User.
  - **ChatDev [2]**, used for code development, involves 5 agents: CEO, CTO, Reviewer, Programmer, and Tester.
  - The experiments of **AutoGen [3]**, which supports automatic MAS structure generation based on tasks, also consider only 3-4 agents.
  - Some other jobs can also be listed.(**BlockAgents [4]**, **AgentVerse [5]**, **AutoDefense [6]**, **MetaGPT[7]**)

Of course, we believe that as relevant research progresses, MAS will indeed face the problem of more nodes as you have considered in the future, and our work has laid a solid foundation for it. Our current work **focuses on exploring foundational mechanisms for agent safety** and does not yet address scalability in extreme cases. 

---

[1] CAMEL: Communicative Agents for “Mind” Exploration of Large Language Model Society

[2] ChatDev：Communicative Agents for Software Development

[3] AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation

[4] BlockAgents: Towards Byzantine-Robust LLM-Based Multi-Agent Coordination via Blockchain

[5] AgentVerse: Facilitating Multi-Agent Collaboration And Exploring Emergent Behaviors

[6] AutoDefense: Multi-Agent LLM Defense against Jailbreak Attacks

[7] MetaGPT: Meta Programming For A Multi-Agent Collaborative Framwork



> <font color=FireBrick>**Question 1**</font>: How does AgentSafe handle dynamic changes in agent permissions or security levels during runtime?

Given that GPT is a black-box model, adjusting its internal parameters is not feasible. As a result, the security of the model is inherently determined by its structure. This makes it particularly difficult to consider dynamic changes in LLMs.To address this challenge, we have shifted our focus to exploring different security scenarios. We believe this approach provides a more practical and reasonable solution.

> <font color=FireBrick>**Question 2**</font>: What are the specific criteria used in the detection function D(m) to assess the validity of messages?

We have an instruction library for each security level information description, which contains n validation criteria, all of which are defined in natural language.

- Instruction library and verification criteria Assume that we have an instruction library $ \mathcal{C} $ to describe the information of each security level, which contains $n$ **verification criteria**. Each verification criterion $ m_i $ is a defined natural language description used to verify whether the information $m$ meets the standard. This can be expressed as: Instruction library and verification criteria: For the information description of each security level, the instruction library contains $n$ verification criteria $m_i$, and each standard $ m_i $ is a natural language description compared with the information $m$: $\mathcal{C} = \{ m_1, m_2, \ldots, m_n \} $ where $m_i$ represents the $i$th verification criterion, and $n$ is the total number of verification criteria.

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

Where $\delta(m, mi)$ is an indicator function, indicating whether the information $m$ meets the $i$th verification criterion $m_i$. $\text{Sim}(m, mi)$ is the similarity between the information $m$ and the verification criterion $m_i$ (which can be cosine similarity, Euclidean distance, etc.). $\theta$ is the similarity threshold. When the similarity is higher than the threshold, the information is considered to meet the verification criterion.

> <font color=FireBrick>**Question 3**</font>: How does the periodic detection mechanism (R(vj,t)) determine which information is false and should be moved to junk memory?

We appreciate your feedback and would like to clarify the process:

- **Method for Permission Level Judgment**:  
  We use two methods to determine permission levels. The first method calls an API and uses a carefully designed prompt with descriptions of different permission levels. This allows the LLM to assess the correct level of access. The key difference between this and previous API calls is that this step is more reflective in nature.

Define the process of calling the API to let LLM realize the reflection of information as $R$, and the involved LLM is set to $l$, which can be expressed by the formula:
$$
R(v_j, t) = l(\rho^t)
$$


Where $\rho$ represents prompt, $\mathcal{C}$ represents the instruction library, and $M_{\text{junk}}^t$ represents the information in the junk memory at time $t$.

The difference between this and the previous step of calling the API to determine information is that this step is more reflective (Camel [1], Reflexion [2]). We designed a more reasonable prompt $\rho$, which can be expressed as the following formula:
$$
\rho^t = \{ \text{reflextion},  C, M_{\text{junk}}^t\}
$$
Reflextion is a text prompt that encourages LLM to reflect on the information.

Based on the above, the junk information detection process is as follows:


$$
F_l = \{ m \mid R(v_j, t) = \text{"junk"} \}
$$
After time t, update $\rho$, $\rho^{t+1} = \rho^{t} + F_l$ for all identified $F_l$.



- **Experiment to Validate the Importance of This Step**:  
  To test the importance of this method, we conducted additional experiments under MBA attacks. After \( n \) rounds of interaction, we compared the impact on subsequent jailbreak attempts with and without this step.

| state/num | 1    | 2    | 3    |
| --------- | ---- | ---- | ---- |
| R         | 0.87 | 0.95 | 0.82 |
| w/o R     | 0.87 | 0.95 | 0.82 |

- **Results**:  
  The results show a slight improvement in the defense rate against jailbreaks when this step is included.

- **Future Work**:  
  We are working to supplement these findings and will add the experiment results to the appendix. If there are any updates, we will include them in the discussion phase.

---

[1]CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society

[2]Reflexion: Language Agents with Verbal Reinforcement Learning



> <font color=FireBrick>**Question 4**</font>: How might AgentSafe be adapted or extended to handle emerging attack vectors that may not fit into the current TBA or MBA categories?

We agree that emerging attack vectors may not fit neatly into the existing TBA or MBA categories. Here’s how we plan to adapt AgentSafe to handle these new threats:

- **Dynamic Threat Detection and Adaptation**: We propose integrating machine learning capabilities into AgentSafe, enabling it to identify and respond to new attack patterns in real time. This would allow the system to adapt to novel attack types without requiring a complete overhaul.
- **Expanding Attack Categories**: In addition to TBA and MBA, we plan to introduce hybrid attack categories to address more complex threats that combine elements of both topology and memory attacks. 
- **Collaborative Defense and Continuous Learning**: We also suggest enhancing collaboration among agents, allowing them to share information and improve threat detection through continuous learning. 

Thanks and Regrads,

Authors



