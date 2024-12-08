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

> <font color=FireBrick>**Question 1**</font>: How does AgentSafe handle dynamic changes in agent permissions or security levels during runtime?

Given that GPT is a black-box model, adjusting its internal parameters is not feasible. As a result, the security of the model is inherently determined by its structure. This makes it particularly difficult to consider dynamic changes in LLMs.To address this challenge, we have shifted our focus to **exploring different security scenarios**. We believe this approach provides a more practical and reasonable solution.

> <font color=FireBrick>**Question 2**</font>: What are the specific criteria used in the detection function D(m) to assess the validity of messages?

**Detection Function D(m)**

- **Instruction library and verification criteria**: Assume that we have an instruction library $\mathcal{C}$ to describe the information of each security level, which contains $n$ **verification criteria**. Each verification criterion $m_i$ is a defined natural language description used to verify whether the information $m$ meets the standard. This can be expressed as: $\mathcal{C} = \{ m_1, m_2, \ldots, m_n \}$ where $m_{i}$ represents the $i$ th verification criterion, and $n$ is the total number of verification criteria.

- **Similarity calculation with verification criteria**: For each information $m$, we determine whether the information meets the criteria by calculating the **vector semantic similarity** $\text{Sim}(m, m_i)$ between the information $m$ and each verification criterion $m_i$, which can be cosine similarity, Euclidean distance, etc. This can be expressed by the following formula:

$$
\delta(m, m_i) = \mathbb{I}(\text{Sim}(m, m_i) > \theta)
$$

- **Information validity judgment**: We can judge the validity of information by aggregating the compliance of all verification criteria. If all verification criteria are validated, then $D(m) = 1 $. The final information validity $D(m)$ can be expressed as:


$$
D(m) = 
\begin{cases} 
1 &amp; \text{if } \sum_{i=1}^{n} \delta(m, m_i) = n \\
0 &amp; \text{if } \sum_{i=1}^{n} \delta(m, m_i) &lt; n
\end{cases}
$$


> <font color=FireBrick>**Question 3**</font>: How does the periodic detection mechanism (R(vj,t)) determine which information is false and should be moved to junk memory?

We appreciate your feedback and would like to clarify the process:

- **Method for Permission Level Judgment**:  We call an API and use a carefully designed prompt with descriptions of different permission levels. This allows the LLM to assess the correct level of access. The key difference between this and previous API calls is that this step is more reflective in nature.

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



- **Experiment to Validate the Importance of This Step**:  
  To test the importance of this method, we conducted additional experiments under MBA attacks. After \( n \) rounds of interaction, we compared the impact on TBA and MBA with and without this step.

| state/num | 3    | 4    | 5    |
| --------- | ---- | ---- | ---- |
| R         | 0.91 | 0.95 | 0.88 |
| w/o R     | 0.81 | 0.78 | 0.77 |

- **Results**:  
  The results show a slight improvement in the defense rate against jailbreaks when this step is included.

- **Future Work**:  
  We are working to supplement these findings and will add the experiment results to the appendix. If there are any updates, we will include them in the discussion phase.
  

> <font color=FireBrick>**Question 4**</font>: How might AgentSafe be adapted or extended to handle emerging attack vectors that may not fit into the current TBA or MBA categories?

We agree that emerging attack vectors may not fit neatly into the existing TBA or MBA categories. Here’s how we plan to adapt AgentSafe to handle these new threats:

- **Dynamic Threat Detection and Adaptation**: We propose integrating machine learning capabilities into AgentSafe, enabling it to identify and respond to new attack patterns in real time. This would allow the system to adapt to novel attack types without requiring a complete overhaul.
- **Expanding Attack Categories**: In addition to TBA and MBA, we plan to introduce hybrid attack categories to address more complex threats that combine elements of both topology and memory attacks. 
- **Collaborative Defense and Continuous Learning**: We also suggest enhancing collaboration among agents, allowing them to share information and improve threat detection through continuous learning. 

---

[1] CAMEL: Communicative Agents for “Mind” Exploration of Large Language Model Society

[2] ChatDev：Communicative Agents for Software Development

[3] AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation

[4] BlockAgents: Towards Byzantine-Robust LLM-Based Multi-Agent Coordination via Blockchain

[5] AgentVerse: Facilitating Multi-Agent Collaboration And Exploring Emergent Behaviors

[6] AutoDefense: Multi-Agent LLM Defense against Jailbreak Attacks

[7] MetaGPT: Meta Programming For A Multi-Agent Collaborative Framwork

[8]Reflexion: Language Agents with Verbal Reinforcement Learning



Thanks and Regrads,

Authors
