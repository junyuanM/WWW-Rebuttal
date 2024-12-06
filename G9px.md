Dear Reviewer G9px:

Thank you for your thoughtful and constructive feedback. We greatly appreciate the time and effort you have invested in reviewing our work. Below, we provide responses to your concerns and suggestions, along with the revisions we have made to address them.

> <font color=FireBrick>**Weakness 1**</font>: The relevance of LLMs and MAS to Web security and privacy is unclear to me.

We believe that MAS and the Web have a clear correlation, because each node in MAS can be understood as an entity in the Web. The security challenges in multi-agent systems are similar to those in Web systems, but with the addition of collaboration and information sharing between agents. The introduction of LLM may change the form of security threats.

- **LLMs in Web Security and Privacy**: LLMs can analyze large amounts of data to detect vulnerabilities, identify threats, and provide real-time responses, making them valuable tools for enhancing Web security.
- **MAS in Web Security and Privacy**: In a Multi-Agent System, multiple agents (including LLMs) can collaborate to model attack scenarios, detect adversarial behavior, and coordinate defense mechanisms, offering new ways to manage security and privacy in decentralized Web environments.

> <font color=FireBrick>**Weakness 2**</font>: Authors omitted any discussions about the details of their system with regards to implementation. 

To make it easier for readers to understand, we have included specific details in the article including:

1. For a clearer formulation, we have adopted a more precise formal expression and included a notation table in the appendix. For example, the content in W5, as well as additional formal expressions regarding the validation function and periodic detection mechanism in HierarCache.
### Detection Function D(m)

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

### Periodic Detection Mechanism R(vj,t) 

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
  To test the importance of this method, we conducted additional experiments under MBA attacks. After \( n \) rounds of interaction, we compared the impact on TBA and MBA with and without this step.

| state/num | 1    | 2    | 3    |
| --------- | ---- | ---- | ---- |
| R         | 0.91 | 0.95 | 0.88 |
| w/o R     | 0.81 | 0.78 | 0.77 |

- **Results**:  
  The results show a slight improvement in the defense rate against jailbreaks when this step is included.


2. Additionally, we have provided a theoretical derivation of the computational overhead to demonstrate that the overhead of Agentsafe has an overall favorable impact on performance.

We have added a discussion on computational overhead. Agentsafe itself will bring additional overhead, but due to Agentsafe's unique information layering mechanism, many meaningless or even harmful information will be detected and put into junk memory, **and this part will not participate in the subsequent agent execution of tasks**. In addition, each time the information for a specific security level is sent, only a part of the memory is required as the historical conversation record of the LLM. Combining the above two points, **Agentsafe will greatly reduce the overhead of the subsequent architecture operation**, and thus improve the performance.

Considering communication $G$,  $T$ dialogue rounds, let the average cost of a piece of information be $c$. For an agent, the original input information in each round is $I$. After the **ThreatSieve** information hierarchical judgment, the remaining content is $I^{\prime}$. The cost of **ThreatSieve** is $c\left|I\right|$.

After passing through the **HierarCache** valid information detection function $D$, the remaining content is $I^{\prime}$, and the cost of $D$ is $c\left|I^{\prime}\right|\left|C\right|$. Therefore, the total cost before the information is stored in the memory is:

$$
c\left|I\right|+ c\left|I^{\prime}\right|\left|C\right|
$$

 Each round t detects the information in the memory R. 

There are N layers $\Gamma $ = $\{\varepsilon_1, \varepsilon_1, ...\varepsilon_N, \varepsilon_\text{junk} \}$. The cost of each detection is:

$$
c\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk} \right | \right )
$$

A total of $T^{\prime}$ detections are performed. Therefore, the total cost is:

$$
cT\left( \left|I\right|+ \left|I^{\prime}\right|\left|C\right| \right) + cT^{\prime}\sum_{1}^{N}\left | \varepsilon_i \right| \left ( 1 + \left |\varepsilon_\text{junk}^t \right |\right)
$$

The above is the computational overhead of the Agentsafe architecture. Next, we will calculate the computational overhead reduction brought by Agentsafe.


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

> <font color=FireBrick>**Weakness 3**</font>: I didn't find the value in most of the formal definitions in the paper.

We found it **necessary to give formula definitions** in the introduction. These concepts are common in the graph(LTH [1], Snowflake [2], GPTSwarm [3], OpenGraph [4], DHGR [5]).We believe that the expression of formulas is more convenient for subsequent work and helps readers follow our research. At the same time, we agree that the formulas in the introduction are too bloated. We have deleted the formulas, but the formulas in the main text are meaningful.


> <font color=FireBrick>**Weakness 4**</font>: The space in the paper could have been used for topics such as details of the system, or the discussion of system components that is currently in the Appendix

We have added more details in the methodology, just as in W3, and we have put the extra details in the appendix.

> <font color=FireBrick>**Weakness 5**</font>: Is ThreatSieve simply authenticating the communication channel between the agents? What does its cryptographic verification and authority validation look in practice? Is it based on an existing protocol? 

- The conversations between agents are conducted in natural language, while the content of the communication adheres to a more structured format, such as the JSON file specification.

- Research on multi-agent systems is still in its early stages. Experiments are generally carried out under the assumption that identity substitution does not occur; each agent assumes a predefined role, and the communication between agents follows a designed protocol.

- Since agents process natural language through LLMs, even if the target agent is explicitly informed via a prompt that the other party is a fake identity, there remains a risk of misjudgment during task processing. This can be attributed to the positive feedback that erroneous tokens corresponding to fake information can generate within the LLM, leading to misinterpretations.

- To mitigate this risk, in **AgentSafe**, identity is verified by extracting information about the source agent through an API call and a specific program. If an incorrect identity is detected, the associated information is immediately treated as "junk information," thus preventing successful attacks that arise from the inherent non-determinism in LLM task processing.

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

> <font color=FireBrick>**Weakness 6**</font>: How does Agentsafe characterize junk memory? How does Agentsafe identify this in practice and how are these terms defined in different contexts?

We appreciate your feedback and would like to clarify the process:

1. **Definition of Junk Information**:  
   Junk information refers to harmful content that is irrelevant to the agent’s tasks, including misleading, erroneous, biased, or contradictory information.

2. **Before Information Enters Memory**:  
   We use two methods to assess information before it is stored in memory:

   * **API Call**: A designed prompt, which includes descriptions of different permission levels, helps the LLM determine the correct level of access.
   * **Vector Comparison**: We compare the information against predefined permission levels using vector mappings.

3. **For Information Stored in Memory**:  
   For stored information, we ensure that each interaction is contextually designed for retrieval. Then, we apply the same method of using a prompt to assess the permission level. The main difference here is that this step is more reflective, considering the context of the information (see Camel [6], Reflexion [7]).

> <font color=FireBrick>**Question 1**</font>: What is the effort involved in building Agentsafe and marking data and agent access hierarchy?

- The focus of building agentsafe is the design of the overall architecture and experiments to ensure that each part can run well. We have enriched the expression of methodology in the article.

- The construction of the dataset first determines the task. For example, the task of agentsafe includes basic information between agents and various attack statements. Then, like many articles in the existing LLM agent field [], the generation of the dataset is to first generate a large amount of information or questions through LLM, and then manually review to generate the dataset.

- Regarding the construction of the agent access hierarchy, we built the hierarchical datasets RIOH and WCEI for mas in simulating human interaction[6] and company scenarios[7]. MAS applications are emerging across various industries. For example, ChatDev [3] simulates a technology company to develop code, and Agent Hospital[9] simulates a fully AI-driven hospital to assist patients. In these applications, agents perform specific roles and collaborate to accomplish tasks. However, these systems lack a layered information management framework. This means that sensitive information from higher-level decision-makers, such as company executives or hospital administrators, may inadvertently leak through lower-level agents. Therefore, constructing a dataset that is of practical significance is crucial. Our experiments also prove that AgentSafe can provide effective defense in different fields.


[1] A Unified Lottery Ticket Hypothesis for Graph Neural Networks

[2] The Snowflake Hypothesis: Training and Powering GNN with One Node One Receptive Field

[3] GPTSwarm: Language Agents as Optimizable Graphs

[4] OpenGraph: Towards Open Graph Foundation Models

[5] Make Heterophilic Graphs Better Fit GNN: A Graph Rewiring Approach

[6] CAMEL: Communicative Agents for “Mind” Exploration of Large Language Model Society

[7] Reflexion: Language Agents with Verbal Reinforcement Learning

[8] ChatDev: Communicative Agents for Software Development

[9] Agent Hospital: A Simulacrum of Hospital with Evolvable Medical Agents

Warm regards,

Authors

